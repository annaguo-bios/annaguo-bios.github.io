---
layout: page
title: Criss-cross missing data model
description: Identifiability and Estimation under Missing Not at Random Mechanisms
img: assets/img/criss.jpg
importance: 1
category: Missing data
---

Conducting valid statistical analyses is challenging in the presence of missing-not-at-random (MNAR) data, where the missingness mechanism is dependent on the missing values themselves even conditioned on the observed data. Here, we consider a MNAR model that generalizes several prior popular MNAR models in two ways: first, it is less restrictive in terms of statistical independence assumptions imposed on the underlying joint data distribution, and second, it allows for all variables in the observed sample to have missing values. This MNAR model corresponds to a so-called <b>criss-cross</b> structure considered in the literature on graphical models of missing data that prevents nonparametric identification of the entire missing data model. Nonetheless, part of the complete-data distribution remains nonparametrically identifiable. By exploiting this fact and considering a rich class of exponential family distributions, we establish sufficient conditions for identification of the complete-data distribution as well as the entire missingness mechanism. We then propose methods for testing the independence restrictions encoded in such models using odds ratio as our parameter of interest. We adopt two semiparametric approaches for estimating the odds ratio parameter and establish the corresponding asymptotic theories: one involves maximizing a conditional likelihood with order statistics and the other uses estimating equations. The utility of our methods is illustrated via simulation studies. 