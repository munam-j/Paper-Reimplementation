# Multimodal Emotion Recognition Using Cross-Modal Translation
## Overview

This repository implements the research paper **"Can We Exploit All Datasets? Multimodal Emotion Recognition Using Cross-Modal Translation"** (IEEE Access 2022). The system performs emotion recognition by integrating three modalities:

- **Visual**: Facial expressions from video frames
- **Audio**: Speech tone and patterns from audio
- **Text**: Transcript content from spoken words

### Key Features

- **6 VAEGAN translators** with weight sharing for all modality pairs
- **K+1 sequence discriminators** for unaligned multimodal data
- **Cycle consistency loss** ensuring translation quality
- **Missing modality handling** - train on ANY dataset regardless of available modalities
- **End-to-end training** with optimal 1:7 translator-to-classifier ratio
- **Cross-modal attention** for effective multimodal fusion

## Dataset

This implementation uses the **CMU-MOSI** dataset. 
<img width="399" height="440" alt="image" src="https://github.com/user-attachments/assets/c45270a3-8b90-4ae7-844d-36b30f75927a" />

