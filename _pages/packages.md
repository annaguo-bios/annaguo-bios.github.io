---
layout: page
permalink: /packages/
title: Packages
description:
nav: true
nav_order: 3
---

<style>
  .packages-intro {
    border-top: 1px solid var(--global-divider-color);
    border-bottom: 1px solid var(--global-divider-color);
    font-style: italic;
    margin: 2rem 0;
    padding: 1rem 0;
  }

  .packages-list {
    list-style: none;
    margin: 0;
    padding: 0;
  }

  .packages-list li {
    margin-bottom: 1.1rem;
  }

  .packages-list .package-name {
    font-weight: 700;
  }

  .packages-list .package-meta {
    margin-left: 0.55rem;
    white-space: nowrap;
  }

  .packages-list .package-meta + .package-meta {
    margin-left: 0.4rem;
  }
</style>

<div class="packages-intro">
  We try our best to pair each methodological paper with a practical, well-documented tool that has been tested through the paper’s experiments, so that the method can move beyond theory and support real-world practice.
</div>

<ul class="packages-list">
  <li><a class="package-name" href="{{ '/blog/2026/primal/' | relative_url }}">flexCausal</a><span class="package-meta">[<a href="https://github.com/annaguo-bios/flexCausal">github</a>]</span><span class="package-meta">[<a href="https://CRAN.R-project.org/package=flexCausal">CRAN</a>]</span>: <a href="{{ '/blog/2026/primal/' | relative_url }}">Causal Effect Estimation in Graphical Models with Unmeasured Variables</a></li>
  <li><a class="package-name" href="{{ '/blog/2026/mtree/' | relative_url }}">flexMissing</a><span class="package-meta">[<a href="https://github.com/annaguo-bios/flexMissing">github</a>]</span>: <a href="{{ '/blog/2026/mtree/' | relative_url }}">An R Package for Target/Full Law Identification and Imputation in Given Missing Data mDAGs</a></li>
  <li><a class="package-name" href="{{ '/blog/2026/frontdoor/' | relative_url }}">fdcausal</a><span class="package-meta">[<a href="https://github.com/annaguo-bios/fdcausal">github</a>]</span>: <a href="{{ '/blog/2026/frontdoor/' | relative_url }}">An R Package for Causal Effect Estimation via the Front-Door Functional</a></li>
  <li><a class="package-name" href="{{ '/blog/2026/napkin/' | relative_url }}">napkincausal</a><span class="package-meta">[<a href="https://github.com/annaguo-bios/napkincausal">github</a>]</span>: <a href="{{ '/blog/2026/napkin/' | relative_url }}">An R Package for Causal Effect Estimation in the Napkin Graph</a></li>
</ul>
