---
title: "DAG It: Drawing Assumptions Before Conclusions Changes Results"
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

Presenting at the *Causal Data Science Meeting* (with Graham Goff).

<p style="font-size: 12pt; width: 100%; text-align: left;">The improper selection and interpretation of control variables in regression analysis is the subject of numerous methodological texts, but uptake of relevant recommendations is low in political science. To diagnose the state of the art, we examined all articles using observational regressions with control variables in the 2024 edition of the American Political Science Review (APSR). Then, we constructed Directed Acyclic Graphs (DAGs) for the main results of the articles and emailed the articles' original authors for feedback. On the basis of our final DAGs, we find that 56% of articles control for colliders, mediators, or descendants, and 22% of articles interpret unidentifiable control variables. After removing DAG-inconsistent controls and re-examining authors' main results using algorithmically-derived minimal and canonical adjustment sets, we find that 30% of articles no longer exhibit statistical significance and/or have treatments whose coefficients' signs change directions. In line with the dictum of the eminent biostatistician Miguel Hernán, from which we take this article's title, drawing assumptions before conclusions really does change results. To make checking assumptions before conclusions a routine and easy part of social scientists' workflow, we proffer a new, open-source R package, <a href="https://cran.r-project.org/web/packages/DAGassist/index.html">DAGassist</a>. It functions with all regression packages, quickly identifies controls inconsistent with the DAG by type, and compares results from full models against those with minimal and canonical adjustment sets. Given APSR's role as the flagship journal of political science, our results likely represent lower bounds on control variable selection problems, enabling <a href="https://cran.r-project.org/web/packages/DAGassist/index.html">DAGassist</a> to play a constructive part in improving empirical practice going forward.</p> 

<p style="font-size: 12pt; width: 100%; text-align: left;"><img src="/images/DAGr.png" class="thumbnailsafeguards2" style="width: 80%;"></p>