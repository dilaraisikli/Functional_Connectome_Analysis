$$ \textbf{Functional Connectome Analysis for ASD vs TD Subjects} $$  
### *Fisher Z-Transformation • Bonferroni Correction • Bootstrap Confidence Intervals*

**Authors:** Dilara Isıklı, Mert Yıldız  

---

$$ \textbf{1. Overview} $$

This project analyzes resting-state fMRI functional connectivity for:

- **12 Autism Spectrum Disorder (ASD) subjects**
- **12 Typically Developed (TD) subjects**

Using ROI-level time series, we compute:

- Correlation matrices  
- Fisher Z-transformed matrices  
- Bonferroni-corrected confidence intervals  
- True association graphs  
- Bootstrap difference graphs  

---

$$ \textbf{2. Correlation Matrices} $$

For each subject:

```r
asd_sel_rho <- lapply(asd_sel, cor)
td_sel_rho  <- lapply(td_sel,  cor)
```

Each correlation matrix is:

$$ 116 \times 116 $$

---

$$ \textbf{3. Fisher Z-Transformation} $$

To stabilize the variance, each correlation value \( r \) is transformed using:

$$
z = \frac{1}{2}\ln\left(\frac{1+r}{1-r}\right)
$$

Variance:

$$
\sigma = \frac{1}{\sqrt{n - 3}}, \quad n = 116
$$

---

$$ \textbf{4. Bonferroni-Corrected Confidence Intervals} $$

Bonferroni-adjusted alpha:

$$
\alpha_{\text{bonf}} = \frac{0.05}{\frac{n(n-1)}{2}}
$$

Confidence interval bounds:

$$
\text{CI}_{\text{low}} = z - z_{1-\alpha/2}\sigma
$$

$$
\text{CI}_{\text{upp}} = z + z_{1-\alpha/2}\sigma
$$

---

$$ \textbf{5. Edge Selection Rule} $$

Threshold:

$$
t = Q_{0.80}(|r_{ij}|)
$$

Condition for including edge \((i,j)\):

- If  
  $$
  \text{CI}_{\text{upp}} \le -t
  $$  
  or  
  $$
  \text{CI}_{\text{low}} \ge t
  $$  
  then the edge is included.

This filters for **significant and strong correlations**.

---

$$ \textbf{6. Graph Construction} $$

Adjacency matrix:

$$
G_{ij} =
\begin{cases}
r_{ij}, & \text{if edge condition is satisfied} \\
0, & \text{otherwise}
\end{cases}
$$

Graphs visualized using:

```r
graph_from_adjacency_matrix(G_matrix, mode = "upper", weighted = TRUE)
```

---

$$ \textbf{7. ASD vs TD Results} $$

### Findings:

- TD graphs contain **more edges**  
- TD subjects show **stronger correlations**  
- ASD subjects exhibit **sparser connectivity**  
- Overall, TD networks look more integrated

This aligns with known neuroimaging patterns.

---

$$ \textbf{8. Without Bonferroni Correction} $$

Using \( \alpha = 0.05 \):

- Many weak edges appear  
- Network becomes overly dense  
- Higher risk of false associations  

Thus, Bonferroni correction is essential for high-dimensional correlation matrices.

---

$$ \textbf{9. Bootstrap Difference Analysis} $$

### Standardization:

$$
X_{\text{std}} = \frac{X - \mu}{\sigma}
$$

Group mean matrices:

- \( \bar{X}_{ASD} \)
- \( \bar{X}_{TD} \)

### Bootstrap:

For \( B = 1000 \):

$$
R_b^\* = \text{cor}(\bar{X}^\*), \quad b=1,\dots,B
$$

Standard error:

$$
SE = \text{sd}(R_1^\*, \dots, R_B^\*)
$$

### Difference matrix:

$$
\Delta = R_{ASD} - R_{TD}
$$

Confidence intervals:

$$
\text{CI}_{\text{upp}} = \Delta + z_{1-\alpha/2} \cdot SE
$$

$$
\text{CI}_{\text{low}} = \Delta - z_{1-\alpha/2} \cdot SE
$$

Edges included if:

$$
\text{CI}_{\text{upp}} \le -t \quad \text{or} \quad \text{CI}_{\text{low}} \ge t
$$

---

$$ \textbf{10. Interpretation of Bootstrap Difference Graph} $$

The graph shows:

- Strong and significant ASD–TD connectivity differences  
- ASD shows reduced strength in many ROI pairs  
- TD exhibits higher functional integration  

Histogram of \( \Delta \) confirms group-level shifts.

---


$$ \textbf{11. Required R Libraries} $$

```
corrplot
igraph
ggraph
stats
```

---

$$ \textbf{12. Summary} $$

This pipeline provides:

- Corrected and reliable connectome inference  
- ASD vs TD group comparison  
- Bootstrap-validated significance  
- High-quality graph visualizations  

The results show **weaker and sparser connectivity in ASD**, consistent with scientific literature.

---
