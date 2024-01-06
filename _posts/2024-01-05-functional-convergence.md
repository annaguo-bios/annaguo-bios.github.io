---
layout: post
title: A brief overview of von Mise expansion and Donsker class 
date: 2023-09-1 09:56:00-0400
description: a reading note (attempted summary) of Fernholz (1983) and Kosorok (2006)
tags: learning
categories: learning
giscus_comments: true
related_posts: false
toc:
  sidebar: left
---



> I was trying to figure out how von Mise expansion and the Donsker condition helps with establishing the asymptotic results of statistical estimators. For this purpose, I turned to two wonderful books "‘von Mises Calculus for Statistical Functionals" by Fernholz and "Introduction to Empirical Processes and Semiparametric Inference" by Kosorok. This post is my attempted summary of some contents from those books.



## von Mise expansion

A statistic can be considered as a function $T(\cdot)$ of distribution function $F$ as $T(F)$, where $T:{\mathcal{F}}\longmapsto {\mathsf{R}}$ is a mapping from a collection of distribution functions to the real line, and $F\in\mathcal{F}$. This function $T$ is refered to as the _statistical functional_. For example, the mean of a distribution can be considered as a statistical functional of the form
$$
\begin{align}

\mu=T(F)=\int x dF(x)

\end{align}
$$
Statistical functional appears to be a elegant way for describing the features of statistical distributions and shred light on estimation as well. Replacing $F$ with its _empirical distribution function_ $F_n$ yields a natural estimator $T(F_n)$ for $\mu$. Given an i.i.d sample $(X_1,X_2,\cdots, X_n)$ of $F$. $F_n$ is defined as 
$$
\begin{align}
\mathbb{F}_n(t)=n^{-1} \sum_{i=1}^n \mathbb{I}\left\{X_i \leq t\right\}
\end{align}
$$
It turns out $T(F_n)=n^{-1}\sum_{i=1}^n Xᵢ$ is the sample mean.

Studing theoretical properties of statistical functionals yields powerful tools for establishing the asymptotic behavior of statistical estimators. To start, let's fisrt formally define _statistcal functionals_

> Let $X_1, X_2,\cdots, X_n$ be an i.i.d sample from a population with distribution function $F$. Let $T_n=T_n(X_1,X_2,\cdots,X_n)$ be a statistic. If $T_n$ can be written as a function of the empirical distribution function $F_n$ as $T_n=T(F_n)$, where $T$ does not depend on $n$, then $T$ is called a **statistical functional**.

A statistical function $T$ is called a *linear* statistical functional if it takes the form
$$
\begin{align}
T(F)=\int \phi(x) d F(x)
\end{align}
$$
for some real valued function $\phi(\cdot)$.

R. von Mises  (1947) developed a theory for the analysis of the asymptotic distribution of statistical functionals using a form of Taylor expansion involving the derivatives of the functionals. In particular, he observed that a statistic $T(F_n)$ can be re-expressed as
$$
\begin{align}
T\left(F_n\right)=T(F)+T_F^{\prime}\left(F_n-F\right)+\operatorname{Rem}\left(F_n-F\right)
\end{align}
$$
Here $T^{\prime}_F$ is the derivative of functional $T$ at $F$. $\operatorname{Rem}\left(F_n-F\right)$ is the remainder term. As will be discussed shortly, $T_F^{\prime}\left(F_n-F\right)$ is a linear statistical functional. Therefore, the central limit theorem (CLT) implies that for some finite $\sigma²>0$
$$
\begin{align}
\sqrt{n} \mathrm{~T}_{\mathrm{F}}^{\prime}\left(\mathrm{F}_{\mathrm{n}}-\mathrm{F}\right) \stackrel{D}{\longrightarrow} \mathrm{N}\left(0,\sigma^2\right),
\end{align}
$$
where $\stackrel{D}{\longrightarrow}$ refers to convergence in distribution.

If it is further true (under some conditions)
$$
\begin{align}
\sqrt{n} \operatorname{Rem}\left(\mathrm{F}_{\mathrm{n}}-\mathrm{F}\right) \xrightarrow{P} 0 ,
\end{align}
$$
then by [Slutsky's theorem](https://en.wikipedia.org/wiki/Slutsky%27s_theorem)
$$
\begin{align}
\sqrt{n}\left(T\left(F_n\right)-T(F)\right) \stackrel{D}{\longrightarrow} N\left(0, \sigma^2\right).
\end{align}
$$
The central idea of von Mise expansion is to approximate non-linear statistical functionals by linear functionals, and therefore offering a mean for establishing asymptotic properties.

Two questions arise (1) how is the functional derivative $T^{\prime}_F$ defined, and how is the von Mise expansion established? (2) what conditions are needed to ensure the convergence of the remainder term? To the first question, let's first define von Mises derivative

> Let $X_1, X_2,\cdots X_n$ be an i.i.d sample. Let $T$ be a statistical functional defined on a convex set $\mathcal{F}$ of distribution functions, which contains all empricial distribution functions and the population distribution function $F$. Let $G\in\mathcal{F}$. Then the von Mises derivative $T^{\prime}_F$ of $T$ at $F$ is defined by
> $$
> T_F^{\prime}(G-F)=\left.\frac{d}{d t} T(F+t(G-F))\right|_{t=0}
> $$
> if there exists a real valued function $\phi_F$ (independent of $G$) such that
> $$
> T_F^{\prime}(G-F)=\int \phi_F(x) d(G-F)(x) .
> $$

Higher order derivatives can be defined in a similar manner. The function $\phi_F$ is uniquely defined up to an additive constant since $d(G-F)$ has total measure zero. We shall normalize by making 
$$
\int \phi_F(x) d F(x)=0 .
$$
Taking $G=\delta_x$, where $\delta_x$ is the distribution function taking point mass one at $x$, the corresponding $\phi_F$ is defined as the **influence function** or **influence curve** of $T$ at $F$. It is usually denoted as
$$
I C(x ; F, T)=\phi_F(x).
$$
This function has played an important role in the theory of robust estimation, due to work by F. Hampel (1968, 1974) who observed that for large $n$, $\phi_F(x)$ measures the effect on $T_n$ of a single additional observation with value $x$. The influence curve also provides asymptotic variance when $T_n$ is asymptotically normal. The properties of the influence curve are discussed at length in Hampel (1974). 



von Mise expansion is developed by applying Taylor expansion to the following function 
$$
A(t)=T(F+t(G-F)),\quad t\in[0,1].
$$
Taylor expansion around $t=0$ yields
$$
A(t)=A(0)+A^{\prime}(0) t+\ldots+\frac{A^{(k)}(0) t^k}{k !}+\operatorname{Rem}_k
$$
Adopting first order expansion and evalute $A(t)$ at $t=1$ gives
$$
A(1)=T(G)=T(F)+T_F^{\prime}(G-F)+\operatorname{Rem}(G-F)
$$
For $G=F_n$, the expansion becomes
$$
\begin{align*}
T(F_n)&=T(F)+T^{\prime}(F_n-F)+\operatorname{Rem}(F_n-F)\\
&=T(F)+\int\phi_F(x)d(F_n-F)(x)+\operatorname{Rem}(F_n-F)\\
&=T(F)+\int\phi_F(x)dF_n(x)+\operatorname{Rem}(F_n-F).
\end{align*}
$$
The second term in the expansion is linear
$$
\int\phi_F(x)dF_n(x)=n^{-1}\sum_{i=1}^n \phi_F(X_i).
$$
If $E[\phi^2_F]=\sigma²<\infty$, and 
$$
\begin{align}
\sqrt{n} \operatorname{Rem}\left(\mathrm{F}_{\mathrm{n}}-\mathrm{F}\right) \stackrel{\mathrm{P}}{\longrightarrow} 0,\label{eq:rem}
\end{align}
$$
 the CLT and Slutsky's theorem imply that
$$
\sqrt{n}\left(T\left(F_n\right)-T(F)\right) \stackrel{D}{\longrightarrow} N\left(0, \sigma^2\right),\qquad\text{as}\quad n\rightarrow\infty.
$$
Proceed to the second question about conditions required for $\eqref{eq:rem}$. Note that first-order differentiability of a statistical function $T$ in the sense of von Mise derivative does not guarantee $\eqref{eq:rem}$ is satisfied. To ensure $\eqref{eq:rem}$, von Mises assumed the second-order differentiability of $T$, which is too restrictive a condition for many functionals to satisfy. Therefore, stronger forms of differentiation, such as Frechet derivative and Hadamard (or compact) derivative, are introduced, under which it can usually be shown that $\eqref{eq:rem}$ holds with the existence of the first derivative alone. 

Let's first establish a general form of differentiation and then restrict this to the form we wish to use. 

> Let $V$ and $W$ be topological vector spaces and let $L(V,W)$ be the set of continuous linear transformations from $V$ to $W$. Let $\mathcal{S}$ be a class of subsets of $V$ such that every subset consisting of a single point belongs to $\mathcal{S}$ and let $\mathcal{A}$ be an open subset of $V$. A function $T:\mathcal{A}\rightarrow W$ is $\mathcal{S}$-differentiable at $F\in\mathcal{A}$ if there exists $T^{\prime}_F\in L(V,W)$ such that for any $K\in\mathcal{S}$ 
> $$
> \lim _{t \rightarrow 0} \frac{T(F+t H)-T(F)-T_F^{\prime}(t H)}{t}=0
> $$
> uniformly for any $H\in K$. The linear function $T^{\prime}_F$ is called the $\mathcal{S}$-derivative of $T$ at $F$. 

Here we are interested in three particular types of differentiation: 

1. $\mathcal{S}$ = {bounded subsets of $V$}; this corresponds to _Frechet_ differentiation. 
2. $\mathcal{S}$ = {compact subsets of $V$}; this corresponds to _Hadamard_ differentiation. 
3. $\mathcal{S}$ = {single point subsets of $V$}; this corresponds to _Gateaux_ differentiation. 

From the definition, it immediately follows that Frechet differentiability implies Hadamard differentiability, which in turn implies Gateaux differentiability. The latter bears a close resemblance to von Mises differentiability. For a detailed discussion of functional differentiability and its implications on asymptotic properties, please refer to Fernholz (1983).



## *Glivenko-Cantelli* condition and Donsker condition

