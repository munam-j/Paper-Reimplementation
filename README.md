# Multimodal Emotion Recognition Using Cross-Modal Translation
## Overview
Trains a trimodal (visual + audio + text) emotion classifier by simultaneously learning six cross-modal VAEGAN translators — one per directed pair among the three modalities — so that unimodal and bimodal auxiliary datasets can also contribute to the trimodal model.

## Results
<img width="1500" height="475" alt="image" src="https://github.com/user-attachments/assets/3adbe3c0-1e90-448b-a6b5-e78006da883e" />

Paper reports 52.7% WA on CMU-MOSEI. The ~20pp gap is almost entirely due to dataset constraints (MOSEI unavailable on Kaggle → MOSI proxy used; AFEW and IEMOCAP absent) and the backbone substitution (FAN → VGGFace2). 

## Dataset
This implementation uses the **CMU-MOSI** dataset. 

<img width="1500" height="655" alt="image" src="https://github.com/user-attachments/assets/f4434f53-c189-4c27-b430-e728c6dedcc4" />

The paper uses CMU-MOSEI (not available on Kaggle) as the primary dataset. CMU-MOSI is used as a substitute, mapping sentiment scores to happy (score > 1.0) and sad (score < −1.0). This is the largest single source of the accuracy gap.

All datasets share the unified 6-class MOSEI emotion space: happy=0  sad=1  anger=2  surprise=3  disgust=4  fear=5.

## Training Curriculum
Training follows Algorithm 1 from the paper with a warmup addition:
# Phase 1 — Warmup (epochs 1–3): 
Classifier-only training. Missing modalities are zero-padded rather than translated (random-initialised translators produce noisier features than zeros). This gives the classifier a stable prior before the GAN losses are introduced.
# Phase 2 — E2E co-training (epochs 4–10): 
Every 7th step fires the full Algorithm 1:
1. For each of the 6 directed translator pairs, compute KL + reconstruction + GAN + SeqDis + cycle losses and synthesise fake features.
2. Build modality pools: pool_a = [real_a, fake_va, fake_ta], similarly for v and t (3 entries each).
3. Run classifier on all 3×3×3 = 27 pool combinations and average the cross-entropy loss.
4. One backward pass covers all translator and classifier losses jointly.

Between E2E steps, the classifier runs with translated augmentation (use_translators=True).

## Outputs
After training, three files are written to /kaggle/working/:
<img width="1500" height="422" alt="image" src="https://github.com/user-attachments/assets/8219fac0-8a7c-4927-94dd-e9d690645e6c" />
<img width="1800" height="600" alt="training_curves (1)" src="https://github.com/user-attachments/assets/b1e763d3-ff0c-4e69-859f-aaf6d0802b37" />
<img width="1200" height="1050" alt="confusion_matrix_test (1)" src="https://github.com/user-attachments/assets/eddfa848-f64b-4f2b-84b5-6ceda70c6dd6" />

## Known Deviations from the Paper
<img width="1500" height="802" alt="image" src="https://github.com/user-attachments/assets/661e84ca-32d7-42f4-b477-211e6b66993d" />








