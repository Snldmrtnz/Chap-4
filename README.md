## 16. Chapter 4: Results and Discussion

This chapter presents the experimental findings of the proposed Integrated DRIMUX framework. Results are organized by study objective and discussed using quantitative evidence, practical implications, and alignment with related studies.

### 4.1 Presentation of Results

This section presents the observed outputs of the experiments using tables and figures.

#### 4.1.1 Objective 1: Assess the semantic classification performance

The final dataset contained 15,000 balanced samples (5,000 per class), split into 10,500 training, 2,250 validation, and 2,250 test instances.

Classification benchmark results are summarized below:

| Model | Family | Accuracy | Weighted F1 |
|---|---|---:|---:|
| SVM | Baseline | 0.9209 | 0.9209 |
| Logistic Regression | Baseline | 0.9178 | 0.9181 |
| Transformer | Deep Learning | 0.9182 | 0.9181 |
| Bi-LSTM | Deep Learning | 0.9116 | 0.9115 |
| Naive Bayes | Baseline | 0.8956 | 0.8942 |

Among deep models, Transformer achieved the best performance (weighted F1 = 0.9181), so it was selected as the semantic source model.

Transformer class-level test scores:

- misinformation: precision 0.8563, recall 0.9213, F1 0.8876
- activism: precision 0.9867, recall 0.9880, F1 0.9873
- neutral: precision 0.9162, recall 0.8453, F1 0.8793

![Figure 4.1 Transformer Confusion Matrix](../data/results/figures/confusion_matrix_transformer.png)

**Figure 4.1.** Confusion matrix of the selected Transformer model.

![Figure 4.2 Bi-LSTM Confusion Matrix](../data/results/figures/confusion_matrix_bilstm.png)

**Figure 4.2.** Confusion matrix of the Bi-LSTM model.

#### 4.1.2 Objective 2: Construct and validate semantic scaling profiles

Semantic scaling coefficients were configured as alpha_misinformation = 1.25, alpha_activism = 1.10, and alpha_neutral = 0.75. The resulting mean semantic scales from selected samples were:

- misinformation: 1.2490
- activism: 1.1000
- neutral: 0.7505

![Figure 4.3 Semantic Scaling by Content Type](../data/results/figures/semantic_scaling_by_content_type.png)

**Figure 4.3.** Mean semantic scaling by content type.

#### 4.1.3 Objective 3: Evaluate diffusion outcomes of integrated DRIMUX

Aggregate benchmark outcomes over Monte Carlo runs are shown below:

| Model | Final Reach Mean | Peak Activation Mean | Normalized AUC Mean |
|---|---:|---:|---:|
| Integrated DRIMUX | 196.3 | 23.5 | 0.4090 |
| Baseline DRIMUX | 165.7 | 18.7 | 0.2986 |
| SIR baseline | 51.2 | 7.1 | 0.0842 |

![Figure 4.4 Benchmark Diffusion Reach](../data/results/figures/benchmark_diffusion_reach.png)

**Figure 4.4.** Cumulative diffusion reach over time for benchmark models.

Content-specific integrated diffusion outputs are:

| Content Type | Semantic Scale | Final Reach Mean | Peak New Activation Mean |
|---|---:|---:|---:|
| misinformation | 1.2490 | 196.3 | 22.9 |
| activism | 1.1000 | 193.7 | 22.3 |
| neutral | 0.7505 | 82.3 | 8.2 |

![Figure 4.5 Content-Type Diffusion Reach](../data/results/figures/content_type_diffusion_reach.png)

**Figure 4.5.** Integrated diffusion reach by content type (mean with 95% confidence interval).

![Figure 4.6 New Activations by Content Type](../data/results/figures/new_activations_by_content_type.png)

**Figure 4.6.** New activations per time step by content type.

![Figure 4.7 Final Reach Distribution](../data/results/figures/content_type_final_reach_distribution.png)

**Figure 4.7.** Distribution of final reach across Monte Carlo runs.

![Figure 4.8 Normalized AUC Distribution](../data/results/figures/content_type_auc_distribution.png)

**Figure 4.8.** Distribution of normalized AUC across Monte Carlo runs.

![Figure 4.9 Integrated Misinformation Snapshot](../data/results/figures/integrated_misinformation_snapshot.png)

**Figure 4.9.** Network snapshot of integrated misinformation diffusion at an early step.

### 4.2 Comparative Analysis / Performance Evaluation

This section compares model performance and explains the significance of observed differences.

#### 4.2.1 Classification performance comparison

Although SVM achieved the highest overall weighted F1 (0.9209), the Transformer was the best deep model and therefore the appropriate semantic backbone for Integrated DRIMUX. The Transformer and Logistic Regression produced nearly identical weighted F1 (0.9181), indicating competitive classification quality between modern deep and strong sparse-feature baselines on this dataset.

From Figure 4.1 and Figure 4.2, Transformer exhibits cleaner class separation than Bi-LSTM, especially in the activism class. Neutral-class recall is lower than activism, indicating that neutral expressions overlap with persuasive or oppositional language patterns.

These trends align with prior text-classification findings that transformer-based encoders generally improve representation quality in multi-class semantic tasks, while high-quality linear baselines can remain competitive on balanced textual corpora.

#### 4.2.2 Diffusion performance comparison

Integrated DRIMUX outperformed Baseline DRIMUX and SIR in all key diffusion indicators:

- normalized AUC gain over Baseline DRIMUX: +36.98%
- final reach gain over Baseline DRIMUX: +18.47%
- normalized AUC gain over SIR baseline: +385.52%

Figure 4.4 confirms that integrated trajectories rise faster and remain above baselines throughout most of the simulation horizon. This indicates that semantic scaling strengthens both propagation speed and cumulative spread.

Figure 4.5 to Figure 4.8 further show that misinformation and activism consistently dominate neutral content across trajectory shape, activation bursts, endpoint reach, and total diffusion intensity. These results are consistent with studies in information diffusion showing that content semantics and framing influence propagation potential.

#### 4.2.3 Implications of comparative outcomes

The comparative results suggest that semantic-aware diffusion offers practical advantages over structure-only propagation models. In an operational setting, this means high-risk semantic content can be prioritized earlier for monitoring, verification, or intervention due to its higher predicted spread potential.

### 4.3 Performance Trade-offs Analysis

This section discusses key trade-offs observed from the results.

| Optimization Strategy | Improvement | Trade-off Observed |
|---|---|---|
| Transformer semantic model selection | Better deep-model F1 and clearer class separation | Higher architectural complexity than linear models |
| Semantic amplification in diffusion | Higher final reach and normalized AUC | Stronger sensitivity to stochastic network conditions |
| Content-aware scenario simulation | Clear differentiation of misinformation, activism, and neutral spread | Additional profiling and reporting overhead |

The experiments show a clear benefit-versus-cost pattern. Semantic integration improves diffusion realism and discriminatory power, but adds model and analysis complexity. Trial-level variability (including low-spread outlier runs) indicates that stochastic graph effects remain influential even with semantic scaling. However, integrated DRIMUX still maintained the best average performance, supporting its selection as the final framework.

From a thesis perspective, the optimal configuration is the one that balances predictive quality and interpretability with manageable computational complexity: Transformer as semantic source model plus integrated DRIMUX for diffusion simulation.

### 4.4 Chapter Summary

Chapter 4 demonstrates that the proposed framework satisfies the study objectives:

1. The semantic classification module achieved strong performance, with Transformer as the best deep model.
2. Semantic profiles were coherent and followed the intended risk ordering (misinformation > activism > neutral).
3. Integrated DRIMUX achieved superior diffusion outcomes versus Baseline DRIMUX and SIR.
4. The observed gains are meaningful despite stochastic variability and increased modeling complexity.

Overall, the evidence supports the effectiveness of integrating deep semantic inference into diffusion modeling for misinformation analysis.

Note: Replace literature-alignment sentences with your final cited sources from your related literature chapter to match your institutional citation format.
