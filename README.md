# Mouse Ultrasonic Vocalization Localization using Microphone Arrays

This repository contains research papers, datasheets, and technical documentation for building a system to localize and attribute ultrasonic vocalizations (USVs) from mice using microphone arrays and neural networks.

## Overview

Mice produce ultrasonic vocalizations (30-110 kHz) during social interactions. This project explores methods to:
- **Localize** the spatial position of vocalizations with millimeter precision
- **Attribute** vocalizations to individual mice in multi-animal settings
- **Analyze** vocal interactions during courtship and social behavior

## Repository Contents

### Research Papers

- **`HyVL_rodent_ultrasonic_vocal_interaction.pdf`** - Hybrid Vocalization Localizer (eLife, 2023)
  - State-of-the-art system achieving 3.4-4.8 mm precision
  - Uses 64-microphone Cam64 array + 4 high-quality microphones
  - 91% assignment rate of vocalizations to specific mice

### Hardware Documentation

- **`mic_SPH0641LU4H-1.pdf`** - Knowles MEMS Microphone Datasheet
  - Digital PDM (Pulse Density Modulation) output
  - Ultrasonic mode: 3.072-4.8 MHz clock frequency
  - SNR: 64.3 dB(A), suitable for ultrasonic recording

### Summary Document

- **`Research_Papers_Summary.md`** - Comprehensive overview of multiple research papers on mouse vocalization localization, including:
  - Mountable Miniature Microphones (Cell Reports, 2025) - 90-97% accuracy with wearable mics
  - VCL Benchmark (2024) - Deep neural networks achieving <1cm error
  - SLIM Algorithm (Scientific Reports, 2017) - 13-14mm precision with 4 microphones

## Technical Approach

### Hardware Options

| Approach | Precision | Cost | Invasiveness |
|----------|-----------|------|--------------|
| **Wearable MEMS mics** | 97% accuracy | ~$1-2/animal | High (mounted on animals) |
| **4-mic array (SLIM)** | 13-14 mm | ~$200-500 | Non-invasive |
| **64-mic array (Cam64/HyVL)** | 3.4-4.8 mm | ~$10,000+ | Non-invasive |
| **Deep learning (VCL)** | <1 cm | Varies | Non-invasive |

### Beamforming for Localization

This system uses **beamforming** - a spatial filtering technique that uses time differences of arrival (TDOA) between microphones to determine sound source location:

1. **Capture**: All 64 microphones record synchronized digital audio (PDM format)
2. **Delay-and-Sum**: Align signals based on expected delays from target position
3. **Triangulation**: Calculate (x, y) coordinates from time delays
4. **Attribution**: Assign vocalization to specific mouse based on position

**Critical requirement**: All microphones must share the same clock signal for phase-coherent recording.

### Sampling Requirements

For 80 kHz ultrasonic signals:
- **Nyquist minimum**: >160 kHz sampling rate
- **Recommended**: 400-500 kHz for 5× oversampling
- **Clock frequency**: 3.2-4.8 MHz (ultrasonic mode)
- **Decimation factor**: 8-12 (PDM to PCM conversion)

### Implementation Options

#### Option 1: FPGA (Recommended)
- **Hardware**: Xilinx Artix-7 or Intel Cyclone V
- **Advantages**: Parallel processing, real-time beamforming
- **Data bandwidth**: 512 Mbit/s (64 channels × 500 kHz × 16-bit)
- **Code**: Verilog implementation with CIC decimation filters

#### Option 2: Microcontroller Array
- **Hardware**: 6-8 STM32H7 microcontrollers
- **Interface**: SAI peripheral for PDM reception
- **Synchronization**: Shared clock distribution critical

#### Option 3: Raspberry Pi CM4
- **Hardware**: Compute Module 4 with custom microphone HATs
- **Advantages**: Linux software stack, easier development

## Key Research Findings

### HyVL System (2023)
- First system to achieve **millimeter precision** (~3.4-4.8 mm)
- Approximately **3× better** than previous systems
- Approaches physical limits (mouse snout ~10 mm)
- Hybrid approach combines acoustic camera + high-quality microphones

### Cam64 Microphone Array
- **Manufacturer**: Sorama B.V. (sorama.eu)
- **Configuration**: 64 × Knowles SPH0641LU4H-1 MEMS microphones
- **Pattern**: Fermat's spiral arrangement for optimal spatial resolution
- **Sample rate**: 250 kHz (can be increased to 400+ kHz)

### Behavioral Insights
- Male mice produce **>96%** of ultrasonic vocalizations during courtship
- Females produce **<1%** but do vocally interact with males
- Males vocalize primarily during close proximity, especially ano-genital investigation

## Getting Started

### Hardware Requirements
- 64× Knowles SPH0641LU4H-1 MEMS microphones
- FPGA development board (Artix-7 or Cyclone V) OR
- 6-8× STM32H7 microcontrollers with SAI support
- Clock generation circuit (4 MHz synchronized distribution)
- USB 3.0 or Ethernet for data transfer

### Software Requirements
- FPGA: Xilinx Vivado or Intel Quartus
- MCU: STM32CubeIDE with HAL library
- DSP: PDM-to-PCM decimation filters (CIC filters)
- Analysis: Python with NumPy, SciPy for beamforming algorithms

## Related Papers

Additional papers on mouse vocalization analysis (see `Research_Papers_Summary.md`):
- **VocalMat** (eLife, 2021) - CNN-based USV classification with ~86% accuracy
- **Deep Learning Analysis** (Nature Scientific Reports, 2023) - Auto-Encoder, U-NET, RNN approaches

## License

This repository contains research papers and datasheets that are subject to their respective licenses and copyrights. Please refer to the original sources for usage rights.

## Author

**Lior Segev**
Weizmann Institute of Science
lior.segev@weizmann.ac.il

---

**Last Updated**: 2026-01-13
**Research compiled with assistance from**: Claude Sonnet 4.5
