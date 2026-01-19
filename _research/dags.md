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

<p style="font-size: 12pt; width: 100%; text-align: left;">We introduce a workflow to align regressions with an estimand and the identification it requires. The workflow entails five steps: (1) declare an estimand; (2) draw a Directed Acyclic Graph (DAG); (3) classify control variables by role; (4) estimate models using DAG-consistent adjustment sets; and (5) recover the interpretable estimand. We implement the workflow in a user-friendly R package, <a href="https://cran.r-project.org/web/packages/DAGassist/index.html">DAGassist</a>. To illustrate its utility, we examine all 2024 American Political Science Review (APSR) articles using observational regressions with controls. Incorporating author feedback on our DAGs, we do not find any colliders. However, 61% of articles include non-admissible mediators or their descendants, and 21% of articles interpret them without qualifications. Removing DAG-inconsistent variables also decreases standard errors, increases coefficient magnitudes, and reduces statistical significance. Especially given APSR’s flagship status, our results suggest that drawing assumptions before conclusions with <a href="https://cran.r-project.org/web/packages/DAGassist/index.html">DAGassist</a> can improve empirical practice.</p> 

<p style="font-size: 12pt; width: 100%; text-align: left;"><img src="/images/DAGr.png" class="thumbnailsafeguards2" style="width: 95%;"></p>