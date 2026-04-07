Full code for plots is available in final_project.ipynb

# Original Model Description

My assignment is based on an extended model of the HPA axis by Karin et al. It is a five-dimensional ODE system that takes into account functional hormone gland masses (masses of corticotrophs and of adrenal gland). Their new model successfully models the dysregulation of the HPA axis after chronic stress on longer time scales.

Instead of using units, the system's variables are defined in proportion to a healthy baseline (1).

# Extension: Big Picture Idea
I combined the 5-dimensional ODE system by Karin et al and data from experiments of rats under melatonin treatment. I altered some coefficients in the HPA axis model according to how the data indicated that melatonin affects corticotroph receptors.

* My extension also uses equations from an extension of the HPA axis model made by Yadav & Singh, who added dynamic glucocorticoid receptor feedback.
* I decided to look into the relationship between melatonin treatment and the HPA axis because:
    * I've heard from people that melatonin helps calm down your stress and helps you sleep, but I didn't know very much about the science behind that.
    * Cortisol and melatonin are closely related! They both work in cicadian patterns. Cortisol peaks when you wake up, and melatonin helps us sleep.
    * After some more research (Konakchieva, Marinova, Persengiev), I found data that shows that melatonin treatment has a strong effect on glucocorticoid receptors (pivotal to the HPA axis and to stress response)


# Extension Data (Raw)
## Relevant Parameter Values
(Karin et al, Yadav & Singh)
| Parameter | Description | Value |
| --------- | ----------- | ----- |
| $w_1$ | CRH clearance rate | 0.17/min |
| $w_2$ | ACTH clearance rate | 0.035/min|
| $w_3$ | Cortisol clearance rate | 0.0086/min|
| $w_C$ | Corticotroph mass turnover | $\frac{0.099}{(24)(60)}$/min
| $w_A$ | Adrenal mass turnover | $\frac{0.049}{(24)(60)}$/min |
| $n$ | Hill coefficient | 3 | 

## Melatonin Effect on Mice
(Marinova et al)

* $K_d$ is glucocorticoid receptor threshold factor (higher = lower affinity/ higher threshold)
* $B_{max}$ is expression of receptors (higher = more expression)

### Changes in hypothalamic glucocorticoid receptors
| Treatment Group | Meaning | $K_d$ | $B_{max}$ |
| --------------- | -------- | ----- | --------|
| CS + NaCl | Chronic stress | 13.7 | 17.6 |
| CS + MEL | Chronic stress + melatonin treatment | 16.4 | 19.3 |
| CONT + NaCl | Control group | 9.8 | 15.5|
| CONT + MEL | Control group + melatonin treatment | 26.9| 53.1 |

### Changes in pituitary glucocorticoid receptors
| Treatment Group | Meaning | $K_d$ | $B_{max}$ |
| --------------- | -------- | ----- | --------|
| CS + NaCl | Chronic stress | 2.5 | 16.6 |
| CS + MEL | Chronic stress + melatonin treatment | 4.1 | 9.3 |
| CONT + NaCl | Control group | 2.2 | 8.2|
| CONT + MEL | Control group + melatonin treatment | 10.4 | 20.3 |

# Full Extended Model
## Functions from original model (Karin et al)
### Hormones
$\frac{d}{dt}x_1=k_1g_1(x_3)u-w_1x_1$
$\frac{d}{dt}x_2=k_2g_2(x_3)x_1-w_2x_2$
$\frac{d}{dt}x_3=k_3x_2-w_3x_3$

* $x_1$ = CRH concentration
* $x_2$ = ACTH concentration
* $x_3$ = cortisol concentration

### Gland masses
$\frac{dC}{dt}=C(k_Cx_1-w_C)$
$\frac{dA}{dt}=A(k_Ax_2-w_A)$
* C = functional corticotroph mass
* A = functional adrenal gland mass

### Negative feedback on CRH
$g_1(x_3)= G(x_3)\cdot M(x_3)$

### Negative feedback on ACTH
$g_2(x_3)= G(x_3)$

### Mineralocorticoid feedback
$M(x_3)=\frac{1}{x_3}$


## Extension (Melatonin Treatment Affects GR)
### R(x_3) = Density of glucocorticoid receptors
Equation (modeling glucocorticoid receptor resistance) modified from original by Karin et al to incorporate receptor density (R), which varies when melatonin treatment is on

$G(x_3)=\frac{R(x_3)}{1+(\frac{x_3}{K_t})^n}$

### Sensitivity of glucocorticoid receptors
Equation modified from original by Yadav & Singh to incorporate melatonin modifier (L)

$K(t) = \begin{cases} 5 \cdot L(x_3), & \text{if } \frac{t}{1440} \le D_{\text{stop}} \\[8pt] 2 \cdot L(x_3) + 3 \cdot L(x_3)\, e^{-\frac{t - 1440 D_{\text{stop}}}{\tau}}, & \text{if } \frac{t}{1440} > D_{\text{stop}}
\end{cases}$


### Melatonin effect while stressed and while not stressed
$R(x_3) = \begin{cases}
1, & \text{if melatonin treatment is off} \\[6pt]
b_{\text{not stressed}} & \text{if melatonin treatment is on and x3 } \geq \text { threshold}\\
b_{\text{stressed}} & \text{if melatonin treatment is on and x3 } < \text { threshold}\\
\end{cases}$

$L(x_3) = \begin{cases} 1, & \text{if melatonin treatment is off} \\[6pt]
l_{\text{not stressed}} & \text{if melatonin treatment is on and x3 } \geq \text { threshold}\\
l_{\text{stressed}} & \text{if melatonin treatment is on and x3 } < \text { threshold}\\
\end{cases}$


* $x3$ threshold is the steady state of $x3$ = (CA)^\frac{1}{2} \approx 0.94$ 

* $b_\text{not stressed}$ and $b_\text{stressed}$ are calculated by averaging the headings $B_\text{max}$ (glucocorticoid threshold factor) from the tables in the section "Melatonin Effect on Mice," for the groups going under long term stress and melatonin treatment, and for the control groups with melatonin treatment.

* $l_\text{not stressed}$ and $l_\text{stressed}$ are calculated by averaging the headings $K_d$ (glucocorticoid threshold factor) from the tables in the section "Melatonin Effect on Mice," for the groups going under long term stress and melatonin treatment, and for the control groups with melatonin treatment.

# Bibliography

Karin, O., Raz, M., Tendler, A., Bar, A., Korem Kohanim, Y., Milo, T., & Alon, U. (2020). A new model for the HPA axis explains dysregulation of stress hormones on the timescale of weeks. Molecular Systems Biology, 16(7), e9510. https://doi.org/10.15252/msb.20209510

Konakchieva, R., Mitev, Y., Almeida, O. F. X., & Patchev, V. K. (1998). Chronic melatonin treatment counteracts glucocorticoid-induced dysregulation of the hypothalamic-pituitary-adrenal axis in the rat. Neuroendocrinology, 67(3), 171-80. https://proxy.lib.uwaterloo.ca/login?url=https://www.proquest.com/scholarly-journals/chronic-melatonin-treatment-counteracts/docview/274617016/se-2

Marinova, C., Persengiev, S., Konakchieva, R., Ilieva, A., & Patchev, V. (1991). Melatonin effects on glucocorticoid receptors in rat brain and pituitary: Significance in adrenocortical regulation. International Journal of Biochemistry, 23(4), 479–481. https://doi.org/10.1016/0020-711X(91)90177-O

Persengiev, S. P. (1999). Multiple domains of melatonin receptor are involved in the regulation of glucocorticoid receptor-induced gene expression. The Journal of Steroid Biochemistry and Molecular Biology, 68(5), 181–187. https://doi.org/10.1016/S0960-0760(99)00029-1

Persengiev, S., Patchev, V., & Velev, B. (1991). Melatonin effects on thymus steroid receptors in the course of primary antibody responses: Significance of circulating glucocorticoid levels. International Journal of Biochemistry, 23(12), 1487–1489. https://doi.org/10.1016/0020-711X(91)90292-U

Yadav, M., & Singh, P. (2025). A mechanistic modeling framework to interpret ACTH stimulation tests across HPA axis adaptation states and glucocorticoid feedback dynamics. Computers in Biology and Medicine, 198, 111173. https://doi.org/10.1016/j.compbiomed.2025.111173
