# FFD

> FFD: Rethinking Optical-SAR Fusion Object Detection from a Feature-First Perspective

[![Paper](https://img.shields.io/badge/Paper-%202026-blue)](#citation)
[![Task](https://img.shields.io/badge/Task-Optical--SAR%20Fusion%20Detection-green)](#)
[![Backbone](https://img.shields.io/badge/Backbone-DINOv3-orange)](#)
[![License](https://img.shields.io/badge/License-TBD-lightgrey)](#license)

This repository contains the official implementation of **FFD**.

Vision foundation models provide strong general representations, but they are not naturally aware of cross-modal optical-SAR alignment, SAR physical imaging priors, or the extreme scale variation in remote sensing scenes. FFD bridges this gap by injecting task-specific priors into a frozen VFM and performing balanced optical-SAR feature fusion.

## News
- The code will be made public once the paper is accepted (2026.10.07).

## Highlights

- A feature-first paradigm shifts OSFD from how to fuse toward what to fuse.
- Task-oriented priors are learned from SAR cues and multi-scale object structures.
- Task-prior guidance adapts frozen VFM representations to optical-SAR detection.
- Modality-balanced fusion improves integration of optical and SAR features.
- FFD achieves state-of-the-art results on three optical-SAR detection benchmarks.

## Framework

![FFD framework](https://github.com/wchao0601/FFD/blob/main/network.png)

FFD follows a two-stage design:

1. **Task-prior learning.** TPLM is pretrained on optical-SAR data to consolidate task-specific physical and spatial priors.
2. **VFM adaptation and fusion.** A frozen VFM extracts general optical/SAR features. TPLM priors are injected via TPGM, then optical and SAR features are fused by MBFM and sent to an oriented bounding box detection head.

## Method
![Task-oriented Prior Adapter](https://github.com/wchao0601/FFD/blob/main/adapter.png)
### Task-Prior Learning Module

**TPLM** serves as a task-specific knowledge carrier for OSFD. It contains:

- **PPEM: Physical Prior Enhancement Module**  
  Models SAR backscattering characteristics with ratio-of-averages style gradient priors to suppress multiplicative speckle noise.

- **MKEM: Multi-scale Knowledge Enhancement Module**  
  Decouples feature channels into heterogeneous branches for point, local, medium-range, and global attention, improving perception of objects with large-scale variation and irregular shapes.

### Task-Prior Guidance Mechanism

**TPGM** adaptively injects task-specific priors into VFM features. This prevents physical/spatial priors from overwhelming the general representation learned by the foundation model.

### Modality-Balanced Fusion Module

**MBFM** learns to balance optical texture/color cues and SAR structural cues, producing discriminative fused features for oriented object detection.

## Main Results



## Qualitative Results

![Detection comparison](https://github.com/wchao0601/FFD/blob/main/detect.png)

![Heatmap comparison](https://github.com/wchao0601/FFD/blob/main/heatmap.png)

## Installation

```bash
git clone https://github.com/wchao0601/FFD.git
cd FFD

conda create -n ffd python=3.10 -y
conda activate ffd

pip install -r requirements.txt
```

## Dataset Preparation

Please organize the datasets as follows:

```text
datasets/
+-- OGSOD-1.0/
|   +-- rgb/
|       +-- images/
|           +-- train/
|           +-- test/
|       +-- labels/
|           +-- train/
|           +-- test/
|   +-- sar/
|       +-- images/
|           +-- train/
|           +-- test/
|       +-- labels/
|           +-- train/
|           +-- test/
+-- OGSOD-2.0/
+-- M4-SAR/
```

Supported benchmarks:

| Dataset | Resolution | Classes | Train / Val / Test |
|---|---:|---|:---:|
| OGSOD-1.0 | 256 x 256 | Bridge, Harbor, Oil-Tank | 14,665 / - / 3,666 |
| OGSOD-2.0 | 256 x 256 | Bridge, Harbor, Oil-Tank | 14,250 / 2,035 / 4,047 |
| M4-SAR | 512 x 512 | Bridge, Harbor, Oil-Tank, Playground, Airport, Wind-Turbine | 56,116 / 22,112 / 33,946 |

## Training

### Train FFD

```bash
python tools/train.py
```

## Evaluation

```bash
python tools/test.py
```

## Model Zoo

## Citation

If this work is helpful for your research, please consider citing:

```bibtex
@article{wang2026topnet,
  title={FFD: Task-oriented Prior Adapter of Vision Foundation Models for Optical-SAR Fusion Detection},
  author={Wang, Chao and Yu, Zhenbo and Sun, Yanguang and Yang, Jian and Luo, Lei},
  year={2026}
}
```

## Acknowledgements

This project builds on prior research in optical-SAR fusion detection, vision foundation models, parameter-efficient tuning, and oriented object detection. We thank the authors of the related open-source projects and datasets.

## License

The license will be released with the code.
