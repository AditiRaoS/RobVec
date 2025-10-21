
# RobVec: Robust Self-Supervised Speech Representation Learning

RobVec is a robust self-supervised learning framework for speech representation learning designed to perform effectively across noisy and low-resource conditions. Inspired by wav2vec 2.0, RobVec enhances the robustness of learned representations by introducing noise-aware training objectives and data augmentation strategies. This repository provides the training pipeline, data preparation methodology, and evaluation scripts to reproduce the results reported in our work.

---

## 📚 Background

Self-supervised learning (SSL) has transformed automatic speech recognition (ASR) by allowing models to learn powerful acoustic representations from raw, unlabeled audio. Despite the success of models like wav2vec 2.0 and HuBERT, their performance often degrades in real-world conditions with background noise, reverberation, or limited transcriptions.

RobVec addresses this gap by integrating noise robustness directly into the pretraining stage. It uses paired clean and noise-augmented audio and introduces a cosine similarity loss in addition to the standard contrastive and diversity losses. This approach enforces consistency in representations across clean and noisy conditions without requiring external denoising modules.

---

## 🚀 Features

* Noise-invariant representation learning with dual-input pretraining.
* Augmentation pipeline: additive Gaussian noise, temporal shuffling, masking, and silence injection.
* Cosine similarity loss to align clean and noisy representations.
* Compatible with base and large configurations of wav2vec 2.0.
* Evaluation across various LibriSpeech fine-tuning splits (1h, 10h, 100h, 960h) and FreeSound noisy datasets.

---

## 🛠️ Training Specifications

### Model Configurations

| Parameter              | Base | Large |
| ---------------------- | ---- | ----- |
| Transformer blocks     | 12   | 24    |
| Embedding dimension    | 768  | 1024  |
| Attention heads        | 8    | 16    |
| Feed-forward dimension | 3072 | 4096  |
| Dropout                | 0.1  | 0.1   |

### Training Details

* Optimizer: Adam
* Learning Rate: 5e-4 with linear warmup
* Effective Batch size: 32
* Mask probability: 0.065
* Mask length: 10 frames
* Number of updates: 400k
* Loss: Contrastive + Diversity + Cosine Similarity

---

## 📦 Dataset & Augmentation

### Pretraining

* **LibriSpeech (clean 100h/360h/960h)**: Primary pretraining corpus
* **FreeSound**: Environmental noise samples
* **TIMIT**: Used for phoneme-level evaluation

### Augmentation Pipeline

* Additive white Gaussian noise 
* Random temporal shuffling 
* Frame masking with values
* Silence insertion

---

## 📊 Results

### Fine-tuning on LibriSpeech

RobVec consistently outperforms baseline models, particularly under low-resource and noisy settings.

| Fine-tuning Split | Model        | test-clean | test-other |
| ----------------- | ------------ | ---------- | ---------- |
| 1h                | RobVec BASE  | 5.7        | 11.2       |
| 10h               | RobVec BASE  | 4.3        | 9.6        |
| 100h              | RobVec BASE  | 3.5        | 8.0        |
| 960h              | RobVec BASE  | 2.3        | 3.4        |
| 960h              | RobVec LARGE | 2.0        | 3.3        |

### Noisy Fine-tuning (FreeSound)

| Model                   | Avg. WER  |
| ----------------------- | --------- |
| wav2vec 2.0 (Clean)     | 56.81     |
| wav2vec 2.0 (FreeSound) | 27.92     |
| EW2                     | 23.85     |
| RobVec BASE (ours)      | **20.89** |

RobVec achieves statistically significant improvements (p < 0.001) in ANOVA tests across all fine-tuning splits, confirming its effectiveness in diverse acoustic scenarios.

---



## 📬 Contact

For questions, issues, or collaboration inquiries, please reach out via [GitHub Issues](https://github.com/AditiRaoS/RobVec/issues) or email at `aditisheshagirirao@gmail.com`.

---

## 🧠 Acknowledgements

This project builds on the foundations of wav2vec 2.0 (Meta AI), HuBERT, and the Hugging Face Transformers ecosystem. We also thank the authors of EW2 and MS-HuBERT for their open-source contributions.
