---
title: "DAGassist"
permalink: /software/dagassist
collection: software
excerpt: "An R package for DAG-informed adjustment, target estimands, and robustness checks."
---

<style>
.software-detail p {
  font-size: 12pt;
  text-align: left;
}

.software-detail .software-logo {
  display: block;
  width: 220px;
  max-width: 100%;
  height: auto;
  margin: 1.25em 0;
}
</style>

<div class="software-detail">
  <p><i>Joint work with Graham Goff.</i></p>

  <p>DAGassist is an R package that helps researchers align regression analyses
  with causal assumptions and target estimands. Given a directed acyclic graph
  and a model specification, it classifies variables by their causal roles,
  compares the original specification with DAG-derived adjustment sets, and
  supports target-estimand recovery and reporting of robustness checks.</p>

  <p>
    [<a href="https://CRAN.R-project.org/package=DAGassist">CRAN</a>]
    [<a href="https://grahamgoff.com/DAGassist/">Documentation</a>]
    [<a href="https://github.com/grahamgoff/DAGassist">Source code</a>]
    [<a href="{{ '/research/dags' | relative_url }}">Related paper</a>]
  </p>

  <img src="{{ '/images/DAGassist_logo.png' | relative_url }}"
       class="software-logo"
       alt="DAGassist logo">
</div>

## Installation

```r
install.packages("DAGassist")
```

## Citation

For the package citation, run:

```r
citation("DAGassist")
```

```
To cite package ‘DAGassist’ in publications use:

  Goff G, Denly M (2026). _DAGassist: Test Robustness with Directed Acyclic Graphs_. R
  package version 0.3.0, <https://CRAN.R-project.org/package=DAGassist>.

A BibTeX entry for LaTeX users is

  @Manual{,
    title = {DAGassist: Test Robustness with Directed Acyclic Graphs},
    author = {Graham Goff and Michael Denly},
    year = {2026},
    note = {R package version 0.3.0},
    url = {https://CRAN.R-project.org/package=DAGassist},
  }
```

