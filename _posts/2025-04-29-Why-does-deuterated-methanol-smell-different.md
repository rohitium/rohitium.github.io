---
title: "Why does deuterated methanol smell different?"
date: 2025-04-29
layout: post
---

<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml-full.js"
        id="MathJax-script"
        async></script>

The Born-Oppenheimer approximation[^1] is a fundamental tool in theoretical chemistry and remains valid in case of a large majority of natural phenomena. However, it can lead to the notion that changes in the number of neutrons in the nucleus have little to no impact on chemical, and consequently biological, processes.

This is why it is surprising that flies can be trained to distinguish between natural and deuterated odorants,[^2] i.e. some of the hydrogen atoms in the compound are replaced by "heavy hydrogen" (Hydrogen with a neutron in the nucleus, instead of just a proton). The proposed explanation of this phenomenon caused quite a controversy and was covered by Derek Lowe in a blog post.[^3] And if you believe this guy in the comments section, the observation is supported by trials in people (good luck getting this past the FDA!)

![DeuteratedMethanolExperiment](/assets/DeuteratedMethanolExperiment-comment.png)

There are, of course, well documented mechanisms by which isotopes exert their influence in chemical kinetics, e.g. the Kinetic Isotope effect.[^4] The heavier isotope tends to have *lower* ground state energy and therefore slows down chemical reactions, compared to the lighter isotope. Note that this falls well under the purview of the Born-Oppenheimer approximation, since the shift in the ground state is an equilibrium property of the isotope's potential energy surface, unrelated to the relative dynamics of electrons and the nucleus.

However, the exact mechanism by which our nose could distinguish deuterated methanol is unclear. The flies study claims that typical structure-based models - aka "lock and key" type models - are inadequate in describing their observations, since replacing hydrogens by deuterium atoms does not change the structure of the odorant and so the chemistry of recognition by olfactory receptors is unchanged.[^2] Rather the authors propose that an alternate theory, based on differing vibrational frequencies of the two compounds is more likely.[^5]

According to this theory, ligand recognition by olfactory receptors is mediated by an inelastic electron tunneling (IET) process, entailing charge transfer between the ligand and the receptor that is sensitive to both the olfactant's shape and vibrational state. Since deuterium atoms have different bond energies than hydrogen, the compound's overall vibrational frequencies are perturbed, leading to changes in the energy transfer process to the receptor and altering the resulting conformational state. Note that this mechanism is hotly contested. [^7] [^8]

Nevertheless, apart from academic interest, this question has practical value from the point of view of modern drug discovery. For example, it was shown in the 80s that mouse models of bipolar disorder exhibit different behavioral responses to Li-7 vs Li-6.[^9] Recent experiments in ketamine-induced hyperactivity models of mania have shown similar Li isotope effects in rats.[^10] Fittingly, the discussion section of the latter study proposes quantum processing in the brain as an explanation.[^11]

## References

[^1]: Born, M., & Oppenheimer, R. (1927). Zur quantentheorie der molekeln *Annalen der Physik*, v. 84.
[^2]: Franco, M. I., Turin, L., Mershin, A., & Skoulakis, E. M. (2011). Molecular vibration-sensing component in Drosophila melanogaster olfaction. *Proceedings of the National Academy of Sciences*, 108(9), 3797-3802.
[^3]: https://www.science.org/content/blog-post/vibrational-scent-wins-round.
[^4]: https://en.wikipedia.org/wiki/Kinetic_isotope_effect.
[^5]: Turin, L. (1996). A spectroscopic mechanism for primary olfactory reception. *Chemical Senses*, 21(6), 773-791.
[^6]: Brookes, J. C., Horsfield, A. P., & Stoneham, A. M. (2012). The swipe card model of odorant recognition. *Sensors*, 12(11), 15709-15749.
[^7]: Keller, A., & Vosshall, L. B. (2004). A psychophysical test of the vibration theory of olfaction. *Nature Neuroscience*, 7(4), 337-338.
[^8]: Block, E., Jang, S., Matsunami, H., Sekharan, S., Dethier, B., Ertem, M. Z., ... & Zhuang, H. (2015). Implausibility of the vibrational theory of olfaction. *Proceedings of the National Academy of Sciences*, 112(21), E2766-E2774.
[^9]: Sechzer, J. A., Lieberman, K. W., Alexander, G. J., Weidman, D., & Stokes, P. E. (1986). Aberrant parenting and delayed offspring development in rats exposed to lithium. *Biological Psychiatry*, 21(13), 1258-1266.
[^10]: Ettenberg, A., Ayala, K., Krug, J. T., Collins, L., Mayes, M. S., & Fisher, M. P. (2020). Differential effects of lithium isotopes in a ketamine-induced hyperactivity model of mania. *Pharmacology Biochemistry and Behavior*, 190, 172875.
[^11]: Fisher, M. P. (2015). Quantum cognition: The possibility of processing with nuclear spins in the brain. *Annals of Physics*, 362, 593-602.