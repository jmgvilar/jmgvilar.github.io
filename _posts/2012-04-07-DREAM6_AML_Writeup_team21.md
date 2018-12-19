---
layout: post
mathjax: true
title:  "DREAM6/FlowCAP2 Molecular Classification of Acute Myeloid Leukemia Challenge"
subtitle:   "Best performer (Team team21) approach"
date:   2012-04-07 00:00:00
author:     "Jose Vilar"
background: '/img/bg-post.jpg'
comments: false
---

DREAM6/FlowCAP2 Molecular Classification of Acute Myeloid Leukemia
Challenge team21 Jose M. G. Vilar†‡ †Biophysics Unit (CSIC-UPV/EHU) and
Department of Biochemistry and Molecular Biology, University of the
Basque Country, Bilbao, Spain ‡IKERBASQUE, Basque Foundation for
Science, Bilbao, Spain

Below is a brief description of the approach of team21, consisting of
Jose M. G. Vilar, which ranked first
(http://www.the-dream-project.org/result/classification-aml) among the
best performers at the DREAM6 Molecular Classification of Acute Myeloid
Leukemia Challenge. The description of the challenge can be found at
http://www.the-dream-project.org/challenges/dream6flowcap2-molecular-classification-acute-myeloid-leukaemia-challenge.

Rationale The approach uses relative entropies to evaluate if the
distribution of the values of flow cytometry data for a given individual
is closer to the overall distribution for AML or for Normal individuals.
Relative entropies with respect to AML and Normal individuals in the
space of values $\Gamma$ are defined as
$S_{i,AML} = - \int P_{i}(\Gamma)\ln\frac{P_{i}(\Gamma)}{P_{AML}(\Gamma)}d\Gamma$
and
$S_{i,Normal} = - \int P_{i}(\Gamma)\ln\frac{P_{i}(\Gamma)}{P_{Normal}(\Gamma)}d\Gamma$,
respectively. For an AML individual, the values of the relative
entropies would be $S_{i,AML} \simeq 0$ and $S_{i,Normal} < < 0$. A
Normal individual, in contrast, is expected to lead to $S_{i,AML} < < 0$
and $S_{i,Normal} \simeq 0$. Therefore, the relative entropy difference
$\Delta S_{i} = S_{i,AML} - S_{i,Normal} = - \int P_{i}(\Gamma)\ln\frac{P_{Normal}(\Gamma)}{P_{AML}(\Gamma)}d\Gamma$
indicates that the individual looks like an AML patient for positive
values and like a Normal subject for negative values.

Implementation 1) Compute the 4-dimensional histograms
$H{(\Gamma)}_{i,j,k}$ for the values of $\Gamma$=(\"FS Log\", \"SS
Log\", \"FL3 Log\", $j$) with $j \in$\"FL1 Log\",\"FL2 Log\", \"FL4
Log\",\"FL5 Log\" for Tube \$k\$1,2,3,4,5,6,7 for all individuals $i$.
For each individual there are $7 \times 4 = 28$ histograms.

2\) Compute
$H{(\Gamma)}_{AML,j,k} = \sum_{i \in AML}^{}{H{(\Gamma)}_{i,j,k}}$ and
$H{(\Gamma)}_{Normal,j,k} = \sum_{i \in Normal}^{}{H{(\Gamma)}_{i,j,k}}$
as the overall histograms for AML and Normal individuals.

3\) Normalize the histograms to obtain the probabilities
$P{(\Gamma)}_{i,j,k}$, $P{(\Gamma)}_{AML,j,k}$, and
$P{(\Gamma)}_{Normal,j,k}$.

4\) Compute the relative entropy differences
$\Delta S_{i,j,k} = - \sum_{\Gamma}^{}{P_{i,j,k}(\Gamma)\ln\frac{P_{Normal,j,k}(\Gamma)}{P_{AML,j,k}(\Gamma)}}$.
The total relative entropy difference is defined as
$\Delta S_{i} = \sum_{j,k}^{}{\Delta S_{i,j,k}}$

5\) The likelihood that an individual $i$ is AML positive is quantified
as $L_{i} = 1/(1 + e^{- \Delta S_{i}})$.

Code execution In a directory of a Unix/OSX machine with the files
"series10createdist\_tot.py", "series10usedist\_tot.py",
"DREAM6\_AML\_TrainingSet.csv", and the directory "CSV" with the files
"0001.CSV", "0002.CSV"... execute: cat
DREAM6\_AML\_Predictions\_team21u.txt \| sort -n -k 3 \| cut -f 1,2 \>
DREAM6\_AML\_Predictions\_team21.txt
