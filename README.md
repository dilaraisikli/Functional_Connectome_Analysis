Functional Connectome Analysis for ASD vs TD Subjects
Overview

This repository analyzes Resting-State fMRI ROI functional connectivity for:

12 Autism Spectrum Disorder (ASD) subjects

12 Typically Developed (TD) subjects

Using their ROI time series, we build correlation matrices, apply Fisher Z-transformation, perform multiple testing correction, and construct true association graphs using:

Fisher-Z confidence intervals

Bonferroni correction

Threshold-based edge selection

Bootstrap confidence intervals

The project produces:

ROI-level connectome graphs for all ASD and TD subjects

Comparison of connectivity strength

Bootstrap-based difference graphs between ASD and TD groups

Methodology
1. Correlation Matrices

For each subject:

asd_sel_rho <- lapply(asd_sel, cor)
td_sel_rho  <- lapply(td_sel,  cor)


Each correlation matrix is size:

116 × 116 (ROIs)

2. Fisher Z-Transformation

For a correlation value $r$:

z = atanh(r)


Variance:

σ = 1 / sqrt(n - 3)


with $n = 116$ ROIs.

3. Bonferroni-Corrected Confidence Intervals

We compute the 95% Fisher-Z confidence intervals:

CI_low  = z - z_{1-α/2} * σ
CI_upp  = z + z_{1-α/2} * σ


where:

α_bonf = 0.05 / [n(n-1)/2]

4. Edge Selection Rule

For each ROI pair $(i,j)$:

Let
t = 80th percentile of correlation magnitudes
(i.e., strong baseline connectivity)

An edge is included if:

CI_upp ≤ -t  OR  CI_low ≥ t


Interpretation:

Only connections that are significantly strong and not crossing zero are included.

This yields the true association graph.

5. Graph Construction

Using igraph and ggraph, each subject’s adjacency matrix is converted to a circular connectome:

graph_from_adjacency_matrix(G_matrix, mode = "upper", weighted = TRUE)


Edges are colored by:

abs(correlation strength)

Results
ASD vs TD Graph Comparison

TD subjects show more edges, meaning stronger and more stable ROI connections.

ASD subjects display sparser graphs, indicating weaker functional connectivity.

This aligns with known literature on ASD neuroconnectivity differences.

Without Bonferroni Correction

Using $\alpha = 0.05$ without correction:

Many more edges appear.

Weak correlations are falsely included.

Overestimates connectivity.

Confirms the necessity of multiple testing correction when analyzing 116×116 ROIs.

🧪 Bonus Analysis — Bootstrap Difference Graph
Standardization

Each subject’s ROI matrix is standardized:

standardized = (X - mean) / sd


Group means for ASD and TD are computed across 12 subjects each.

Bootstrap Procedure

We generate:

R_star[,,b] = correlation matrix of mean data with resampled rows
B = 1000 bootstrap samples


Bootstrap standard errors:

R_sd = apply(R_star, c(1,2), sd)

Difference Confidence Intervals

For each ROI pair:

CI_up   = ρ_ASD - ρ_TD + z_{1-α/2} * sd_diff
CI_low  = ρ_ASD - ρ_TD - z_{1-α/2} * sd_diff

Difference Graph

Edges included if:

CI_up ≤ -t  OR  CI_low ≥ t


where t is chosen from the 95th percentile of |ASD − TD| differences.

Interpretation

The bootstrap difference graph shows significant ASD–TD differences.

Strong deviations emphasize ROIs where ASD connectivity is weaker.

Summary

This project demonstrates:

✔ Full connectome construction
✔ Inferential graph analysis
✔ Use of Fisher-Z & Bonferroni
✔ Threshold-based association testing
✔ Group comparison via bootstrapping
✔ Graph visualization with ggraph

The results confirm:

TD subjects show richer, stronger connectivity.

ASD subjects exhibit reduced functional cohesion.

Bootstrap difference graphs highlight specific ROI pairs driving group differences.
