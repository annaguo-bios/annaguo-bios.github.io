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



# Tutorial on submitting parallel jobs on HPC

**Toy Task:** I'd like to numerically evaluate the central limit theorem.

<u>Central Limit Theorem</u>: suppose $$\{X_1,X_2,\dots,X_n\}$$ is a sequence of i.i.d. random variables with $$E(X_i)=\mu$$ and $$Var(X_i)=\sigma^2 <\infty$$ then as $n\rightarrow\infty$, $$\bar{X}\rightarrow N(\mu,\frac{\sigma^2}{n})$$



### Write a R function that perform the task

Create a R script, named `task.R` that simulate $X$ with my specified sample size and seed, and output the sample mean of the simulated $X$ values.

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
    job <- paste0("Rscript parallel_main.R ",n.vec[i]," ",t)
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
git clone https://github.com/ycrc/dSQ
```

- Generate bash jobs

You shall be able to find the `dSQ.py` file from the dSQ directory.

```shell
module load python/3.8
module load R/4.2.2
##########################
# This is step1_dsq.sh
##########################
for i in $(seq 1 5); do
python /path/to/the/file/dSQ.py --job-file joblist_n$i.txt --batch-file joblist_n$i.sh --job-name n$i --mail-type ALL
done
```

- Modify the bash jobs to contain email and partition information

```shell
##########################
# This is step2_modify.sh
##########################

for i in $(seq 1 5); do
# add "python" at the last line
sed -i 's@/path/to/the/file/dSQBatch.py@python /path/to/the/file/dSQBatch.py@' joblist_n$i.sh

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

Finally, we are ready to organize the results. This step varies based on your task. In this toy example, I'd like to plot the distribution of sample mean over different sample sizes. The following R code does the job.

```R
library(ggplot2)

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
    mean_matrix[t,i] <- bias_Y1
    
    }
  }

# plot the result with boxplot
dt <- data.frame(n=rep(n.vec, each=nsim), value=c(mean_matrix)) %>% mutate(n.value=sqrt(n)*value)
  p.box <- ggplot(dt, aes(factor(n), n.value)) + 
    geom_boxplot(position = position_dodge(width = 0.9))+ 
    theme_bw() +
    geom_hline(yintercept=0, linetype="dashed")+
    stat_summary(fun = median, geom = "path",mapping = aes(group = -1))+
    labs(x="Sample size n",y=yaxis_boxplot)

# Save the ggplot visualization to a PNG file
ggsave(file = "plot.png", plot = p, device = "png", width = 6, height = 4, dpi = 300)
```

