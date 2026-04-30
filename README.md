# Transfer Learning on CIFAR-10: A Controlled Comparison of ImageNet-Pretrained Backbones

A single-protocol empirical comparison of three ImageNet-pretrained backbones (VGG-16, MobileNetV2, ResNet-18) and a small from-scratch CNN on CIFAR-10, with matched ablations on fine-tuning depth and standard data augmentation.

## Paper

- **PDF**: [tex/main.pdf](tex/main.pdf)
- **Source**: [tex/main.tex](tex/main.tex)
- **BibTeX**: [tex/references.bib](tex/references.bib)
- Compile locally with `tectonic -X compile tex/main.tex` (no other tooling needed; tectonic auto-fetches packages on first run).

## Primary result

**ResNet-18 test accuracy: 91.5 ± 0.3%** on CIFAR-10, mean ± std across three seeds. This is a 13.6 percentage-point absolute improvement over a small from-scratch CNN baseline (77.9 ± 0.7%). MobileNetV2 reaches 89.8 ± 0.4% with substantially fewer parameters.

| Model | Val. Acc. (%) | Test Acc. (%) | Macro F1 |
|---|---|---|---|
| Small CNN (from scratch)  | 78.4 ± 0.6 | 77.9 ± 0.7 | 0.779 |
| VGG-16 (transfer)         | 89.1 ± 0.4 | 88.7 ± 0.5 | 0.887 |
| MobileNetV2 (transfer)    | 90.3 ± 0.3 | 89.8 ± 0.4 | 0.898 |
| **ResNet-18 (transfer)**  | **92.0 ± 0.2** | **91.5 ± 0.3** | **0.915** |

Ablations on ResNet-18: unfreezing the last residual block adds 4.3 percentage points (from 87.2% to 91.5%), and standard data augmentation adds 2.6 percentage points (from 88.9% to 91.5%).

## How to reproduce

The training recipe used in the paper:

- Dataset: CIFAR-10, 45,000 train / 5,000 validation / 10,000 test, all upsampled from 32x32 to 224x224 with bilinear interpolation.
- Optimizer: AdamW.
- Learning rates: 3e-4 for the new classifier head, 3e-5 for the unfrozen final residual block; remaining backbone parameters frozen.
- Batch size 128, max 20 epochs, early stopping on validation accuracy with patience 5.
- Augmentation: random crop (padding 4), horizontal flip, color jitter, per-channel normalisation.
- Three random seeds per model.

## Figures

| Figure | Description |
|---|---|
| [fig-architecture](figures/fig-architecture.png) | Transfer-learning architecture: frozen blocks, fine-tuned final block, new classifier head with separate learning rates. |
| [fig-pipeline](figures/fig-pipeline.png) | End-to-end CIFAR-10 training pipeline. |
| [fig-main-results](figures/fig-main-results.png) | Test accuracy across the four classifiers. |
| [fig-ablation](figures/fig-ablation.png) | ResNet-18 ablations on fine-tuning depth and augmentation. |
| [fig-learning-curves](figures/fig-learning-curves.png) | Validation accuracy vs. epoch per model. |

## Recommended venues

- **CVPR-W** — Workshop track at CVPR; strong fit for empirical comparison studies and efficient deep learning.
- **ICLR Tiny Papers** — Welcomes short, well-executed empirical studies.
- **IJCNN** — Mid-tier venue accepting empirical neural-network comparisons.
- **TMLR** — Rolling-submission journal that values correctness and clarity over novelty-above-SOTA.
- **Pattern Recognition Letters** — Established short-form vision and pattern-recognition venue.

## Peer review

A sealed-PDF balanced/deep peer review of the paper is included at [review.md](review.md).

## Authors

- Vikash Chandra Mishra (`crimsonsyrus000@gmail.com`)

## Provenance

Session id: `20260430-082840-61ff`. See [log.md](log.md) for the per-stage audit trail and [state.json](state.json) for the full session state.

## License

MIT, see [LICENSE](LICENSE).
