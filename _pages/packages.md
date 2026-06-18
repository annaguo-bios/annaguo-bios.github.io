---
layout: page
permalink: /packages/
title: Packages
description:
nav: true
nav_order: 3
---

{% assign flexCausal = site.posts | where_exp: "post", "post.path contains '2026-06-18-primal.md'" | first %}
{% assign flexMissing = site.posts | where_exp: "post", "post.path contains '2026-06-18-mtree.md'" | first %}
{% assign fdcausal = site.posts | where_exp: "post", "post.path contains '2026-06-18-frontdoor.md'" | first %}
{% assign napkincausal = site.posts | where_exp: "post", "post.path contains '2026-06-18-napkin.md'" | first %}

<ul>
  <li><a href="{{ flexCausal.url | relative_url }}">flexCausal</a> [<a href="https://github.com/annaguo-bios/flexCausal">github</a>] [<a href="https://CRAN.R-project.org/package=flexCausal">CRAN</a>]: {{ flexCausal.title }}</li>
  <li><a href="{{ flexMissing.url | relative_url }}">flexMissing</a> [<a href="https://github.com/annaguo-bios/flexMissing">github</a>]: {{ flexMissing.title }}</li>
  <li><a href="{{ fdcausal.url | relative_url }}">fdcausal</a> [<a href="https://github.com/annaguo-bios/fdcausal">github</a>]: {{ fdcausal.title }}</li>
  <li><a href="{{ napkincausal.url | relative_url }}">napkincausal</a> [<a href="https://github.com/annaguo-bios/napkincausal">github</a>]: {{ napkincausal.title }}</li>
</ul>
