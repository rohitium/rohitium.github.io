---
title: "A drug target liver injury risk score"
date: 2025-07-14
layout: post
---

<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml-full.js"
        id="MathJax-script"
        async></script>

Drug-induced liver injury (DILI) continues to be a leading cause of drug failure in clinical trials and post-marketing withdrawals.[^1] The liver's central role in drug metabolism, combined with its complex cellular architecture and diverse metabolic pathways, makes it particularly vulnerable to drug-induced damage. Traditional approaches to DILI prediction have relied heavily on animal models and in vitro assays, but these methods have limited predictive power for human outcomes.[^2]

The advent of large-scale genomic and proteomic datasets has opened new possibilities for computational approaches to DILI risk assessment. In particular, integration of drug-target interaction data, pathway information, and clinical outcomes has enabled the development of more sophisticated risk prediction models. Here, I present a computational framework for assessing DILI risk at the drug target level, leveraging multiple data sources to create interpretable risk scores that can guide target prioritization in drug discovery.

## Definition

The core insight of this approach is that DILI risk can be assessed through two complementary mechanisms: **direct evidence** and **network guilt-by-association**. Direct evidence captures the known DILI risk associated with drugs that target a specific protein, while network guilt-by-association leverages the biological principle that proteins connected in cellular networks often share functional properties, including susceptibility to drug-induced damage.

### Direct Evidence Computation

For each drug target, we compute direct DILI evidence based on the FDA's DILIrank dataset, which categorizes drugs according to their DILI concern level.[^3] The direct evidence score for a target $$T$$ is calculated as:

$$\text{Direct Evidence}(T) = \frac{\sum_{d \in D_T} w_d}{\sum_{d \in D_T}}$$

where $$D_T$$ is the set of drugs targeting $$T$$, $$w_d$$ is the DILI severity weight for drug $$d$$, and $$\sum_{d \in D_T}$$ is the number of drugs targeting $$T$$.

This approach captures the empirical observation that targets with more high-risk drugs are themselves more likely to be associated with DILI. For example, cytochrome P450 enzymes (CYP3A4, CYP2D6) show high direct evidence scores due to their involvement in the metabolism of numerous hepatotoxic compounds.[^4]

### Network Guilt-by-Association

The network component leverages Pathway Commons data to identify targets that are functionally connected to known DILI-associated proteins.[^5] The network score for a target $$T$$ is computed as:

$$\text{Network Score}(T) = \sum_{N \in \mathcal{N}(T)} \text{Direct Evidence}(N)$$

where $$\mathcal{N}(T)$$ represents the set of network neighbors of target $$T$$.

This approach is based on the biological principle that proteins involved in related cellular processes often share similar drug sensitivity profiles. For instance, targets in the same metabolic pathway or protein complex may exhibit similar DILI risk profiles.

### Combined Risk Score

The final DILI risk score combines both components:

$$\text{DILI Risk Score}(T) = \alpha \cdot \text{Direct Evidence}(T) + (1-\alpha) \cdot \text{Network Score}(T)$$

where $$\alpha$$ is a weighting parameter (typically set to 0.5) that balances the contribution of direct evidence versus network guilt-by-association.

## Data Integration and Validation

The framework integrates data from multiple sources:

1. **OpenTargets**: Drug-target interaction data for 434 targets across the human proteome[^6]
2. **FDA DILIrank**: Clinical DILI concern classifications for 1,036 drugs[^3]
3. **Pathway Commons**: Protein-protein interaction and pathway data[^5]
4. **OpenFDA**: Drug approval and withdrawal data for validation[^7]

The validation strategy employs two complementary approaches:

1. **Approval Rate Correlation**: Higher DILI risk targets should show lower drug approval rates
2. **Withdrawal Rate Correlation**: Higher DILI risk targets should show higher drug withdrawal rates

### Calculation of Validation Metrics

**Approval rates** are computed per target as the proportion of approved drugs among all drugs targeting that protein. For each target $$T$$:

$$\text{Approval Rate}(T) = \frac{\text{Number of approved drugs targeting } T}{\text{Total number of drugs targeting } T}$$

**Withdrawal rates** are computed per target as the proportion of withdrawn drugs among all drugs targeting that protein:

$$\text{Withdrawal Rate}(T) = \frac{\text{Number of withdrawn drugs targeting } T}{\text{Total number of drugs targeting } T}$$

Both metrics are calculated using deduplicated drug-target pairs to ensure consistency with the risk scoring process. Drug approval and withdrawal status is obtained from OpenFDA data, which tracks the regulatory history of pharmaceutical compounds. A drug is considered "approved" if it has received FDA approval for any indication, and "withdrawn" if it has been removed from the market due to safety concerns, including liver injury.

![DILI Risk Score vs Withdrawal Rate](/assets/risk_vs_withdrawal.png)

*Figure 1: DILI risk score vs. drug withdrawal rate for 434 targets. Higher DILI risk targets show higher withdrawal rates (r = 0.212), indicating that targets associated with more hepatotoxic drugs are more likely to have drugs withdrawn from the market due to safety concerns.*

![DILI Risk Score vs Approval Rate](/assets/risk_vs_approval.png)

*Figure 2: DILI risk score vs. drug approval rate for 434 targets. Higher DILI risk targets show lower approval rates (r = -0.252), indicating that targets associated with more hepatotoxic drugs have fewer drugs successfully approved by regulatory agencies.*

## Results and Interpretation

The analysis of 434 drug targets reveals several interesting patterns:

### Risk Distribution
- **Low Risk**: 391 targets (90.1%)
- **Medium Risk**: 31 targets (7.1%)
- **High Risk**: 12 targets (2.8%)

This distribution reflects the reality that most drug targets are not strongly associated with DILI, while a small subset shows elevated risk.

### Top High-Risk Targets

The highest-risk targets identified include:

1. **PTGS1** (COX-1): Risk score 1.000 (High)
   - Associated with 37 unique drugs
   - Withdrawal rate: see validation report
2. **PTGS2** (COX-2): Risk score 1.000 (High)
   - Associated with 38 unique drugs
   - Withdrawal rate: see validation report
3. **SCN10A, SCN11A, SCN1A, SCN2A, SCN3A, SCN4A, SCN5A, SCN7A** (Sodium channels): Risk score 0.793 (High)
   - Each associated with 29 unique drugs
   - Withdrawal rate: see validation report

These results align with known DILI mechanisms. COX enzymes are well-established targets for NSAIDs, which are known to cause liver injury in susceptible individuals. Sodium channels are also implicated in drug-induced toxicity.[^8]

## Limitations and Future Directions

The current approach has several limitations:

1. **Data Quality**: The approach relies on the quality and completeness of the underlying datasets. Missing drug-target interactions or incomplete DILI classifications could affect results.
2. **Mechanistic Complexity**: DILI involves complex interactions between drug properties, target characteristics, and patient factors. A target-level approach necessarily simplifies this complexity.
3. **Temporal Dynamics**: The current approach uses static data, but DILI risk may vary over time as new drugs are developed and clinical experience accumulates.
4. **Validation Scope**: The validation approach focuses on aggregate withdrawal/approval rates, but individual drug outcomes may vary significantly.

Future work could address these limitations through:

1. **Multi-omic Integration**: Incorporating transcriptomic, proteomic, and metabolomic data to capture dynamic cellular responses
2. **Patient-Specific Modeling**: Accounting for genetic, demographic, and clinical factors that influence individual DILI risk
3. **Temporal Analysis**: Tracking how DILI risk profiles evolve as new drugs and clinical data become available
4. **Mechanistic Validation**: Experimental validation of predicted high-risk targets using in vitro and in vivo models

## Clinical and Drug Discovery Implications

Despite its limitations, this computational framework provides several valuable insights for drug discovery and development:

1. **Target Prioritization**: The risk scores can help prioritize targets for drug development, with higher-risk targets requiring more extensive preclinical safety assessment.
2. **Safety Monitoring**: For drugs targeting high-risk proteins, enhanced clinical monitoring for liver function may be warranted.
3. **Drug Repurposing**: The framework can identify existing drugs that target high-risk proteins, potentially flagging candidates for enhanced safety monitoring.
4. **Mechanistic Insights**: The network component can reveal unexpected connections between targets and DILI mechanisms, potentially uncovering new therapeutic targets for DILI prevention.

## Conclusion

The computational framework presented here represents a step toward more systematic assessment of DILI risk at the target level. By integrating multiple data sources and employing both direct evidence and network guilt-by-association approaches, the method provides interpretable risk scores that align with biological expectations and clinical outcomes.

While the current implementation has limitations, it demonstrates the potential for computational approaches to complement traditional experimental methods in drug safety assessment. As datasets grow and methods improve, such approaches may become increasingly valuable for guiding drug discovery and development decisions.

The framework is implemented as an open-source pipeline with a web application for real-time risk score checking, making it accessible to researchers and drug developers worldwide. The modular design allows for easy integration of new data sources and methodological improvements as they become available.

### References:

[^1]: Björnsson, E. S. (2015). Drug-induced liver injury: an overview over the most critical compounds. *Archives of toxicology*, 89(3), 327-334.
[^2]: Olson, H., Betton, G., Robinson, D., Thomas, K., Monro, A., Kolaja, G., ... & Heller, A. (2000). Concordance of the toxicity of pharmaceuticals in humans and in animals. *Regulatory toxicology and pharmacology*, 32(1), 56-67.
[^3]: Chen, M., Suzuki, A., Borlak, J., Andrade, R. J., & Lucena, M. I. (2015). Drug-induced liver injury: Interactions between drug properties and host factors. *Journal of hepatology*, 63(2), 503-514.
[^4]: Zanger, U. M., & Schwab, M. (2013). Cytochrome P450 enzymes in drug metabolism: regulation of gene expression, enzyme activities, and impact of genetic variation. *Pharmacology & Therapeutics*, 138(1), 103-141.
[^5]: Rodchenkov, I., Babur, O., Luna, A., Aksoy, B. A., Wong, J. V., Fong, D., ... & Sander, C. (2020). Pathway Commons 2019 Update: integration, analysis and exploration of pathway data. *Nucleic acids research*, 48(D1), D489-D497.
[^6]: Ochoa, D., Hercules, A., Carmona, M., Suveges, D., Gonzalez-Uriarte, A., Malangone, C., ... & Papatheodorou, I. (2021). Open Targets Platform: supporting systematic drug–target identification and prioritisation. *Nucleic Acids Research*, 49(D1), D1302-D1310.
[^7]: Zarin, D. A., Tse, T., Williams, R. J., Califf, R. M., & Ide, N. C. (2011). The ClinicalTrials. gov results database—update and key issues. *New England Journal of Medicine*, 364(9), 852-860.
[^8]: Watkins, P. B., Kaplowitz, N., Slattery, J. T., Colonese, C. R., Colucci, S. V., Stewart, P. W., & Harris, S. C. (2006). Aminotransferase elevations in healthy adults receiving 4 grams of acetaminophen daily: a randomized controlled trial. *JAMA*, 296(1), 87-93. 