Full code for plots is available in final_project.ipynb

# Original Model Description


# Extension Data (Raw)


# Full Extended Model
## Functions from original model (Karin et al)
### Hormones
$\begin{equation}\frac{d}{dt}x_1=k_1g_1(x_3)u-w_1x_1\end{equation}$
$\begin{equation}\frac{d}{dt}x_2=k_2g_2(x_3)x_1-w_2x_2\end{equation}$
$\begin{equation}\frac{d}{dt}x_3=k_3x_2-w_3x_3\end{equation}$

* $x_1$ = CRH concentration
* $x_2$ = ACTH concentration
* $x_3$ = cortisol concentration

### Gland masses
$\begin{equation}\frac{dC}{dt}=C(k_Cx_1-w_C)\end{equation}$
$\begin{equation}
    \frac{dA}{dt}=A(k_Ax_2-w_A)
\end{equation}$
* C = functional corticotroph mass
* A = functional adrenal gland mass

### Negative feedback on CRH
$\begin{equation}
    g_1(x_3)= G(x_3)\cdot M(x_3)
\end{equation}$

### Negative feedback on ACTH
$\begin{equation}
    g_2(x_3)= G(x_3)
\end{equation}$

### Mineralocorticoid feedback
$\begin{equation}
    M(x_3)=\frac{1}{x_3}
\end{equation}$


## Extension (Melatonin Treatment Affects GR)
### R(x_3) = Density of glucocorticoid receptors
Equation (modeling glucocorticoid receptor resistance) modified from original by Karin et al to incorporate receptor density (R), which varies when melatonin treatment is on
$\begin{equation}
    G(x_3)=\frac{R(x_3)}{1+(\frac{x_3}{K_t})^n}
\end{equation}$

### Sensitivity of glucocorticoid receptors
Equation modified from original by Yadav & Singh to incorporate melatonin modifier (L)
$\begin{equation}
K(t) =
\begin{cases}
5 \cdot L(x_3), & \text{if } \frac{t}{1440} \le D_{\text{stop}} \\[8pt]
2 \cdot L(x_3) + 3 \cdot L(x_3)\, e^{-\frac{t - 1440 D_{\text{stop}}}{\tau}}, & \text{if } \frac{t}{1440} > D_{\text{stop}}
\end{cases}
\end{equation}$

### Exponential causes smooth transitions between melatonin effect while stressed and while not stressed
$\begin{equation}R(x_3) =
\begin{cases}
1, & \text{if melatonin treatment is off} \\[6pt]
b_{\text{low}} + \frac{b_{\text{high}} - b_{\text{low}}}{1 + e^{-15(x_3 - x_{3,\text{threshold}})}}, & \text{if melatonin treatment is on}
\end{cases}\end{equation}$

$\begin{equation}
L(x_3) =
\begin{cases}
1, & \text{if melatonin treatment is off} \\[6pt]
v_{\text{low}} + \frac{v_{\text{high}} - v_{\text{low}}}{1 + e^{-15(x_3 - x_{3,\text{threshold}})}}, & \text{if melatonin treatment is on}
\end{cases}
\end{equation}$

# Bibliography

Karin, O., Raz, M., Tendler, A., Bar, A., Korem Kohanim, Y., Milo, T., & Alon, U. (2020). A new model for the HPA axis explains dysregulation of stress hormones on the timescale of weeks. Molecular Systems Biology, 16(7), e9510. https://doi.org/10.15252/msb.20209510

Konakchieva, R., Mitev, Y., Almeida, O. F. X., & Patchev, V. K. (1998). Chronic melatonin treatment counteracts glucocorticoid-induced dysregulation of the hypothalamic-pituitary-adrenal axis in the rat. Neuroendocrinology, 67(3), 171-80. https://proxy.lib.uwaterloo.ca/login?url=https://www.proquest.com/scholarly-journals/chronic-melatonin-treatment-counteracts/docview/274617016/se-2

Marinova, C., Persengiev, S., Konakchieva, R., Ilieva, A., & Patchev, V. (1991). Melatonin effects on glucocorticoid receptors in rat brain and pituitary: Significance in adrenocortical regulation. International Journal of Biochemistry, 23(4), 479–481. https://doi.org/10.1016/0020-711X(91)90177-O

Persengiev, S. P. (1999). Multiple domains of melatonin receptor are involved in the regulation of glucocorticoid receptor-induced gene expression. The Journal of Steroid Biochemistry and Molecular Biology, 68(5), 181–187. https://doi.org/10.1016/S0960-0760(99)00029-1

Persengiev, S., Patchev, V., & Velev, B. (1991). Melatonin effects on thymus steroid receptors in the course of primary antibody responses: Significance of circulating glucocorticoid levels. International Journal of Biochemistry, 23(12), 1487–1489. https://doi.org/10.1016/0020-711X(91)90292-U

Yadav, M., & Singh, P. (2025). A mechanistic modeling framework to interpret ACTH stimulation tests across HPA axis adaptation states and glucocorticoid feedback dynamics. Computers in Biology and Medicine, 198, 111173. https://doi.org/10.1016/j.compbiomed.2025.111173
