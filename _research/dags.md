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

<p style="font-size: 12pt; width: 100%; text-align: left;">Regressions with control variables underpin observational social science, but control variable selection can change the estimands and inferences that models support. In this paper, we introduce a workflow that combines Directed Acyclic Graphs (DAGs) with automated empirical estimation to recover target estimands. To illustrate its utility, we re-examine all 2024 American Political Science Review (APSR) articles using observational regressions with controls. Incorporating author feedback on our DAGs, we find no colliders. However, 61% of articles include estimand-shifting mediators or their descendants, and 21% of articles interpret them as total effects without qualification. Removing DAG-inconsistent variables also meaningfully alters standard errors, coefficient magnitudes, and statistical significance. Reweighting to recover target estimands not only yields similar patterns for coefficient magnitudes and statistical significance but also some positivity failures. Our results suggest that diagnosing assumptions before drawing conclusions with our R package, <a href="https://cran.r-project.org/web/packages/DAGassist/index.html">DAGassist</a>, can improve empirical practice.</p> 

<p style="font-size: 12pt; width: 100%; text-align: left;"><img src="/images/DAGr.png" class="thumbnailsafeguards2" style="width: 95%;"></p>