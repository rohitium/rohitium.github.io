---
title: "A Single Molecule View of Diffusion-Limited Bimolecular Association"
date: 2025-02-16
layout: post
---

<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml-full.js"
        id="MathJax-script"
        async></script>

The first step of biomolecular communication is the binding of two molecules, e.g., a transcription factor binding to the promoter sequence of the target gene or a drug binding to its target protein. Especially from the drug design point of view, it is useful to optimize the structure of the drug molecule such that this binding process is fast, specific, and robust.[^1] Moreover, the rate of such a binding process, also called the "on"-rate and denoted as $$k_{\text{on}}$$, is commonly used to parameterize pathway models for cell signaling cascades.[^2] Such pathway models, in turn, act as cost-effective tools for _in silico_ drug screening and target identification and prioritization.[^3]

### The Encounter Complex (EC)

A key concept within the theory of bimolecular association is the notion of the Encounter Complex (EC),[^4] [^5] or the set of structural configurations observed when the molecules first "encounter" one another, that may be characterized by non-specific interactions between the two molecules.[^6] [^7] Depending on the precise structure, orientation, and conditions of this encounter, the molecules may dissociate again or continue to form stable interactions, e.g., van der Waals, hydrophobic, or electrostatic interactions, with possible conformational rearrangements, and settle into a bound complex (BC).

![EC-scheme](/assets/EC_scheme.png)

Given an excess of computational resources, the exact on-rate \($$k_{\text{on}}$$\) can, in principle, be calculated from physics-based numerical sampling methods for any two molecules under any arbitrary reaction conditions. However, in practice, the problem is often broken down into two steps:

1. The process that leads to the formation of encounter complex (EC).
2. Molecule-specific interaction dynamics, including conformational changes, that lead to formation of the bound complex (BC) from the encounter complex (EC).

Step 2 must still, in general, be treated using numerical sampling, e.g. all-atom molecular dynamics simulationsm,[^8] [^9] [^10] while step 1 often relies on analytical kinetic rate theories.

### The Smoluchowski Diffusion Rate Equation

More than a century ago, Smoluchowski proposed a theory for the kinetics of colloid-coagulation as a diffusion-controlled process.[^11] Since then, his theory has been further extended to include effects of unsuccessful collisions,[^12] ions,[^13] rotational diffusion,[^14] and asymmetric binding modes.[^15]

According to Smoluchowski theory, the rate at which any two biomolecules encounter each other can be written as:

<div id="eq1">
$$
k_{\text{on}} = 4 \pi D R \tag{1}
$$
</div>

where $$D = D_A + D_B$$ is the sum of diffusion coefficients of the two molecules and $$R = R_A + R_B$$ is the sum of their radii. This result is remarkable for several reasons:

1. The association rate for two diffusing molecules turns out to be simply proportional to the total size and diffusivity of the molecules.
2. It provides the speed limit - a soft upper bound - of the bimolecular reaction rate for any two molecules. The actual association rate is likely to be less than this quantity.
3. From a profound theoretical viewpoint, this rate equation establishes a direct connection between _physical dynamics_ and _chemical kinetics_ of biomolecules.

### Derivation of Smoluchowski Rate

This last point can be exemplified by deriving [Eq. (1)](#eq1) from first-principles. Let us start with the usual derivation of the Smoluchowski rate equation from bulk diffusion.[^16] 

We envision the dynamics of the two molecules as isotropic translational diffusion between molecules of $$A$$ and $$B$$. Without losing any generality, we put ourselves in the reference frame of molecule $$A$$, such that $$A$$ is held stationary, while $$B$$ diffuses until molecules of $$B$$ encounter molecules of $$A$$. This model suggests that the concentration of (unbound) $$B$$ molecules can be described by Fick’s second law:

<div id="eq2">
$$
∂C(\mathbf{r}, t)/∂t = D∇^2C(\mathbf{r}, t) \tag{2} 
$$
</div>

Where $$D = D_A + D_B$$ is again the sum of diffusion coefficients of A and B, while $$C(\mathbf{r}, t)$$ represents the concentration of $$B$$ at any spatial location $$ \mathbf{r}  = (r, 𝜃, ø) $$ and at any time $$t$$. Note that, for the purpose of this problem, we are not interested in the dynamics of the association of $$A$$ and $$B$$ beyond the encounter complex, or the dynamics of dissociation of $$A$$ and $$B$$. 

When $$A$$ and $$B$$ are far apart, the concentration of $$B$$ is just a constant – the bulk concentration of $$B$$ in solution, say $$ C(r → ∞, t) = C_0$$. As soon as $$A$$ and $$B$$ encounter each other, we assume that they have reached the EC. Thus, we install an absorbing boundary condition at the encounter cross-section, e.g. $$r = R = R_A + R_B$$ or the sum of the radii of $$A$$ and $$B$$, such that $$C(r = R, t) = 0$$. 

At steady state, the rate of change of $$C(r, t)$$ or the left hand side of [Eq. (2)](#eq2) goes to zero. In other words, the concentration distribution of $$B$$ becomes constant with respect to time, i.e. $$C(r, t → ∞) → C(r)$$. Thus, [Eq. (2)](#eq2) reduces to:

<div id="eq3">
$$
∇^2C(\mathbf{r}) = 0 \tag{3}
$$
</div>

In order to solve this equation analytically, we exploit the spherical symmetry of the problem – collision of $$A$$ and $$B$$ in any orientation with respect to each other leads to the formation of the encounter complex and, thus, the Laplacian in [Eq. (3)](#eq3) consists of only the radial direction in spherical coordinates:

<div id="eq4">
$$
∂/∂r(r^2 ∂C(r)/∂r) = 0 \tag{4}
$$
</div>

It is easy to verify that the solution to [Eq. (4)](#eq4) that satisfies the necessary boundary conditions can be written as:

<div id="eq5">
$$
C(r) = C_0 (1-R/r) \tag{5}
$$
</div>

Now that we have solved for the concentration distribution profile of $$B$$, we can use this solution to extract the rate of encounter with $$A$$. 

The flux of $$B$$ encountering $$A$$, or the number of collisions between $$A$$ and $$B$$ per unit time per unit area, is given by Fick’s first law:

<div id="eq6">
$$
\mathbf{J}(\mathbf{r}, t) = − D\mathbf{∇}C(\mathbf{r}, t) \tag{6}
$$
</div>

Plugging [Eq. (5)](#eq5) into [Eq. (6)](#eq6), we get the steady state flux of $$B$$ encountering $$A$$:

<div id="eq7">
$$
\begin{aligned}
\mathbf{J}(r = R) 
&= \left[ -D \nabla C(r) \right]_{r=R} \,\hat{\mathbf{r}} \\
&= \left[ -D C_0 \,\frac{R}{r^2} \right]_{r=R} \,\hat{\mathbf{r}} \\
&= -\left(\frac{D C_0}{R}\right)\,\hat{\mathbf{r}}
\end{aligned}
\tag{7}
$$
</div>

The negative sign here simply denotes the direction of the flux in the radial coordinate $$ȓ$$, i.e. towards the molecules of $$A$$ or, equivalently, towards the origin. The total flux over the entire cross-section of the encounter, i.e. the total number of collisions between $$A$$ and $$B$$ per unit time, can be obtained by computing the surface integral:

<div id="eq8">
$$
j = −∮\mathbf{J}(\mathbf{r}, t)⋅d\mathbf{S} \tag{8}
$$
</div>

For the simplest scenario of a spherical encounter cross-section with radius $$r = R$$, $$∮d\mathbf{S} = 4πR^2 ȓ$$ and the total flux turns out to be:

<div id="eq9">
$$
\begin{aligned}
j 
&= -\,\mathbf{J}(r = R)\,\cdot \bigl(4\pi R^2 \,\hat{\mathbf{r}}\bigr) \\
&= \left(\frac{D\,C_0}{R}\right)\,\hat{\mathbf{r}} \,\cdot \bigl(4\pi R^2 \,\hat{\mathbf{r}}\bigr) \\
&= 4\pi\,D\,R\,C_0
\end{aligned}
\tag{9}
$$
</div>

Finally, the rate of this encounter is simply the flux per unit bulk concentration of $$B$$, i.e.

<div id="eq10">
$$
\begin{aligned}
k_{\text{on}} 
&= \frac{j}{C_0} \\
&= 4\pi\,D\,R
\end{aligned}
\tag{10}
$$
</div>

### References

[^1]: Vauquelin, G. (2016). *Effects of target binding kinetics on in vivo drug efficacy: $$k_{\text{off}}, k_{\text{on}}$$, and rebinding*. British Journal of Pharmacology, 173(15), 2319-2334.
[^2]: Klipp, E., & Liebermeister, W. (2006). *Mathematical modeling of intracellular signaling pathways*. BMC Neuroscience, 7, 1-16.
[^3]: Bansal, L., Nichols, E. M., et al. (2022). *Mathematical modeling of complement pathway dynamics for target validation and selection of drug modalities for complement therapies*. Frontiers in Pharmacology, 13, 855743.
[^4]: Eigen, M. (1974). *Diffusion control in biochemical reactions*. Quantum Statistical Mechanics in the Natural Sciences.
[^5]: Bell, G. I. (1978). *Models for the specific adhesion of cells to cells*. Science, 200(4342), 618-627.
[^6]: Gabdoulline, R. R., & Wade, R. C. (1999). *On the protein-protein diffusional encounter complex*. Journal of Molecular Recognition, 12(4), 226-234.
[^7]: Tang, C., Iwahara, J., & Clore, G. M. (2006). *Visualization of transient encounter complexes in protein-protein association*. Nature, 444(7117), 383-386.
[^8]: Tiwary, P., et al. (2015). *Kinetics of protein-ligand unbinding: Predicting pathways, rates, and rate-limiting steps*. Proceedings of the National Academy of Sciences, 112(5), E386-E391.
[^9]: Plattner, N., et al. (2017). *Complete protein-protein association kinetics in atomic detail revealed by molecular dynamics simulations and Markov modeling*. Nature Chemistry, 9(10), 1005-1011.
[^10]: Tang, Z., et al. (2020). *Transient states and barriers from molecular simulations and the milestoning theory*. Journal of Chemical Theory and Computation, 16(3), 1882-1895.
[^11]: Smoluchowski, M. V. (1917). *Mathematical theory of the kinetics of the coagulation of colloidal solutions*. Z. Phys. Chem., 92, 129-168.
[^12]: Sveshnikoff, B. (1935). *On the theory of photochemical reactions and chemiluminescence in solutions*. Acta Physicochim. URSS, 3, 257-268.
[^13]: Debye, P. (1942). *Reaction rates in ionic solutions*. Transactions of the Electrochemical Society, 82(1), 265.
[^14]: Shoup, D., Lipari, G., & Szabo, A. (1981). *Diffusion-controlled bimolecular reaction rates. The effect of rotational diffusion and orientation constraints*. Biophysical Journal, 36(3), 697-714.
[^15]: Schlosshauer, M., & Baker, D. (2002). *A general expression for bimolecular association rates with orientational constraints*. The Journal of Physical Chemistry B, 106(46), 12079-12083.
[^16]: Altan-Bonnet, G., Mora, T., & Walczak, A. M. (2020). *Quantitative immunology for physicists*. Physics Reports, 849, 1-83.
