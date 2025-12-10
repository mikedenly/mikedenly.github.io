---
title: "Aligning Estimands, Identification, and Conclusions"
permalink: /research/dags
collection: research
---

<style>
.thumbnailevbook {
    background-color: black;
    height: 275px;
    display: inline-block; 
    background-size: cover; 
    background-position: center center;
    background-repeat: no-repeat;
}
</style>

Presented at the *Causal Data Science Meeting* (with Graham Goff).

<p style="font-size: 12pt; width: 100%; text-align: left;">We introduce a simple, reproducible workflow to align regressions with an estimand and the identification it requires. The workflow entails four steps: (1) declare an estimand; (2) draw a Directed Acyclic Graph (DAG); (3) classify control variables by role; and (4) estimate models using minimal and canonical adjustment sets. We implement the workflow in a user-friendly R package, <a href="https://cran.r-project.org/web/packages/DAGassist/index.html">DAGassist</a>. To illustrate its utility, we examine all 2024 American Political Science Review (APSR) articles using observational regressions with controls. Incorporating author feedback on our DAGs, we do not find any colliders. However, 64% of articles include non-admissible mediators or their descendants, and 21% of articles interpret them without qualifications. Removing non-admissible variables also changes results: 19% of articles lose statistical significance, and average standard errors fall by 4%. These rates likely constitute lower bounds given APSR’s flagship status. In turn, drawing assumptions before conclusions with <a href="https://cran.r-project.org/web/packages/DAGassist/index.html">DAGassist</a> can improve empirical practice.</p> 

<p style="font-size: 12pt; width: 100%; text-align: left;"><img src="/images/DAGr.png" class="thumbnailsafeguards2" style="width: 95%;"></p>