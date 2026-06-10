# TFIDPose

<div align="center">

<h2>Training-Free Instance Disambiguation for Open-Vocabulary Relative 6D Pose Estimation</h2>

**Chunlong Yang**
Ph.D. Student | 3D Computer Vision | 6D Pose Estimation | Embodied AI

[![Project](https://img.shields.io/badge/Project-Page-blue)](#)
[![Paper](https://img.shields.io/badge/Paper-PDF-red)](#)
[![Code](https://img.shields.io/badge/Code-Coming%20Soon-lightgrey)](#)
[![GitHub](https://img.shields.io/badge/GitHub-YangLeonbjtu-black)](https://github.com/YangLeonbjtu)

</div>

---

## 🔥 Overview

**TFIDPose** is a training-free framework for open-vocabulary relative 6D pose estimation.
Given an anchor RGB-D image, a query RGB-D image, and a text prompt, the goal is to estimate the relative 6D pose of the target object across different scenes.

Existing open-vocabulary methods often couple instance selection and geometric matching into a single representation. This design is vulnerable when multiple same-category instances appear in the query scene. TFIDPose addresses this problem by explicitly decoupling instance disambiguation from geometric correspondence.

---

## 🧠 Key Idea

<div align="center">

<img src="assets/teaser.png" width="90%">

</div>

TFIDPose follows a simple **hypothesis-then-verify** pipeline:

1. **Candidate generation** using GroundingDINO and SAM2
2. **Instance-aware ranking** using DINOv2 mask-pooled descriptors
3. **Geometric verification** using RoMa dense correspondence and RANSAC
4. **Relative pose estimation** from the geometrically consistent hypothesis

This design allows the method to resolve cross-image same-category instance ambiguity without task-specific training.

---

## ✨ Highlights

* **Training-free**: no task-specific fine-tuning is required.
* **Open-vocabulary**: objects are specified by text prompts.
* **Instance-disambiguated**: robust to multiple same-category objects.
* **Geometry-aware**: final selection is decided by 3D geometric consistency.
* **Modular design**: built on frozen foundation models and easily upgradable.

---

## 📌 Method

<div align="center">

<img src="assets/pipeline.png" width="95%">

</div>

TFIDPose consists of three main stages:

### 1. Candidate Instance Generation

We first use GroundingDINO to detect text-specified object candidates and SAM2 to obtain instance masks.

### 2. Appearance-Based Hypothesis Ranking

For each candidate instance, we extract mask-pooled DINOv2 descriptors. Candidate pairs are ranked by instance-aware appearance similarity, and the top-K hypotheses are retained.

### 3. Geometry-Based Verification

For each hypothesis, RoMa produces dense correspondences. We filter correspondences using instance masks, back-project them to 3D, and estimate the relative pose with RANSAC. The hypothesis with the strongest geometric support is selected.

---

## 📊 Results

| Dataset    |        Setting |   AR ↑ | ADD(-S) ↑ |
| ---------- | -------------: | -----: | --------: |
| REAL275    | Predicted Mask | **--** |    **--** |
| YCB-V      | Predicted Mask | **--** |    **--** |
| REAL275-IA | Predicted Mask | **--** |    **--** |
| YCB-V-IA   | Predicted Mask | **--** |    **--** |

> Results will be updated after paper release.

---

## 🖼️ Qualitative Results

<div align="center">

<img src="assets/qualitative.png" width="95%">

</div>

TFIDPose can select the correct target instance from multiple same-category candidates and recover a geometrically consistent relative pose.

---

## 🛠️ Installation

```bash
git clone https://github.com/YangLeonbjtu/TFIDPose.git
cd TFIDPose
```

More installation details will be released soon.

---

## 🚀 Usage

```bash
python demo.py \
  --anchor path/to/anchor_rgbd \
  --query path/to/query_rgbd \
  --prompt "mug"
```

---

## 📁 Repository Structure

```text
TFIDPose/
├── assets/              # figures used in README
├── configs/             # configuration files
├── data/                # example data
├── models/              # model wrappers
├── scripts/             # running scripts
├── demo.py              # demo entry
└── README.md
```

---

## 📚 Citation

```bibtex
@article{yang2026tfidpose,
  title   = {TFIDPose: Training-Free Instance Disambiguation for Open-Vocabulary Relative 6D Pose Estimation},
  author  = {Yang, Chunlong},
  journal = {arXiv preprint},
  year    = {2026}
}
```

---

## 📬 Contact

For questions, please contact:

**Chunlong Yang**
GitHub: [YangLeonbjtu](https://github.com/YangLeonbjtu)

---
