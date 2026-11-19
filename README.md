# PACSHAP-FeatureGraph

**Privacy-aware feature selection: decide which personal data a model actually needs *before* it ever sees it.**

GDPR's data minimisation principle (Art. 5(1)(c)) says organisations should only process personal data that is necessary for a stated purpose. In machine learning that is hard to apply, because it is rarely obvious which features a model truly needs. This project explores a simple position: **minimise at the door, not inside the model.** Instead of training on everything and generalising inputs afterwards, it combines model explanations (SHAP), a feature-relationship graph, and learned representations (autoencoders) to filter and transform features up front, and then tests the result against real privacy attacks (membership and attribute inference) using IBM's Adversarial Robustness Toolbox.

The project started as a critical replication of Goldsteen et al. (2022), *Data minimization for GDPR compliance in machine learning models*, whose reference notebooks no longer ran against current libraries. I revived those notebooks, identified what I see as gaps in the approach, and built an alternative pipeline on the Adult and Nursery benchmark datasets.

> **Status:** research prototype, implemented as Jupyter notebooks. The core idea holds up; several measurements need tightening before the numbers can be treated as conclusive. Both sides are documented below, so you can judge for yourself.

---

## The idea in one picture

```
 raw features ──► 1. relevance graph  ─┐
                  2. SHAP importance  ─┼─► 3. hybrid selection ──► 4. autoencoder (+ adversary, + noise) ──► model
                                       │       (union / intersection)        latent features
                                       │
                         5. evaluation: membership inference, attribute inference, empirical leakage bound
```

1. **Relevance graph ("knowledge graph").** Features become nodes; an edge links a feature to the target when their correlation passes a threshold. It gives a quick, inspectable view of which features plausibly matter for the task.
2. **SHAP importance.** An XGBoost model is explained with TreeSHAP to rank features by their actual contribution to predictions.
3. **Hybrid selection.** Features flagged by the graph and/or SHAP are kept; everything else never enters the pipeline.
4. **Representation learning.** The selected features are compressed by an autoencoder. A variant trains it against an adversary that tries to recover a sensitive attribute, and adds Gaussian noise to the latent space.
5. **Attack-based evaluation.** Black-box membership inference and black-box / white-box attribute inference (ART), plus an empirical upper bound on attacker advantage.

## Where I disagree with the reference approach

Goldsteen et al. train a model on the full feature set and then use a decision tree fitted to the model's predictions to *generalise* inputs at inference time (e.g. replacing an exact age with a range), so that less precise personal data is needed when the model is used. My critique, which motivates this project:

- **Collection still happens.** Generalising at inference does not change what was collected and used for training. Minimising before training reduces what is stored and exposed in the first place.
- **One model at a time.** Real organisations reuse the same customer data across several models (credit, fraud, marketing). Minimisation should be checked per purpose, so a feature needed for fraud detection does not silently leak into marketing.
- **Decision-tree boundaries.** The generalisation is driven by axis-aligned tree splits, which may miss interactions that the original model relies on.
- **Manual, gradual disclosure.** The workflow asks users to disclose features step by step, which is hard to automate in large pipelines.
- **Limited adversarial evaluation.** Minimisation is argued to reduce risk, but the paper does not test the minimised models against inference attacks. This project does.

## Results

All numbers below are taken from the saved outputs of the notebooks in this repository (single run, fixed seed), except where marked.

### Adult (target: income; sensitive attribute: sex)

`proposed_method_implementation/hybrid_feature_privacy_attacks_adult.ipynb`

| Configuration | Features | Accuracy | Membership inference | Attribute inference (sex) |
|---|---|---|---|---|
| Baseline, all features | 14 | 81.65% ¹ | 72.88% ² | 84.19% ² |
| Graph + SHAP selection | 6 | 81.89% | 72.66% | 72.86% |
| Selection without `sex` + autoencoder | 5 → 6 latent | 80.93% | 55.53% | n/a |
| Autoencoder + latent noise (σ = 0.3), `sex` removed | 13 → 6 latent | 73.02% | n/a | 77.54% |

¹ From the revived Goldsteen minimisation notebook. ² Reported in an earlier version of this README; not traceable to a saved notebook output.

### Nursery (target: class; sensitive attribute: social = "problematic")

`proposed_method_implementation/hybrid_feature_privacy_attacks_nursery.ipynb`

| Configuration | Accuracy | Black-box attribute inference | White-box attribute inference | Empirical leakage bound (95%) |
|---|---|---|---|---|
| Baseline decision tree | 92.00% | 55.45% ³ | 60.82% ³ | n/a |
| Graph + SHAP selection | 89.99% | 69.75% | 69.75% | n/a |
| Adversarial autoencoder + noise σ = 0.8 | 81.35% | 63.05% | n/a | 15.07% |
| Same, stronger adversary (weight 6.0) | 78.03% | 63.96% | n/a | 15.97% |
| Differential-privacy variant | 43.60% | 66.07% | n/a | 18.09% |

³ See the notes below: the baseline attack targeted a different column.

### What the results support

- **Aggressive minimisation costs little accuracy on Adult.** Going from 14 features to 6 left accuracy unchanged (81.65% → 81.89%), and compressing 5 features into a 6-dimensional latent space still gave 80.93%. That is the central claim of the project, and it holds.
- **Removing a sensitive attribute is not enough on its own.** With `sex` removed, an attacker still recovered it about 78% of the time from the remaining features and embeddings. This is a useful, honest finding: proxies matter, which is exactly why relevance checks should look at relationships between features and not just drop columns.
- **The autoencoder pipeline coincided with a large drop in membership-inference success** (72.66% → 55.53%) at almost no accuracy cost. See the caveats below before reading this as causal.

## Another way to read these results

I want this repository to be useful to someone evaluating the method critically, so here is the strongest case against over-reading the numbers. None of this sinks the core idea; it marks what still needs to be shown.

- **Attack accuracy needs a majority-class baseline, not 50%.** The leakage bound is computed as *attack accuracy minus 0.5*. But about two-thirds of Adult records are male, and exactly one third of Nursery records have `social = problematic`. An attacker who always guesses the majority value already scores roughly 67% on both. Against that baseline, Adult attribute inference at 73–78% is a modest gain over guessing, and every Nursery attack after the autoencoder (63–66%) is *at or below* guessing.
- **Nursery cannot really test attribute inference from features.** Nursery is a full factorial dataset: every combination of feature values appears exactly once. So `social` is statistically independent of the other features by construction, and no attacker that sees only features can beat the majority guess. That explains why every configuration, from mild noise to a DP variant at 44% accuracy, lands in the same 63–66% band. A dataset with real correlations between the sensitive attribute and other features (Adult, COMPAS, or a credit dataset) is a better testbed.
- **Graph + SHAP selection *raised* attribute-inference success on Nursery** (55% → 70%), the opposite of what the method intends. Two notebook issues contribute: SHAP ranked `social` itself as important, so the sensitive attribute was *selected* as an input; and the baseline attack used column index 0, which after one-hot encoding is not `social`. On Adult, `sex` was likewise part of the selected set before being removed later.
- **The membership-inference numbers use different protocols.** The 72.66% figure trains the attack on one half of the data and tests it on the other half; the 55.53% figure trains and evaluates the attack on the same records. Both use unpruned decision trees, which memorise training data and are unusually vulnerable to membership inference. Part of the drop may come from the change of input space rather than from privacy protection. A fair comparison would hold the protocol fixed, report attack AUC alongside accuracy, and average over several seeds.
- **The "PAC Privacy" bound is an empirical confidence bound, not PAC Privacy.** The notebooks compute *attack accuracy − 0.5 + √(ln(2/δ)/2n)*, a Hoeffding interval on how well one specific attacker did. PAC Privacy (Xiao & Devadas, 2023) is a different, formal framework: it bounds what *any* adversary can recover by simulating the full data-processing mechanism and calibrating the noise to its output variance. The noise in these notebooks is set by hand (σ = 0.2–0.8). The empirical bound is still a meaningful audit metric, but it should be described as such, and a true PAC Privacy calibration is listed under next steps.
- **The "knowledge graph" is a correlation graph.** Edges come from Pearson correlation between label-encoded features and the target, so the graph adds little beyond a correlation filter, and label encoding makes correlations of nominal features (e.g. occupation) arbitrary. A real knowledge graph would bring in information the data does not contain: domain rules, purpose definitions, legal categories of data.
- **Feature selection was partly manual.** The Adult hybrid set is hand-picked after looking at the graph and SHAP outputs; the Nursery set is a union. An automated, reproducible selection rule is needed before the method can be called automatic.
- **The comparison chart in `Logic_Experiment/` is illustrative only.** Its accuracy and attack values are typed in by hand from different notebooks and datasets (DP on Nursery, anonymisation on Adult), its 88% hybrid accuracy does not match any notebook, and its ROC curves are drawn from synthetic scores generated to match chosen AUC values. It should not be read as an experimental result.
- **The company case study runs on synthetic data with random labels.** Because the labels are random, any accuracy above about 50% on them signals leakage or a bug rather than learning. The baseline's 100% comes from a deliberately leaky feature, and the hybrid notebook computes "membership inference accuracy" by comparing membership predictions with the *loan labels* rather than with membership ground truth. The case study is a good walkthrough of the multi-model scenario, but its numbers are not evidence.
- **On the backdoor argument.** Goldwasser et al. (2022) show that a *malicious model trainer* can plant undetectable backdoors. That is a supply-chain risk, and it is not reduced by giving the model fewer features. The stronger argument for minimising before training is simply the one GDPR makes: data that is never collected cannot leak.

## Next steps

These are the changes that would turn the prototype into a defensible result:

1. Use a fixed evaluation protocol for every configuration: held-out attack training and test sets, attack AUC plus accuracy, majority-class baselines, and 5–10 seeds with confidence intervals.
2. Use pruned or regularised target models, so that membership inference measures privacy rather than overfitting.
3. Automate selection with an explicit rule (for example, keep the smallest feature set within *x*% of baseline accuracy, and never select declared sensitive attributes).
4. Replace label encoding with one-hot or target encoding in the relevance graph, and enrich the graph with purpose-level rules, so it does more than a correlation filter.
5. Calibrate noise with the PAC Privacy algorithm of Xiao & Devadas rather than by hand, and report the resulting guarantee next to the empirical attack results.
6. Evaluate attribute inference on datasets where the sensitive attribute is genuinely correlated with other features.
7. Package the pipeline as a small Python module with one script per dataset.

## Repository structure

| Folder | Contents |
|---|---|
| `proposed_method_implementation/` | The proposed pipeline on Adult and Nursery: relevance graph, SHAP, selection, autoencoders, attacks. **Start here.** |
| `Case_Study/` | Multi-model company scenario (loan, fraud, marketing) on synthetic data. Illustrative walkthrough. |
| `Logic_Experiment/` | Design rationale as Q&A, plus an illustrative comparison chart (see caveats above). |
| `theoretical_foundations/` | Motivation and background for each component. |
| `archived_notebooks/` | Original reference notebooks accompanying Goldsteen et al. Kept for reference; they do not run with current libraries. |
| `modified_archived_notebooks/` | The same notebooks, updated to run with current library versions. |
| `Instructions_to_Run/` | Setup and run instructions. |

## Running the notebooks

```bash
git clone https://github.com/rahil1303/PACSHAP-FeatureGraph.git
cd PACSHAP-FeatureGraph
pip install -r requirements.txt
jupyter notebook proposed_method_implementation/hybrid_feature_privacy_attacks_adult.ipynb
```

The notebooks were developed in Google Colab (Python 3.11) and download the Adult and Nursery datasets directly from the UCI Machine Learning Repository, so an internet connection is needed. See `Instructions_to_Run/` for details.

## References

- Goldsteen, A., Ezov, G., Shmelkin, R., Moffie, M., & Farkash, A. (2022). Data minimization for GDPR compliance in machine learning models. *AI and Ethics*, 2, 477–491. https://doi.org/10.1007/s43681-021-00095-8
- Goldsteen, A., Saadi, O., Shmelkin, R., Shachor, S., & Razinkov, N. (2023). AI privacy toolkit. *SoftwareX*, 22, 101352. Code: https://github.com/ElsevierSoftwareX/SOFTX-D-22-00422
- Xiao, H., & Devadas, S. (2023). PAC Privacy: Automatic privacy measurement and control of data processing. *Advances in Cryptology, CRYPTO 2023*. https://arxiv.org/abs/2210.03458
- Lundberg, S. M., & Lee, S.-I. (2017). A unified approach to interpreting model predictions. *Advances in Neural Information Processing Systems (NeurIPS)*.
- Shokri, R., Stronati, M., Song, C., & Shmatikov, V. (2017). Membership inference attacks against machine learning models. *IEEE Symposium on Security and Privacy (S&P)*.
- Edwards, H., & Storkey, A. (2016). Censoring representations with an adversary. *International Conference on Learning Representations (ICLR)*.
- Nicolae, M.-I., et al. (2018). Adversarial Robustness Toolbox v1.0.0. https://arxiv.org/abs/1807.01069
- Goldwasser, S., Kim, M. P., Vaikuntanathan, V., & Zamir, O. (2022). Planting undetectable backdoors in machine learning models. *IEEE Symposium on Foundations of Computer Science (FOCS)*.
- Mienye, I. D., & Sun, Y. (2023). A machine learning method with hybrid feature selection for improved credit card fraud detection. *Applied Sciences*, 13(12), 7254. https://doi.org/10.3390/app13127254

## Contact

**Rahil Sharma** · [officialthingsrahil@gmail.com](mailto:officialthingsrahil@gmail.com) · [LinkedIn](https://www.linkedin.com/in/rahil-sharma-ba5b6718b) · [GitHub](https://github.com/rahil1303)
