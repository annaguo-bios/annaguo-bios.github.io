---
layout: post
title: A brief overview of von Mise expansion and Donsker class 
date: 2024-01-4 09:56:00-0400
description: a reading note (attempted summary) of Fernholz (1983) and Kosorok (2008)
tags: learning
categories: learning
giscus_comments: true
related_posts: false
toc:
  sidebar: left
---



> I was trying to figure out how von Mise expansion and the Donsker condition helps with establishing the asymptotic results of statistical estimators. For this purpose, I turned to two wonderful books "‘von Mises Calculus for Statistical Functionals" by Fernholz and "Introduction to Empirical Processes and Semiparametric Inference" by Kosorok. This post is my attempted summary of some contents from those books.



### von Mise expansion

A statistic can be considered as a function $$T(\cdot)$$ of distribution function $$F$$ as $$T(F)$$, where $$T:{\mathcal{F}}\longmapsto {\mathsf{R}}$$ is a mapping from a collection of distribution functions to the real line, and $$F\in\mathcal{F}$$. This function $$T$$ is refered to as the _statistical functional_. For example, the mean of a distribution can be considered as a statistical functional of the form
$$
\mu=T(F)=\int x dF(x)
$$


Statistical functional appears to be a elegant way for describing the features of statistical distributions and shred light on estimation as well. Replacing $$F$$ with its _empirical distribution function_ $$F_n$$ yields a natural estimator $$T(F_n)$$ for $$\mu$$. Given an i.i.d sample $$(X_1,X_2,\cdots, X_n)$$ of $$F$$. $$F_n$$ is defined as 
$$
\begin{align}
\mathbb{F}_n(t)=n^{-1} \sum_{i=1}^n \mathbb{I}\left\{X_i \leq t\right\}
\end{align}
$$


It turns out $$T(F_n)=n^{-1}\sum_{i=1}^n Xᵢ$$ is the sample mean.

Studing theoretical properties of statistical functionals yields powerful tools for establishing the asymptotic behavior of statistical estimators. To start, let's fisrt formally define _statistcal functionals_

> Let $$X_1, X_2,\cdots, X_n$$ be an i.i.d sample from a population with distribution function $$F$$. Let $$T_n=T_n(X_1,X_2,\cdots,X_n)$$ be a statistic. If $$T_n$$ can be written as a function of the empirical distribution function $$F_n$$ as $$T_n=T(F_n)$$, where $$T$$ does not depend on $$n$$, then $$T$$ is called a **statistical functional**.

A statistical function $$T$$ is called a *linear* statistical functional if it takes the form
$$
\begin{align}
T(F)=\int \phi(x) d F(x)
\end{align}
$$


for some real valued function $$\phi(\cdot)$$.

R. von Mises  (1947) developed a theory for the analysis of the asymptotic distribution of statistical functionals using a form of Taylor expansion involving the derivatives of the functionals. In particular, he observed that a statistic $$T(F_n)$$ can be re-expressed as
$$
\begin{align}
T\left(F_n\right)=T(F)+T_F^{\prime}\left(F_n-F\right)+\operatorname{Rem}\left(F_n-F\right)
\end{align}
$$


Here $$T^{\prime}_F$$ is the derivative of functional $$T$$ at $$F$$. $$\operatorname{Rem}\left(F_n-F\right)$$ is the remainder term. As will be discussed shortly, $$T_F^{\prime}\left(F_n-F\right)$$ is a linear statistical functional. Therefore, the central limit theorem (CLT) implies that for some finite $$\sigma²>0$$
$$
\begin{align}
\sqrt{n} \mathrm{~T}_{\mathrm{F}}^{\prime}\left(\mathrm{F}_{\mathrm{n}}-\mathrm{F}\right) \stackrel{D}{\longrightarrow} \mathrm{N}\left(0,\sigma^2\right),
\end{align}
$$


where $$\stackrel{D}{\longrightarrow}$$ refers to convergence in distribution.

If it is further true (under some conditions)
$$
\begin{align}
\sqrt{n} \operatorname{Rem}\left(\mathrm{F}_{\mathrm{n}}-\mathrm{F}\right) \xrightarrow{P} 0 ,
\end{align}
$$


where $$\xrightarrow{P}$$ refers to convergence in probability, then by [Slutsky's theorem](https://en.wikipedia.org/wiki/Slutsky%27s_theorem)
$$
\begin{align}
\sqrt{n}\left(T\left(F_n\right)-T(F)\right) \stackrel{D}{\longrightarrow} N\left(0, \sigma^2\right).
\end{align}
$$


The central idea of von Mise expansion is to approximate non-linear statistical functionals by linear functionals, and therefore offering a mean for establishing asymptotic properties.

Two questions arise (1) how is the functional derivative $$T^{\prime}_F$$ defined, and how is the von Mise expansion established? (2) what conditions are needed to ensure the convergence of the remainder term? To the first question, let's first define von Mises derivative

> Let $$X_1, X_2,\cdots X_n$$ be an i.i.d sample. Let $$T$$ be a statistical functional defined on a convex set $$\mathcal{F}$$ of distribution functions, which contains all empricial distribution functions and the population distribution function $$F$$. Let $$G\in\mathcal{F}$$. Then the **von Mises derivative** $$T^{\prime}_F$$ of $$T$$ at $$F$$ is defined by
> $$
> \begin{align}
> T_F^{\prime}(G-F)=\left.\frac{d}{d t} T(F+t(G-F))\right|_{t=0}
> \end{align}
> $$
> 
>
> if there exists a real valued function $$\phi_F$$ (independent of $$G$$) such that
> $$
> \begin{align}
> T_F^{\prime}(G-F)=\int \phi_F(x) d(G-F)(x) .
> \end{align}
> $$



Higher order derivatives can be defined in a similar manner. The function $$\phi_F$$ is uniquely defined up to an additive constant since $$d(G-F)$$ has total measure zero. We shall normalize by making 
$$
\begin{align}
\int \phi_F(x) d F(x)=0 .\label{eq:ic}
\end{align}
$$


Taking $$G=\delta_x$$, where $$\delta_x$$ is the distribution function taking point mass one at $$x$$, the corresponding $$\phi_F$$ is defined as the **influence function** or **influence curve** of $$T$$ at $$F$$. It is usually denoted as
$$
\begin{align}
I C(x ; F, T)=\phi_F(x).
\end{align}
$$


This function has played an important role in the theory of robust estimation, due to work by F. Hampel (1968, 1974) who observed that for large $$n$$, $$\phi_F(x)$$ measures the effect on $$T_n$$ of a single additional observation with value $$x$$. The influence curve also provides asymptotic variance when $$T_n$$ is asymptotically normal. The properties of the influence curve are discussed at length in Hampel (1974). 



von Mise expansion is developed by applying Taylor expansion to the following function 
$$
\begin{align}
A(t)=T(F+t(G-F)),\quad t\in[0,1].
\end{align}
$$


Taylor expansion around $$t=0$$ yields
$$
\begin{align}
A(t)=A(0)+A^{\prime}(0) t+\ldots+\frac{A^{(k)}(0) t^k}{k !}+\operatorname{Rem}_k
\end{align}
$$


Adopting first order expansion and evalute $$A(t)$$ at $$t=1$$ gives
$$
\begin{align}
A(1)=T(G)=T(F)+T_F^{\prime}(G-F)+\operatorname{Rem}(G-F)
\end{align}
$$



For $$G=F_n$$, the expansion becomes
$$
\begin{align}\label{eq:expansion}
T(F_n)&=T(F)+T^{\prime}(F_n-F)+\operatorname{Rem}(F_n-F)\\\notag
&=T(F)+\int\phi_F(x)d(F_n-F)(x)+\operatorname{Rem}(F_n-F)\\
&=T(F)+\int\phi_F(x)dF_n(x)+\operatorname{Rem}(F_n-F).\notag
\end{align}
$$


The second term in the expansion is linear
$$
\begin{align}
\int\phi_F(x)dF_n(x)=n^{-1}\sum_{i=1}^n \phi_F(X_i).
\end{align}
$$


If $$E[\phi^2_F]=\sigma²<\infty$$, and 
$$
\begin{align}
\sqrt{n} \operatorname{Rem}\left(\mathrm{F}_{\mathrm{n}}-\mathrm{F}\right) \stackrel{\mathrm{P}}{\longrightarrow} 0,\label{eq:rem}
\end{align}
$$


the CLT and Slutsky's theorem imply that
$$
\begin{align}
\sqrt{n}\left(T\left(F_n\right)-T(F)\right) \stackrel{D}{\longrightarrow} N\left(0, \sigma^2\right),\qquad\text{as}\quad n\rightarrow\infty.
\end{align}
$$


Proceed to the second question about conditions required for \eqref{eq:rem}. Note that first-order differentiability of a statistical function $T$ in the sense of von Mise derivative does not guarantee \eqref{eq:rem} is satisfied. To ensure \eqref{eq:rem}, von Mises assumed the second-order differentiability of $$T$$, which is too restrictive a condition for many functionals to satisfy. Therefore, stronger forms of differentiation, such as Frechet derivative and Hadamard (or compact) derivative, are introduced, under which it can usually be shown that \eqref{eq:rem} holds with the existence of the first derivative alone. 

Let's first establish a general form of differentiation and then restrict this to the form we wish to use. 

> Let $$V$$ and $$W$$ be topological vector spaces and let $$L(V,W)$$ be the set of continuous linear transformations from $$V$$ to $$W$$. Let $$\mathcal{S}$$ be a class of subsets of $$V$$ such that every subset consisting of a single point belongs to $$\mathcal{S}$$ and let $$\mathcal{A}$$ be an open subset of $$V$$. A function $$T:\mathcal{A}\rightarrow W$$ is **$$\mathcal{S}$$-differentiable** at $$F\in\mathcal{A}$$ if there exists $$T^{\prime}_F\in L(V,W)$$ such that for any $$K\in\mathcal{S}$$ 
> $$
> \begin{align}
> \lim _{t \rightarrow 0} \frac{T(F+t H)-T(F)-T_F^{\prime}(t H)}{t}=0
> \end{align}
> $$
> 
>
> uniformly for any $$H\in K$$. The linear function $$T^{\prime}_F$$ is called the $$\mathcal{S}$$-derivative of $$T$$ at $$F$$. 



Here we are interested in three particular types of differentiation: 

1. $$\mathcal{S}$$ = {bounded subsets of $$V$$}; this corresponds to _Frechet_ differentiation. 
2. $$\mathcal{S}$$ = {compact subsets of $$V$$}; this corresponds to _Hadamard_ differentiation. 
3. $$\mathcal{S}$$ = {single point subsets of $$V$$}; this corresponds to _Gateaux_ differentiation. 



From the definition, it immediately follows that Frechet differentiability implies Hadamard differentiability, which in turn implies Gateaux differentiability. The latter bears a close resemblance to von Mises differentiability. For a detailed discussion of functional differentiability and its implications on asymptotic properties, please refer to Fernholz (1983).



### Glivenko-Cantelli class and Donsker class

The last section has been discussing estimate $$T(F)$$ with estimator $$T(F_n)$$.  The empirical distribution function has the following properties (according to [law of large numbers](https://en.wikipedia.org/wiki/Law_of_large_numbers) and central limit theorem respectively)
$$
\begin{align}
\mathbb{F}_n(x) \stackrel{\text { as }}{\rightarrow} F(x),\qquad x\in\mathbb{R}.\label{eq:f_as}
\end{align}
$$


and
$$
\begin{align}
G_n(x) \equiv \sqrt{n}\left[\mathbb{F}_n(x)-F(x)\right] \stackrel{D}{\longrightarrow} G(x),\qquad x\in\mathbb{R}.\label{eq:f_d}
\end{align}
$$


where $$G(x)$$ is some mean-zero normal random variable. Approximate $$F$$ empirically brings the benefit that the second term (linear term) at the expansion \eqref{eq:expansion} converges in distribution to a mean-zero normal distribution. However, empirically estimate the distribution function can sometimes suffers from curse of dimensionality, which limits its usage in some application settings. 



Now the question is what if I want to estimate a probability distribution $$P$$ using other estimators, say $$\hat{Q}$$ . What asymptotic properties do the estimator $$T(\hat{Q})$$ have? What conditions are needed to ensure $$T(\hat{Q})$$ that makes a efficient use of the data? Given those questions, this section focuses on extending the idea of convergence from random variables to functions. Glivenko-Cantelli results extend the law of large numbers and Donsker results extend the central limit theorem.



First let's introduce the definition of *stochastic process* and _empirical process_.



> A *stochastic process* is a collection of random variables $$\left\{X_t, t \in T\right\}$$ on the same probability space, indexed by an arbitrary index set $$T$$. An *empirical process* is a stochastic process based on a random sample.



Let $$X_1,X_2,\cdots, X_n$$ be independent draws from a distribution function $$P$$. Let $$\mathbb{P}_n=n^{-1} \sum_{i=1}^n \delta_{X_i}$$, where $$\delta_x$$ assigns mass 1 at x and 0 elsewhere. For a measurable function $$f: \mathcal{X} \mapsto \mathbb{R}$$, let $$\mathbb{P}_n f=n^{-1} \sum_{i=1}^n f\left(X_i\right) .$$ For any class $$\mathcal{F}$$ of measurable functions $$f: \mathcal{X} \mapsto \mathbb{R}$$, we can define an empirical process $$\left\{\mathbb{P}_n f, f \in \mathcal{F}\right\}$$. As a special case, let $$\mathcal{F}=\{\mathbb{I}\{x \leq t\}, t \in \mathbb{R}\}$$, $$F_n$$ can be re-expressed as an empirical process $$\left\{\mathbb{P}_n f, f \in \mathcal{F}\right\}$$. 

$$F_n$$ and $$\mathbb{P}_n f$$ can be considered as random functions (functions of random variables) over the associated index set.  Each realization of one of these random functions is a *sample path*.  To this end, Glivenko (1933) and Cantelli (1933) demonstrated that \eqref{eq:f_as} could be strengthened to
$$
\begin{align}
\sup _{t \in \mathbb{R}}\left|\mathbb{F}_n(t)-F(t)\right| \stackrel{\text { as }}{\rightarrow} 0 .
\end{align}
$$
Another way of saying this is that the sample paths of $$F_n$$ get uniformly closer to $$F$$ as $$n\rightarrow \infty$$. More generally, a class $$\mathcal{F}$$ of measurable functions $$f: \mathcal{X} \mapsto \mathbb{R}$$ is said to be a **P-Glivenko-Cantelli** class if
$$
\begin{align}
\sup _{f \in \mathcal{F}}\left|\mathbb{P}_n f-P f\right| \stackrel{\text { as* }}{\longrightarrow} 0.
\end{align}
$$
Returning to $$F_n$$, we know by central limit theorem that \eqref{eq:f_d} holds. Donsker (1952) generalized this convergence result from vector of random variables to sample paths of stochastic processes. A class $$\mathcal{F}$$ of measurable functions $$f: \mathcal{X} \mapsto \mathbb{R}$$ is called a **P-Donsker** class if 
$$
\begin{align}
\left\{\sqrt{n}\left(P_n-P\right) f\right\}_{f \in \mathcal{F}}\stackrel{D} {\longrightarrow} G,
\end{align}
$$


where $$G$$ is a gaussian process indexed by $$\mathcal{F}$$. In particular,
$$
\begin{align}
\left(\sqrt{n}\left(P_n-P\right) f_1, \cdots, \sqrt{n}\left(P_n-P\right) f_k\right) \rightarrow\left(G_{f_1}, \ldots, G_{f_k}\right)
\end{align}
$$
 and
$$
\begin{align}
\operatorname{cov}\left(G_{f_i}, G_{f_j}\right)=\operatorname{cov}\left(f_i(X), f_j(X)\right),
\end{align}
$$
 for $$fᵢ\in \mathcal{F}, i=1,2,\cdots, k$$ and $$X\sim P$$.



To introduce the theorems establishing conditions needed for $$\mathcal{F}$$ to be a P-Glivenko-Cantelli class or Donsker class, let's first introduce some definitions.

For $$1\geq r<\infty$$, let $$L_r(P)$$ denotes the collection of functions $$g: \mathcal{X} \mapsto \mathbb{R}$$ such that 
$$
\|g\|_{r, P} \equiv\left[\int_{\mathcal{X}}|g(x)|^r d P(x)\right]^{1 / r}<\infty .
$$


- A  $$\epsilon$$-bracket in $$L_r(P)$$ is a pair of functions $$l, u \in L_r(P)$$ with $$P(l(X)<u(X))=1$$ and $$\|l-u\|_{r, P} \leq \epsilon$$.
- A function $$f\in\mathcal{F}$$ lies in the bracket $$l,u$$ if $$\mathrm{P}\{l(X) \leq f(X) \leq u(X)\}=1$$.
- The bracketing number $$N_{[]}\left(\epsilon, \mathcal{F}, L_r(P)\right)$$ is the minimum number of $$\epsilon$$-brackets needed to ensure that every $$f\in\mathcal{F}$$ lies in at least one of the brackets.
- The bracketing integral is defined as

$$
\begin{align}
J_{[]}\left(\delta, \mathcal{F}, L_r(P)\right) \equiv \int_0^\delta \sqrt{\log N_{[]}\left(\epsilon, \mathcal{F}, L_r(P)\right)} d \epsilon.
\end{align}
$$

**Theorem 1** Let $$\mathcal{F}$$ be a class of measureable functions. $$\mathcal{F}$$ is P-Glivenko-Cantelli if $$N_{[]}\left(\epsilon, \mathcal{F}, L_r(P)\right)<\infty$$ for every $$\epsilon>0$$.



**Theorem 2** Let $$\mathcal{F}$$ be a class of measureable functions. $$\mathcal{F}$$ is P-Donsker if $$J_{[]}\left(\delta, \mathcal{F}, L_2(P)\right)<\infty.$$



Now returning to our initial question, if I'd like to estimate $$P$$ by $$\hat{Q}$$, the von Mise expansion of $$T(P)$$ at $$\hat{Q}$$ yields
$$
\begin{align}
T(P)=T(\hat{Q})+\int\phi_{\hat{Q}}dP+\operatorname{Rem}(P-\hat{Q}).
\end{align}
$$


Let $$Pf\equiv\int f dP$$, we can rewrite it as
$$
\begin{align}
T(\hat{Q})-T(P)= P_n \phi_P-P_n \phi_{\hat{Q}}+\left(P_n-P\right)\{\phi_{\hat{Q}}-\phi_{P}\}+\operatorname{Rem}(\hat{Q}-P).
\end{align}
$$


Here $$\phi_P$$ is introduced using the property of the influence curve as in \eqref{eq:ic}. The first term ($$P_n \phi_P$$) is a sample average of mean zero i.i.d. terms and thus enjoys standard $$n^{1/2}$$ asymptotic behavior.  The third term converges to a gaussian process if $$\{\phi_{\hat{Q}}-\phi_{P}\}$$ is P-Donsker. If the second term ($$P_n \phi_{\hat{Q}}$$) also exihibts $$n^{1/2}$$ asymptotic behavior (as is the case with [TMLE estimators](https://link.springer.com/book/10.1007/978-1-4419-9782-1) ), then the estimator $$T(\hat{Q})$$ has only a second-order bias quantified by the remainder term $$\operatorname{Rem}(\hat{Q}-P).$$





#### References

1. Fernholz, Luisa Turrin. *Von Mises calculus for statistical functionals*. Vol. 19. Springer Science & Business Media, 2012.
2. Kosorok, Michael R. *Introduction to empirical processes and semiparametric inference*. Vol. 61. New York: Springer, 2008.
3. Lecture notes of [STAT 620](https://zhangxiany-tamu.github.io/downloads/620sp22/Apr-14.pdf) by Xianyang Zhang.
4. Lecture notes of [STAT 512: Statistical Inference](https://faculty.washington.edu/yenchic/20A_stat512/Lec10_functional.pdf) by Yen-Chi Chen.
