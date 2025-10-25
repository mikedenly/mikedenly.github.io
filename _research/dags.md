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

<p style="font-size: 12pt; width: 100%; text-align: left;"> Problems in selecting controls variables can undermine observational regressions. To diagnose the state of the art, we examine all relevant articles in the 2024 edition of the American Political Science Review (APSR) and draw Directed Acyclic Graphs (DAGs) for each main result, incorporating feedback from the original authors. We find that 56% of articles control for colliders, mediators, or descendants, and 22% of articles omit estimand or identification qualifications when interpreting these variables. Replacing them with algorithmically-derived minimal and canonical adjustment sets changes results: 30% of articles lose statistical significance, and average standard errors fall by 5%. These rates likely represent lower bounds on covariate-selection problems given APSR’s role as a flagship journal. To improve empirical practice, we develop an open-source R package, <a href="https://cran.r-project.org/web/packages/DAGassist/index.html">DAGassist</a>. It classifies variables by their role, compares full models to minimal/canonical specifications, and makes drawing assumptions before conclusions an easy part of social scientists’ workflows.</p> 

<p style="font-size: 12pt; width: 100%; text-align: left;"><img src="/images/DAGr.png" class="thumbnailsafeguards2" style="width: 80%;"></p>