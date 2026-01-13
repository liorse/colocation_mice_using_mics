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

### System Architecture: Microphone-to-FPGA Interface

```
┌─────────────────────────────────────────────────────────────┐
│  Digilent Nexys A7-100T FPGA Board                          │
│                                                              │
│  ┌─────────────────┐         ┌──────────────────────┐      │
│  │ Clock Generator │ 4 MHz   │  64× PDM Receivers   │      │
│  │  (PLL Divider)  ├────────►│  (CIC Decimation)    │      │
│  │  100MHz → 4MHz  │         │                      │      │
│  └─────────────────┘         └──────────┬───────────┘      │
│                                         │ 16-bit PCM        │
│  ┌──────────────────┐         500 kHz sample rate         │
│  │ Pmod Ports (32)  │◄─────┐                              │
│  │ + GPIO Headers   │      │  ┌──────────────────────┐   │
│  └───────┬──────────┘      └──┤ Beamforming Engine   │   │
│          │                     │ (TDOA Calculation)   │   │
│          │ 32 DATA lines       └──────────┬───────────┘   │
│          │                                 │               │
│          │                     ┌───────────▼──────────┐   │
│          │                     │  USB 3.0 / Ethernet  │   │
│          │                     │  (to Host PC)        │   │
└──────────┼─────────────────────┴──────────────────────────┘
           │
           │
    ┌──────▼──────────────────────────────────────┐
    │  Custom Microphone Array PCB                │
    │  (Fermat's Spiral Pattern)                  │
    │                                              │
    │  64× Knowles SPH0641LU4H-1                 │
    │                                              │
    │  Connections per mic:                       │
    │  • VDD (3.3V) ───────────┐                 │
    │  • GND ──────────────────┤                 │
    │  • CLOCK (4MHz) ◄────────┤ Shared buses    │
    │  • DATA (PDM) ───────────┤ Individual lines│
    │  • SELECT (L/R) ─────────┘ Config per pair │
    │                                              │
    └──────────────────────────────────────────────┘
```

**Interface Details:**
- **32 stereo pairs**: Each pair shares one DATA line, differentiated by SELECT pin (L=GND, R=VDD)
- **Clock distribution**: Single 4 MHz clock fanned out to all 64 microphones with <1mm trace length matching
- **PDM data**: 32 individual DATA lines from FPGA GPIO to microphone pairs
- **Decimation**: FPGA converts 4 MHz PDM to 500 kHz 16-bit PCM in real-time
- **Throughput**: 64 channels × 500 kHz × 16 bits = 512 Mbit/s to host PC

### Sampling Requirements

For 80 kHz ultrasonic signals:
- **Nyquist minimum**: >160 kHz sampling rate
- **Recommended**: 400-500 kHz for 5× oversampling
- **Clock frequency**: 3.2-4.8 MHz (ultrasonic mode)
- **Decimation factor**: 8-12 (PDM to PCM conversion)

### Implementation Options

#### Option 1: FPGA (Recommended for 64-Microphone Array)

**Recommended Board: Digilent Nexys A7-100T**
- **FPGA Chip**: Xilinx XC7A100T-1CSG324C
- **Logic Cells**: 101,440
- **Block RAM**: 4,860 Kbit
- **DSP Slices**: 240 (perfect for CIC decimation filters)
- **I/O**: 100+ user I/O through 4× Pmod connectors + expansion headers
- **Clock**: 100 MHz oscillator (generates 4 MHz PDM clock)
- **Memory**: 128 MB DDR2 RAM for audio buffering
- **Price**: ~$409 USD ([Digilent Store](https://digilent.com/shop/nexys-a7-amd-artix-7-fpga-trainer-board-recommended-for-ece-curriculum/) | [DigiKey](https://www.digikey.com/en/products/detail/digilent-inc/410-292/5117190) | [Amazon](https://www.amazon.com/Digilent-Nexys-DDR-Artix-7-FPGA/dp/B0714MKJ4H))

**Why Nexys A7-100T?**
- **Direct connections**: 32 data pins via Pmod ports can handle 64 mics (L/R stereo pairs)
- **No I/O expanders needed**: Simpler hardware design
- **Parallel processing**: All 64 channels processed simultaneously
- **Real-time beamforming**: Calculate TDOA for 2,016 microphone pairs in hardware
- **Data bandwidth**: 512 Mbit/s (64 channels × 500 kHz × 16-bit PCM)

**Alternative Budget Option: Digilent Arty A7-100T**
- Same FPGA chip but fewer accessible I/O pins (~$299)
- Requires external I/O multiplexers for 64 microphones
- [Purchase Link](https://digilent.com/shop/arty-a7-100t-artix-7-fpga-development-board/)

#### Option 2: Microcontroller Array
- **Hardware**: 6-8 STM32H7 microcontrollers
- **Interface**: SAI peripheral for PDM reception
- **Synchronization**: Shared clock distribution critical
- **Cost**: ~$300-400 total but more complex firmware

#### Option 3: Raspberry Pi CM4
- **Hardware**: Compute Module 4 with custom microphone HATs
- **Advantages**: Linux software stack, easier development
- **Limitations**: Software-based processing, higher latency

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

#### Microphones (Exact Specification from Datasheet)
**64× Knowles SPH0641LU4H-1 MEMS Microphones**
- **Part Number**: SPH0641LU4H-1 (as specified in `mic_SPH0641LU4H-1.pdf`)
- **Type**: Digital MEMS microphone with PDM output
- **Output Format**: PDM (Pulse Density Modulation), single-bit digital stream
- **Operating Voltage**: 1.62-3.6V (typical 3.3V)
- **SNR**: 64.3 dB(A) - suitable for ultrasonic recording
- **Frequency Response**: Optimized for ultrasonic (3.072-4.8 MHz clock)
- **Sensitivity**: -26 dBFS
- **Package**: Surface mount, top-port design
- **Pinout**: 5 pins (VDD, GND, CLOCK, DATA, SELECT for L/R channel)
- **Cost**: ~$2-3 per unit in bulk

**Critical Connection Requirements:**
- All 64 microphones must share **identical 4 MHz clock signal**
- Clock jitter must be <250 ns for phase-coherent recording
- Separate DATA line for each microphone (or 32 stereo pairs using SELECT pin)

#### FPGA Development Board
**Digilent Nexys A7-100T** (Recommended)
- FPGA: Xilinx XC7A100T-1CSG324C Artix-7
- Handles 64 PDM microphones with CIC decimation filters
- Direct microphone connections via Pmod expansion ports
- Purchase: ~$409 ([Buy here](https://digilent.com/shop/nexys-a7-amd-artix-7-fpga-trainer-board-recommended-for-ece-curriculum/))

#### Supporting Hardware
- **Clock Generation**: Si5351 programmable clock generator or FPGA PLL (for 4 MHz distribution)
- **Power Supply**: Low-noise 3.3V regulator for microphones (e.g., TPS7A4700)
- **PCB**: Custom microphone array board with Fermat's spiral pattern
- **Data Interface**: USB 3.0 (512 Mbit/s bandwidth) or Gigabit Ethernet

### Software Requirements
- **FPGA Tools**: Xilinx Vivado Design Suite (free WebPACK edition supports Artix-7)
- **HDL**: Verilog implementation with CIC decimation filters (example code available)
- **DSP Processing**: PDM-to-PCM conversion with 8-12× decimation
- **Beamforming**: Python with NumPy, SciPy for TDOA calculation and localization
- **Visualization**: MATLAB or Python for real-time position tracking

### Bill of Materials (Estimated)

| Component | Quantity | Unit Price | Total |
|-----------|----------|------------|-------|
| Knowles SPH0641LU4H-1 | 64 | $2.50 | $160 |
| Nexys A7-100T FPGA | 1 | $409 | $409 |
| Custom PCB (array board) | 1 | $50-100 | $75 |
| Clock generator + components | 1 | $20 | $20 |
| Power supplies, connectors | - | $30 | $30 |
| **Total Estimated Cost** | | | **~$694** |

*For comparison: Commercial Cam64 system costs ~$10,000+*

## Open Source Projects & Reference Implementations

### FPGA PDM Microphone Array Projects

#### Recommended Starting Points

1. **Matrix Creator - 8 PDM Microphone Array** ⭐ Most Complete Example
   - **GitHub:** [Hoi-Jeon/Verilog-for-Mic-in-Matrix-Creator](https://github.com/Hoi-Jeon/Verilog-for-Mic-in-Matrix-Creator)
   - **Hardware:** Xilinx Spartan-6 FPGA + 8× PDM microphones
   - **Language:** Verilog
   - **Key modules:** `wb_mic_array.v` (PDM reception), `bram.v` (buffering)
   - **Use for:** Understanding multi-channel PDM interfacing

2. **LiteX PDM2PCM - Scalable CIC Filter**
   - **GitHub:** [kamejoko80/litex-pdm2pcm](https://github.com/kamejoko80/litex-pdm2pcm)
   - **Features:** CIC decimation filter with flexible parameters
   - **Scalability:** Easily replicable for microphone arrays
   - **Use for:** Production-ready PDM-to-PCM conversion

3. **Simple PDM Interface**
   - **GitHub:** [kazkojima/pdmmic-example](https://github.com/kazkojima/pdmmic-example)
   - **Language:** Amaranth HDL (generates Verilog)
   - **Based on:** [Tom Verbeure's PDM Tutorial](https://tomverbeure.github.io/2020/10/04/PDM-Microphones-and-Sigma-Delta-Conversion.html)
   - **Use for:** Clean reference implementation

4. **Audio Processing on Artix-7** ⭐ Same FPGA!
   - **GitHub:** [FallingLights/Audio-Processing-Artix-7](https://github.com/FallingLights/Audio-Processing-Artix-7)
   - **Hardware:** Artix-7 FPGA (Nexys A7 compatible)
   - **Language:** VHDL
   - **Use for:** Artix-7-specific implementations

### Hardware Design References

5. **Pmod PDM Microphone Array**
   - **GitHub:** [MarcinWachowiak/pmod-pdm-microphone-array](https://github.com/marcinwachowiak/pmod-pdm-microphone-array)
   - **Design:** Linear array with Pmod connector interface
   - **Spacing:** λ/2 at 3 kHz for beamforming
   - **Use for:** Custom PCB design inspiration

6. **PDM Signal Processing Tools**
   - **GitHub:** [siorpaes/pdm_playground](https://github.com/siorpaes/pdm_playground)
   - **Target:** Xilinx Basys3 (Artix-7)
   - **Features:** Signal generation, acquisition, and decoding
   - **Use for:** Testing and debugging PDM interfaces

### Beamforming & Sound Localization

7. **Acoustic Array Tools - STM32H7**
   - **GitHub:** [mcbridejc/acoustic-array-tools](https://github.com/mcbridejc/acoustic-array-tools)
   - **Hardware:** STM32H7 microcontroller
   - **Features:** Complete beamforming implementation
   - **Use for:** Understanding beamforming algorithms

8. **Microphone Array Beamforming Toolbox**
   - **GitHub:** [MiguelBlancoGalindo/MicArrayBeamforming](https://github.com/MiguelBlancoGalindo/MicArrayBeamforming)
   - **Language:** MATLAB/Python
   - **Use for:** Post-processing and algorithm development

9. **MIT 6.111 Sound Source Localizer**
   - **Report:** [MIT Final Report (PDF)](http://web.mit.edu/6.111/www/f2016/projects/finalreport_changpingchen_jorenlauwers.pdf)
   - **Hardware:** Nexys 4 (Artix-7 FPGA)
   - **Use for:** Academic reference and educational approach

### Large-Scale Implementations

10. **192-Channel Phased Array Microphone** 🔥 Impressive!
    - **Blog:** [Ben Wang's Project](https://benwang.dev/2023/02/26/Phased-Array-Microphone.html)
    - **Hackaday:** [Project Page](https://hackaday.io/project/12363-phased-array-microphone-using-fpga)
    - **Scale:** 192 PDM microphones with FPGA + GPU beamforming
    - **Interface:** Gigabit Ethernet for raw PDM data
    - **Use for:** Inspiration for scaling beyond 64 microphones

### SPH0641LU4H-1 Specific Projects

11. **ESP32 SPH0641 Ultrasonic Recording**
    - **GitHub:** [kunsen-an/espidf_pdm_sph0641_mic_out](https://github.com/kunsen-an/espidf_pdm_sph0641_mic_out)
    - **Microphone:** SPH0641LU4H-1 (exact same model!)
    - **Sampling:** 48 kHz / 96 kHz, supports up to 80 kHz ultrasonic
    - **Use for:** Quick prototyping and microphone testing

### Additional Resources

12. **FPGA Beamforming Architectures Survey**
    - **Paper:** [MDPI Sensors Journal](https://www.mdpi.com/2073-431X/7/3/41)
    - **Title:** "FPGA-Based Architectures for Acoustic Beamforming with Microphone Arrays"
    - **Content:** Comprehensive review of trends, challenges, and opportunities

13. **PDM-to-PCM Conversion Guide**
    - **PDF:** [Signal Conversion on FPGA](https://yatian-liu.github.io/public/PDM_PCM_Signal_Conversion_FPGA.pdf)
    - **Topics:** CIC and FIR filter implementation details

14. **Tom Verbeure's PDM Tutorial** 🌟 Must Read!
    - **Blog:** [PDM Microphones and Sigma-Delta Conversion](https://tomverbeure.github.io/2020/10/04/PDM-Microphones-and-Sigma-Delta-Conversion.html)
    - **Content:** In-depth PDM principles and FPGA implementation

### GitHub Topic Pages

Browse more projects:
- [microphone-array](https://github.com/topics/microphone-array)
- [beamforming](https://github.com/topics/beamforming)
- [sound-localization](https://github.com/topics/sound-localization)
- [artix-7](https://github.com/topics/artix-7)

### Recommended Learning Path

For building your 64-microphone Artix-7 system:

1. **Start:** Study Matrix Creator 8-mic example for multi-channel PDM reception
2. **Filtering:** Implement LiteX PDM2PCM CIC filter and scale to 64 channels
3. **Hardware:** Design custom PCB inspired by Pmod microphone array
4. **Beamforming:** Implement TDOA algorithms from acoustic-array-tools
5. **Scale:** Learn from Ben Wang's 192-channel architecture for optimization
6. **Test:** Use ESP32 example to verify individual microphones work correctly

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
