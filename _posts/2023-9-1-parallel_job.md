---
layout: post
title: Submit parallel jobs on HPC
date: 2023-09-1 09:56:00-0400
description: a hands-on tutorial
tags: code
categories: code
giscus_comments: true
related_posts: false
toc:
  sidebar: left
---



> The workflow discussed in this post is particularly suited for scenarios in which you need to execute similar code multiple times, with each execution having a substantial time requirement. In each iteration, the code exhibits minor variations in specific input parameters. Then this is when the *parallel* computing comes into play.



**Toy Task:** Suppose I'd like to numerically evaluate the central limit theorem.

<u>Central Limit Theorem</u>: suppose $$\{X_1,X_2,\dots,X_n\}$$ is a sequence of i.i.d. random variables with $$E(X_i)=\mu$$ and $$Var(X_i)=\sigma^2 <\infty$$ then as $$n\rightarrow\infty$$, $$\bar{X}\rightarrow N(\mu,\frac{\sigma^2}{n})$$



### Write a R function that perform the task

Create a R script, named `task.R` that simulate $$X$$ with my specified sample size and seed, and output the sample mean of the simulated $$X$$ values.

The following code takes two command-line arguments `n` and `seed` and output the sample mean to a Rdata file.

```R
args = commandArgs(trailingOnly=T)
n=args[1]
seed=args[2]

set.seed(seed)

# generate data
x <- rnorm(n,0,1)

# sample mean of X
E.x <- mean(x)

# save the output
save(list = c("E.x"),file = paste0("output_",n,"_",seed,".Rdata"))
```



***Note:*** How to create the *task.R* file in linux?

- Create a file named *task.R* with the following code

```shell
vim task.R
```

- type `i` to enter the insert mode, and paste the above code.
- press `Esc` to exit the insert mode, and type `:wq` to save and quit



To test the code, let $n=100$ and $seed=10$. Run the following code:

```shell
# load R
module load R

# run the job
Rscript task.R 100 10
```

It will produce a file named `output_100_10.Rdata` .



### Generate the jobs

We want to execute the `task.R` script for different sample sizes: 250, 500, 1000, 2000, and 4000. For each sample size, we intend to run `task.R` 1000 times by setting seeds from 1 to 1000.

For computational efficiency, we aim to run the $5\times 100$0 jobs in **parallel**. To achieve this, first create a `.txt` file that contains all the jobs with the following R code. 

```R
# sample size
n.vec <- c(250,500,1000,2000, 4000)

# number of simulations
nsim <- 1000

for (i in seq_along(n.vec)){
  joblist <- c()
  for (t in 1:nsim){
    job <- paste0("Rscript task.R ",n.vec[i]," ",t)
    joblist <- c(joblist,job)
  }
  write.table(joblist, file = paste0("joblist_n",i,".txt"),quote = F, col.names = F, row.names = F)
}
```

Usually, we don't want to submit too many jobs at a time, so I split the jobs into 5 files, named *joblist_n250.txt, joblist_n500.txt, joblist_n1000.txt, joblist_n2000.txt, joblist_n4000.txt*.



### Submit parallel jobs

Personally I prefer to submit parallel jobs with the help of [dSQ](https://github.com/ycrc/dSQ) command. 

- download dSQ (you will need to install git command first if it's not already installed).

```shell
# Download dSQ
git clone https://github.com/ycrc/dSQ

# Define the path to the dSQ folder
dsq_path="/path/to/my/dSQdirectory"
```

- Generate bash jobs

```shell
module load python/3.8
module load R/4.2.2
##########################
# This is step1_dsq.sh
##########################
for i in $(seq 1 5); do
python "$dsq_path/dSQ.py" --job-file joblist_n$i.txt --batch-file joblist_n$i.sh --job-name n$i --mail-type ALL
done
```

- Modify the bash jobs to contain email and partition information. Change the partition name as need. I'm using the `day-long-cpu` partition here.

```shell
##########################
# This is step2_modify.sh
##########################

for i in $(seq 1 5); do
# add "python" at the last line
sed -i 's@'"$dsq_path"'/dSQBatch.py@python '"$dsq_path"'/dSQBatch.py@' "joblist_n$i.sh"

# add email and packages
sed -i '/#SBATCH --mail-type "ALL"/a #SBATCH --mail-user=your-email@xxx.edu\nmodule load python/3.8\nmodule load R/4.2.2' joblist_n$i.sh

# specify partition name
sed -i '1a #SBATCH --partition=day-long-cpu' joblist_n$i.sh
done

```

- submit the jobs one by one, so we don't abuse the cluster:)

```shell
sbatch joblist_n250.sh
```

if you want to submit all the jobs at once:

```shell
##########################
# This is step3_submit.sh
##########################
for i in $(seq 1 5); do
  sbatch joblist_n$i.sh
done
```



### Organize the results

Finally, we are ready to organize the results. This step varies based on your task. In this toy example, we'd like to verify:
$$
\sqrt{n}(\bar{X}-0)\rightarrow N(0,1)
$$
Therefore, I'll plot the kernel density estimate of $$\bar{X}$$ for each sample size, and compare them with the standard normal distribution $$N(0,1)$$

 The following R code does the job.

```R
library(ggplot2)
library(dplyr)
library(latex2exp)

# sample size
n.vec <- c(250,500,1000,2000,4000)

# number of simulations
nsim <- 1000

# record results  
mean_matrix <- matrix(nrow = nsim, ncol = length(n.vec))

for (i in seq_along(n.vec)){
  
  # sample size
  n <- n.vec[i]
  
  for (t in 1:nsim){
 
    load(paste0("output_",n,"_",t,".Rdata"))
    
    # record bias
    mean_matrix[t,i] <- E.x
    
    }
  }

# plot the result with boxplot
dt <- data.frame(n=rep(n.vec, each=nsim), value=c(mean_matrix)) %>% mutate(n.value=sqrt(n)*value)

######################
  # density plot
######################
  
  p.den <- ggplot(dt, aes(x=n.value, color=factor(n))) + 
    geom_density(key_glyph = draw_key_path)+
    theme_bw()+
    xlab(TeX(r'($\sqrt{n}E(X)$)')) + 
    ylab("Density")+
    theme(legend.position = c(.95, .95),
          legend.justification = c("right", "top"))+
    guides(color = guide_legend(title = "Sample size"))
  

  # Overlay a single normal curve
  p.den <- p.den +
    stat_function(
      fun = dnorm,
      args = list(mean = 0, sd = 1),
      linetype = "dashed",
      color = "black",
      size = 1
    )

# Save the ggplot visualization to a PNG file
ggsave(filename = "dens_plot.png", plot = p.den, device = "png", width = 6, height = 4, dpi = 300)
```

The resulting plot:

{% include figure.html path="assets/img/dens_plot.png" class="img-fluid rounded z-depth-1" zoomable=true %}
