---
layout: post
title: Graphoid axioms and its proof
date: 2024-01-25 11:12:00-0400
description: serves as a reminder for myself
tags: misc
categories: misc
related_posts: false
---

>Given three disjoint subsets $$X, Y, Z$$, prove that the following relations hold:



**Symmetry:** $$X \perp Y \mid Z  \implies Y \perp X \mid Z.$$

<details>
<summary>Proof</summary>
<br>
$$\begin{align} 
  X \perp Y \mid Z & \Leftrightarrow p(X, Y \mid Z)=p(X \mid Z) p(Y \mid Z) \\ & \Leftrightarrow p(Y \mid Z) p(X \mid Z)=p(Y, X \mid Z) \\ & \Leftrightarrow Y \perp X \mid Z 
\end{align}$$
</details>





**Decomposition:** $$X \perp Y, W \mid Z  \implies X \perp Y \mid Z \  \text{and} \ X \perp W \mid Z.$$

<details>
<summary>Proof</summary>
<br>
$$\begin{align} 
  X \perp Y, W \mid Z & \Leftrightarrow p(X, Y, W \mid Z)=p(X \mid Z) p(Y, W \mid Z) \\ & \Leftrightarrow p(W \mid X, Y, Z) p(X, Y \mid Z)=p(X \mid Z) p(W \mid Y, Z) p(Y \mid Z) \\ & \Rightarrow \sum_{w} p(W \mid X, Y, Z) p(X, Y \mid Z)=\sum_{w} p(W \mid Y, Z) p(X \mid Z) p(Y \mid Z) \\ & \Leftrightarrow p(X, Y \mid Z)=p(X \mid Z) p(Y \mid Z) \\ & \Leftrightarrow X \perp Y \mid Z 
  \end{align}$$
	similarly,
	$$\begin{align} 
  X \perp Y, W \mid Z & \Leftrightarrow p(X, Y, W \mid Z)=p(X \mid Z) p(Y, W \mid Z) \\ & \Leftrightarrow p(Y \mid X, W, Z) p(X, W \mid Z)=p(X \mid Z) p(W \mid Z) p(Y \mid W, Z) \\ &\Rightarrow \sum_{Y} p(Y \mid X, W, Z) p(X, W \mid Z)=\sum_{Y} p(X \mid Z) p(W \mid Z) p(Y \mid W, Z) \\ &\Leftrightarrow p(X, W \mid Z)=p(X \mid Z) p(W \mid Z)\\ & \Leftrightarrow X \perp W \mid Z 
  \end{align}$$
</details>




**Weak union:** $$X \perp Y, W \mid Z  \implies X \perp Y \mid W, Z. $$

<details>
<summary>Proof</summary>
<br>
$$\begin{align} 
  X \perp Y, W \mid Z & \Leftrightarrow p(X, Y, W \mid Z)=p(X \mid Z) p(Y, W \mid Z) \\ & \Leftrightarrow p(X, Y \mid W, Z) p(W \mid Z)=p(X \mid Z) p(Y \mid W, Z) p(W\mid Z) \\ & \Leftrightarrow p(X, Y \mid W, Z)=p(X \mid Z) p(Y \mid W, Z) \\ \text{Based on (ii)} & \Rightarrow p(X, Y \mid W, Z)=p(X \mid Z, W) p(Y \mid W, Z) \\ & \Leftrightarrow X \perp Y \mid W, Z 
  \end{align}$$
</details>




**Contraction:** $$X \perp Y \mid Z \  \text{and} \ X \perp W \mid Y, Z \implies X \perp Y, W \mid Z.$$

<details>
<summary>Proof</summary>
<br>
$$\begin{align}
	X \perp Y \mid Z \text { \& } X \perp W \mid Y, Z & \Leftrightarrow p(X, Y \mid Z)=p(X \mid Z) p(Y \mid Z)\\
	& \text{ \& } p(X, W \mid Y, Z)=p(X \mid Y, Z) p(W \mid Y, Z)\\ & \begin{aligned}
	    \Rightarrow p(X, Y, W \mid Z)&=p(X, W \mid Y, Z) P(Y \mid Z)\\ & =p(X \mid Y, Z) p(W \mid Y, Z) p(Y \mid Z)\\& =p(X \mid Z) p(W, Y|Z)
	\end{aligned}
	\\ & \Leftrightarrow X \perp W, Y \mid Z
	\end{align}
	$$
</details>




**Intersection:** $$X \perp Y \mid W, Z \  \text{and} \ X \perp W \mid Y, Z \implies X \perp Y, W \mid Z.$$  (Holds under strictly positive distributions)

<details>
<summary>Proof</summary>
<br>
$$
	\begin{align}
	X \perp W \mid Y, Z & \Leftrightarrow p(X, W \mid Y, Z)=p(X \mid Y, Z) p(W \mid Y, Z)\\ &
	\begin{align}
	\Rightarrow p(X, W, Y \mid Z)& =p(X \mid Y, Z) p(W \mid Y, Z) p(Y \mid Z)\\
	&=p(X\mid Y,Z)p(W,Y\mid Z) \overset{\Delta}{=} p(X\mid Z)p(W,Y\mid Z)\Rightarrow X \perp W, Y\mid Z
	\end{align}
	\end{align}
	$$
	the $\Delta$ holds since $$\begin{align}
	&p(X \mid W, Z, Y)=p(X \mid Y, Z)=p(X \mid W, Z)\\
	& \Leftrightarrow \frac{p(X, Y \mid Z)}{p(Y \mid Z)}=\frac{p(X, W \mid Z)}{p(W \mid Z)}\\
    & \Rightarrow \sum_W p(X, Y \mid Z)p(W \mid Z)=\sum_W p(X, W \mid Z)p(Y \mid Z)\\
	& \Leftrightarrow p(X, Y \mid Z)=p(X \mid Z) p(Y \mid Z)\\
	& \Leftrightarrow X \perp Y \mid Z
	\end{align}$$
</details>
