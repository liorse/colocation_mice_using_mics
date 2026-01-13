# Research Papers on Colocating Mice Using Microphones and Neural Networks

This document summarizes the key research papers on assigning/localizing mouse ultrasonic vocalizations using microphone arrays and neural networks.

---

## 1. Mountable Miniature Microphones to Identify and Assign Mouse Ultrasonic Vocalizations

**Authors:** Elena N. Waidmann et al.

**Published:** Cell Reports Methods, 2025; 5(6):101081

**DOI:** 10.1016/j.crmeth.2025.101081

### Summary
This paper introduces an inexpensive, ultrasound-sensitive wearable microphone system to identify which individual mouse produces ultrasonic vocalizations (USVs) during social interactions. The researchers developed mountable miniature microphones using commercially available MEMS microphone components ($0.60 per microphone) to track vocalizations in male-female mouse pairs.

### Key Findings
- Assigns approximately **90% of vocalizations** to a specific animal using amplitude differences alone
- Combined with video tracking for distance correction, accuracy reaches **97%**
- Male mice produce >96% of ultrasonic vocalizations in courtship pairs, while females produce <1%
- Males vocalize primarily during close proximity to females, particularly during ano-genital investigation
- Provides an inexpensive alternative to complex microphone array systems

### Download Links
- **PubMed Central (Open Access):** https://pmc.ncbi.nlm.nih.gov/articles/PMC12272246/
- **bioRxiv Preprint:** https://www.biorxiv.org/content/10.1101/2024.02.05.579003v1
- **ResearchGate:** https://www.researchgate.net/publication/378019829_Mountable_miniature_microphones_to_identify_and_assign_mouse_ultrasonic_vocalizations
- **Cell Reports Methods:** https://www.cell.com/cell-reports-methods/fulltext/S2667-2375(25)00117-1

---

## 2. Vocal Call Locator Benchmark (VCL) for Localizing Rodent Vocalizations

**Authors:** Ralph E. Peterson, Aramis Tanelus, Christopher Ick, Bartul Mimica, Niegil Francis, Violet J. Ivan, Aman Choudhri, Annegret L. Falkner, Mala Murthy, David M. Schneider, Dan H. Sanes, and Alex H. Williams

**Published:** Preprint 2024 (Presented at NeurIPS 2024)

**DOI:** 10.1101/2024.09.20.613758

### Summary
This preprint introduces the **first large-scale benchmark dataset** for sound source localization specifically designed for rodent vocalizations. The researchers compiled synchronized video and multi-channel audio recordings containing approximately **767,000 sounds** with annotated ground-truth source positions across nine different experimental conditions.

### Key Findings
- **Task 1:** Compares classical sound source localization algorithms with deep neural networks
- **Task 2:** Assigns vocalizations to individuals in a dyad (vocalization attribution)
- **Deep neural networks** consistently produced estimates closer to ground truth than classical methods (MUSE)
- Achieved **<1 cm error** on 80.6% and 66.0% of test sets for two datasets
- This level of resolution enables attribution of most vocalizations in realistic social encounters
- Future improvements may combine visual (pose) and acoustic information using transformers

### Dataset and Code
- **Dataset Website:** https://vclbenchmark.flatironinstitute.org
- **GitHub Repository:** https://github.com/neurostatslab/vocalocator

### Download Links
- **bioRxiv PDF (Direct):** https://www.biorxiv.org/content/10.1101/2024.09.20.613758v1.full.pdf
- **PubMed Central:** https://pmc.ncbi.nlm.nih.gov/articles/PMC11430026/
- **ResearchGate:** https://www.researchgate.net/publication/397198177_Vocal_Call_Locator_Benchmark_VCL_for_localizing_rodent_vocalizations_from_multi-channel_audio

---

## 3. HyVL - Hybrid Vocalization Localizer (✓ Downloaded)

**Authors:** Sterling et al.

**Published:** eLife, 2023

**DOI:** 10.7554/eLife.86126

**File:** `HyVL_rodent_ultrasonic_vocal_interaction.pdf` (4.3 MB - Downloaded successfully!)

### Summary
HyVL is a **hybrid ultrasonic tracking system** that synergistically integrates a high-resolution acoustic camera with high-quality ultrasonic microphones. This system represents a major advancement in precision for localizing ultrasonic vocalizations in rodents.

### Key Findings
- **First to achieve millimeter precision** (~3.4–4.8 mm, 91% assigned) in localizing USVs
- Approximately **3× better** than other systems
- Approaches the physical limits (mouse snout ~10 mm)
- Uses a 64-microphone 'acoustic camera' combined with 4 high-quality ultrasound microphones
- Both systems can individually localize USVs but exhibit complementary patterns of localization errors
- Systems are fused into a hybrid approach for optimal performance

### Download Links
- **eLife (Direct PDF):** https://elifesciences.org/articles/86126.pdf ✓
- **eLife Article:** https://elifesciences.org/articles/86126

---

## 4. High-Precision Spatial Localization of Mouse Vocalizations

**Authors:** Heckman et al.

**Published:** Scientific Reports, 2017

**DOI:** 10.1038/s41598-017-02954-z

### Summary
Researchers at Howard Hughes Medical Institute's Janelia Research Campus developed a **four-channel ultrasonic-microphone-array-based system** that enables the localization and assignment of vocalizations to socially interacting individual mice.

### Key Findings
- Novel algorithm **SLIM (Sound Localization via Intersecting Manifolds)**
- Achieves **2–3-fold improvement** in accuracy (13.1–14.3 mm) using only 4 microphones
- Extends to many microphones and localization in 3D
- Allows reliable assignment of **84.3% of all USVs**
- Provided first compelling evidence that **female mice vocally interact with males** during courtship

### Download Links
- **Scientific Reports:** https://www.nature.com/articles/s41598-017-02954-z
- **PMC:** https://pmc.ncbi.nlm.nih.gov/articles/PMC5462771/

---

## 5. Additional Relevant Papers

### VocalMat - CNN-based USV Analysis
**Published:** eLife, 2021

Uses computer vision and **convolutional neural networks (CNNs)** to classify USVs into 11 categories with ~86% accuracy.

**Link:** https://elifesciences.org/articles/59161

### Extended Performance Analysis of Deep-Learning Algorithms
**Published:** Nature Scientific Reports, 2023

Compares Auto-Encoder (AE), U-NET, and Recurrent Neural Networks (RNN) for mice vocalization segmentation, achieving >90% precision and recall.

**Link:** https://www.nature.com/articles/s41598-023-38186-7

### Automatic Classification Using Machine Learning and CNNs
**Published:** PLOS One, 2021

Tests Support Vector Machine, Random Forest, Multilayer Perceptrons, and CNNs for automatic USV classification.

**Link:** https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0244636

---

## Download Instructions

### Successfully Downloaded
- ✓ **HyVL paper:** `HyVL_rodent_ultrasonic_vocal_interaction.pdf`

### Manual Download Required
Due to website protections (Cloudflare, authentication requirements), the following papers need to be downloaded manually:

1. **Mountable Miniature Microphones paper:**
   - Visit: https://pmc.ncbi.nlm.nih.gov/articles/PMC12272246/
   - Click the "Download PDF" link at the top of the page
   - Save as: `Mountable_miniature_microphones_mice_USV.pdf`

2. **VCL Benchmark paper:**
   - Visit: https://www.biorxiv.org/content/10.1101/2024.09.20.613758v1
   - Click "Download PDF" button
   - Save as: `VCL_benchmark_rodent_vocalization_localization.pdf`

### Alternative: Use Browser Extensions or Academic Tools
- **Unpaywall:** Browser extension that finds free, legal PDFs
- **Institutional Access:** If you have university access, use your institution's library
- **ResearchGate:** Request full-text from authors

---

## Summary of Approaches

| Paper | Method | Accuracy | Hardware |
|-------|--------|----------|----------|
| Mountable Miniature Mics | Wearable MEMS mics + amplitude | 90-97% | $0.60/mic |
| VCL Benchmark | Deep Neural Networks | <1cm (80%) | Multi-channel array |
| HyVL | Hybrid: 64-mic camera + 4 high-quality mics | 3.4-4.8mm (91%) | 68 microphones |
| SLIM (Janelia) | Intersecting Manifolds algorithm | 13-14mm (84%) | 4 microphones |

---

## Key Insights for Implementation

1. **Trade-offs:**
   - **Wearable mics:** Most accurate (97%) but requires mounting on animals
   - **Microphone arrays:** Non-invasive but requires complex hardware and algorithms
   - **Neural networks:** Best for complex scenarios with many animals

2. **Cost Considerations:**
   - Wearable approach: ~$1-2 per animal
   - 4-mic array: ~$200-500
   - 64-mic acoustic camera: $10,000+

3. **Neural Network Advantages:**
   - Handle occlusion and complex acoustic environments better
   - Can integrate visual and acoustic information
   - Continuously improve with more training data

4. **Current State-of-the-Art:**
   - HyVL achieves best precision (3-4mm) for array-based systems
   - Wearable mics achieve best overall accuracy (97%) but invasive
   - Deep learning approaches (VCL) show promise for <1cm accuracy

---

## Recommended Reading Order

1. **Start with:** VCL Benchmark paper - provides overview of the field and benchmark comparisons
2. **Then read:** HyVL paper (already downloaded!) - state-of-the-art array-based approach
3. **Finally:** Mountable Miniature Microphones - innovative wearable approach

---

**Generated:** 2026-01-13
**Working Directory:** C:\Users\liors\Documents\colocation_mice_using_mics
