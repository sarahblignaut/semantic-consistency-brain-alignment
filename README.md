# semantic-consistency-brain-alignment
Replicating and extending Ryskina et al. (2025) to naturalistic fMRI (SemReps-8K)

# Semantic Consistency and Language Model Alignment in the Brain

MSc research internship, MAASAI team, Inria & Université Côte d'Azur (April-June 2026)
Supervisors: Prof. Frédéric Precioso, Dr. Samuel Deslauriers-Gauthier

> **Status:** Code is being cleaned up and will be released here [once approved by my supervisors].

## Question

Language model (LM) representations predict brain activity, but *where* in the brain is
that alignment strongest? Ryskina et al. (2025) introduced a **semantic consistency (SC)**
metric: a brain region has high SC if it responds similarly to the same concept whether it
is presented as text or as a picture, i.e. it encodes the concept rather than the sensory
modality. They found LM–brain alignment increases from low-SC to high-SC cortex, but only
on 180 isolated concept words (Pereira et al., 2018).

This project asks: **does that relationship generalize to naturalistic stimuli, and does
it depend on visual grounding?**

## Data

- **Pereira et al. (2018):** used to replicate the original pipeline first.
- **SemReps-8K (Nikolaus et al., 2026):** fMRI from 6 participants viewing naturalistic
  COCO scene images and reading their captions in separate trials; 23,272 single-trial
  training stimuli and 70 matched image–caption test pairs.
- The two stimulus sets barely overlap (Jaccard similarity = 0.03), so a replication
  reflects generalization, not shared content.

## Methods

- Computed SC per voxel as the correlation between caption and image responses, averaged
  within 354 HCP-MMP1 parcels, and split cortex into quartiles (Q1 = least, Q4 = most
  semantically consistent)
- Extracted every-layer representations from 16 transformer models: GPT-2 variants,
  Qwen2.5 base and instruct models, FLAVA, and vision-language models (VLMs) Qwen2.5-VL
  and LLaVA-1.5
- Brain encoding: cross-validated ridge regression per parcel (14 models)
- Representational similarity analysis (RSA) per SC quartile (16 models), with permuted
  baselines, permutation tests and FDR correction
- Ablation: re-ran each VLM with image input removed
- All pipelines run on the Grid'5000 HPC cluster

## Findings

- **Encoding generalizes:** accuracy rose from Q1 to Q4 for all 14 models (mean r = 0.026
  → 0.079; all p_FDR ≤ 0.005), regardless of architecture or scale.
- **Geometry needs vision:** only models receiving image input showed significant RSA
  alignment, and highest alignment in high-SC cortex. Text-only models did not survive
  correction.
- **Ablation:** removing image input from the VLMs reversed the Q4–Q1 RSA gradient
  (mean Δ = +0.024 → −0.007) and eliminated significance.

In short: text-only models can predict what semantically consistent regions do, but only
models with visual input organize concepts the *way* those regions do.

## Acknowledgements

Builds on the open-source pipeline of Ryskina et al. (2025). Data from Pereira et al.
(2018) and Nikolaus et al. (2026). Computation on Grid'5000.
