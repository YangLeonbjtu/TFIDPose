# TFIDPose

<div align="center">

<h2>Training-Free Instance Disambiguation for Open-Vocabulary Relative 6D Pose Estimation</h2>

<p>
  <strong>Chunlong Yang</strong>,
  <strong>Chunjie Zhang</strong><sup>*</sup>,
  <strong>Guodong Yang</strong>,
  <strong>Wei Wang</strong>,
  <strong>Xiaolong Zheng</strong>
</p>

<p>
  Beijing Jiaotong University &nbsp;|&nbsp; Institute of Automation, Chinese Academy of Sciences
</p>

<p>
  <sup>*</sup>Corresponding author
</p>

<p>
  <a href="https://github.com/YangLeonbjtu">
    <img src="https://img.shields.io/badge/GitHub-YangLeonbjtu-black?logo=github" alt="GitHub">
  </a>
  <a href="https://scholar.google.com/citations?user=EqFOt3kAAAAJ">
    <img src="https://img.shields.io/badge/Google%20Scholar-Profile-blue?logo=googlescholar" alt="Google Scholar">
  </a>
  <a href="#">
    <img src="https://img.shields.io/badge/Paper-Coming%20Soon-red" alt="Paper">
  </a>
  <a href="#">
    <img src="https://img.shields.io/badge/Code-Coming%20Soon-lightgrey" alt="Code">
  </a>
</p>

</div>

---

## 🔥 Overview

<div align="center">
  <img src="assets/teaser.png" width="95%">
</div>

**TFIDPose** is a training-free framework for **open-vocabulary relative 6D pose estimation**.

Given an anchor RGB-D image, a query RGB-D image, and a text prompt, TFIDPose estimates the relative 6D pose of the target object across different scenes. The key challenge is **cross-image same-category instance ambiguity**. When multiple visually similar objects appear in the query scene, existing methods may select the wrong instance and produce an incorrect pose.

Our core idea is simple:

> **Instance selection and geometric matching should not be forced into a single coupled representation.**

TFIDPose explicitly decouples these two stages. It first proposes multiple plausible anchor-query instance pairs, then uses geometric consistency to verify which pair is correct.

---

## ✨ Highlights

* **Training-free**: no task-specific fine-tuning is required.
* **Open-vocabulary**: target objects are specified by text prompts.
* **Instance-disambiguated**: handles same-category multi-instance ambiguity.
* **Geometry-aware**: final selection is decided by 3D geometric consistency.
* **Hypothesis-then-verify**: appearance proposes candidates, geometry makes the final decision.

---

## 🧠 Method

<div align="center">
  <img src="assets/pipeline.png" width="100%">
</div>

TFIDPose follows a simple **hypothesis-then-verify** pipeline.

### 1. Candidate Generation

We use **GroundingDINO** to detect text-specified candidate objects and **SAM2** to obtain instance masks.

### 2. Appearance Hypothesis Ranking

For each candidate instance, we extract mask-pooled **DINOv2** descriptors. Candidate pairs are ranked by instance-aware appearance similarity, and the top-K pairs are retained as hypotheses.

### 3. Geometric Matching

For each hypothesis, **RoMa** generates dense correspondences between the anchor and query candidates. Instance masks are used to filter background and distractor matches.

### 4. Pose Estimation

We back-project valid correspondences into 3D, estimate the relative pose with **RANSAC**, and select the hypothesis with the strongest geometric support.

---

## 🔑 Core Insight

Appearance similarity is useful for proposing candidates, but it is not reliable enough to make the final decision under same-category ambiguity.

Different instances from the same category may look similar. However, only the correct anchor-query pair should support a coherent rigid transformation between their 3D point sets.

Therefore, TFIDPose uses:

```text
DINOv2 appearance similarity  →  candidate hypothesis ranking
RoMa + RANSAC geometry        →  final instance and pose selection
```

---

## 🛠️ Installation

The code will be released soon.

```bash
git clone https://github.com/YangLeonbjtu/TFIDPose.git
cd TFIDPose
```

---

## 🚀 Usage

A demo script will be provided after code release.

```bash
python demo.py \
  --anchor path/to/anchor_rgbd \
  --query path/to/query_rgbd \
  --prompt "a white small bowl"
```

---

## 📁 Repository Structure

```text
TFIDPose/
├── assets/
│   ├── teaser.png
│   └── pipeline.png
├── README.md
└── code coming soon
```

---

## 📚 Citation

If you find this project useful, please consider citing our work.

```bibtex
@article{yang2026tfidpose,
  title   = {TFIDPose: Training-Free Instance Disambiguation for Open-Vocabulary Relative 6D Pose Estimation},
  author  = {Yang, Chunlong and Zhang, Chunjie and Yang, Guodong and Wang, Wei and Zheng, Xiaolong},
  journal = {arXiv preprint},
  year    = {2026}
}
```

---

## 📬 Contact

For questions, please contact:

**Chunlong Yang**
GitHub: [YangLeonbjtu](https://github.com/YangLeonbjtu)
Google Scholar: [EqFOt3kAAAAJ](https://scholar.google.com/citations?user=EqFOt3kAAAAJ)

---
