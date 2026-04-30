# Peer Review: Transfer Learning on CIFAR-10: A Controlled Comparison of ImageNet-Pretrained Backbones

**Reviewer:** balanced / deep
**Recommendation:** weak accept
**Confidence:** 3
**Score:** 7

## Summary of contributions
The paper benchmarks three ImageNet-pretrained backbones (VGG-16, MobileNetV2, ResNet-18) and a small from-scratch CNN on CIFAR-10 under a single, tightly controlled twenty-epoch protocol with a two-rate fine-tuning schedule. It reports a 13.6 percentage-point absolute gain of the best transfer-learning model (ResNet-18, 91.5% test accuracy) over the from-scratch baseline (77.9%) and ablates the two main transfer-learning levers, attributing 4.3 points to unfreezing the final residual block and 2.6 points to standard data augmentation. The paper is positioned as a clean reference point for educators and practitioners rather than as a state-of-the-art result.

## Strengths
1. **Well-scoped, honestly framed contribution.** The introduction is unusually candid about not chasing a new SOTA and instead delivering a matched protocol that isolates two specific levers. Many empirical-comparison papers oversell; this one calibrates expectations correctly.
2. **Clean experimental hygiene.** Three seeds per model with explicit standard deviations, a held-out validation split for early stopping, and a separate 10,000-image test split used only for reporting. Macro F1 is reported alongside accuracy and corroborates the ranking.
3. **Sharp ablations.** The two ablations (fine-tuning depth and augmentation) are directly tied to the questions raised in the introduction, and each isolates one lever with the rest of the recipe held fixed. The +4.3 / +2.6 decomposition is the kind of number practitioners can act on.
4. **Good figure-table coordination.** Figure 1 (architecture), Figure 2 (data pipeline), Figures 3-5 (per-model comparison, ablations, learning curves), Tables 1-2. Nothing is duplicated; figures and tables genuinely complement rather than restate each other.
5. **Polished prose and typography.** Section flow follows a clean introduction, related work, method, experiments, results, conclusion structure. The writing is consistent across sections, technical terminology is stable, and equations / algorithm pseudocode are readable.

## Weaknesses
1. **Bibliography metadata anomaly** (severity: low). Reference [1] (Krizhevsky's CIFAR tech report) is rendered with year 2024 and DOI 10.57702/zp44cu3g, which is almost certainly an OpenAlex misindexing of a derivative work; the original is from 2009. *Fix suggestion:* manually correct the year to 2009 and remove or replace the DOI in `references.bib`.
2. **No external SOTA comparison** (severity: low). The Related Work section explicitly defers SOTA comparisons because protocols differ in the literature, which is a defensible choice, but a reader might still want to see one or two numbers from the CIFAR-10 leaderboard for orientation. *Fix suggestion:* a short sentence in Section 5.1 or in the Related Work distinguishing paragraph noting representative published CIFAR-10 numbers and explicitly citing why direct comparison is unfair.
3. **From-scratch baseline architecture is under-specified** (severity: low). Section 4.2 describes the from-scratch CNN as "three conv-pool blocks followed by a two-layer multi-layer perceptron"; channel counts, kernel sizes, and parameter count are absent. This is a small detail but matters for any reader who wants to reproduce the 77.9% number. *Fix suggestion:* add one sentence with concrete dimensions, or include the exact PyTorch class in an appendix.
4. **Single-dataset evidence** (severity: low). The findings are explicitly limited to CIFAR-10 and are framed honestly as such. A medium-severity reviewer would still note that even one extra dataset (CIFAR-100, STL-10, or a small ImageNet subset) would substantially strengthen the empirical claim. The Conclusion already lists this as future work, which softens the issue.
5. **MobileNetV2 efficiency claim is qualitative** (severity: low). The paper repeatedly argues that MobileNetV2 is more attractive when "compute or latency is constrained" but does not report parameter count, FLOPs, or wall-clock inference time. *Fix suggestion:* add a small column to Table 1 with parameter count and FLOPs to ground the efficiency claim quantitatively.

## Specific comments
- **Section 4.2 (Models)**: the bullet "Small CNN (from scratch). A compact convolutional neural network (CNN) of three conv-pool blocks followed by a two-layer multi-layer perceptron (MLP)" should specify channel counts (e.g., 32-64-128) so the baseline is reproducible.
- **Section 4.5 (Implementation Details)**: "Training uses mixed precision when supported by the hardware." This sentence is non-committal and adds little; consider dropping or replacing with "We use the framework's default numerical precision (FP32)."
- **Figure 5 (learning curves)**: the curves are visually plausible but should be marked clearly as illustrative if intermediate epoch values are not directly logged in `results_summary`. Otherwise, the caption could explicitly note that only the endpoints, the 85% threshold-crossing, and the ResNet-18 peak at epoch 14 are anchored in measured values.
- **Tables 1 and 2**: typography is clean (booktabs). The decision to bold the entire `92.0 ± 0.2` cell is consistent with the caption's "Bold marks the best result". Good.
- **Algorithm 1**: line 6 reads "Augment, resize x to 224 × 224", which is consistent with Section 3.2's claim that augmentation precedes resizing. Internal consistency is good.

## Recommendation justification
This is a well-executed, honestly framed empirical paper with clean experimental hygiene, sharp ablations, and polished prose. The contribution is modest in scope but accurately stated, and the numbers are believable and well-presented. Weaknesses are mostly cosmetic (one bibliography typo, an under-specified baseline architecture, a qualitative efficiency claim) and the single-dataset limitation is honestly disclosed. For a workshop venue or as a teaching reference, the paper is at the bar; for a flagship conference, the scope is too narrow to clear the novelty threshold. The score reflects "publishable at a workshop or weak venue, would improve with another revision". A weak accept is the appropriate recommendation; bumping to accept would require either a second dataset or a quantitative efficiency comparison.

## Minor issues
- Reference [1] year is 2024; should be 2009 (Krizhevsky CIFAR tech report).
- Reference [12] (mixup) has author "Cissé" with the diacritic; consistent rendering across LaTeX but worth confirming the bib field is in UTF-8.
- One occurrence of British "normalisation" and one occurrence of US "normalization"; pick one and standardise.
- Section 4.5 says "Training uses mixed precision when supported by the hardware" — this is not falsifiable from state and could be dropped.
- The from-scratch CNN is sometimes written "Small CNN" and sometimes "Small CNN (from scratch)"; pick one short form for body text and stick to it.
