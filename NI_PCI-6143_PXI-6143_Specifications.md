# National Instruments PCI-6143 / PXI-6143 Specifications

## Overview

These are the data acquisition devices used in the VCL Benchmark research for recording ultrasonic vocalizations from microphone arrays.

**Source:** Supplemental Information PDF (NIHPP2024.09.20.613758V1-supplement-1.pdf)

---

## PCI-6143 Specifications

**Model:** PCI-6143  
**Type:** PCI Multifunction I/O Device  
**Description:** 8 AI (16-Bit, 250 kS/s/ch), 8 DIO

### Key Specifications:
- **Analog Input Channels:** 8 channels
- **Resolution:** 16-bit
- **Maximum Sampling Rate:** 250 kS/s per channel
- **Sampling Type:** Simultaneous sampling (all channels sampled at the same instant)
- **Digital I/O:** 8 channels
- **Counters:** Two 24-bit counters
- **Triggering:** Digital triggering support
- **Interface:** PCI bus (desktop computer)
- **Connector:** BNC-2110 terminal block (BNC connections)
- **Applications:** High-energy physics, ultrasonic/sonar testing, ballistics, transient signals, multiaxis control

### Usage in VCL Research:
- **Environment:** E1 datasets
- **Configuration:** 4 ultrasonic microphones (Avisoft CM16/CMPA48AAF-5V)
- **Sampling Rate Used:** 125 kHz (half of maximum capability)
- **Connection:** Via BNC-2110 terminal block
- **Control:** NI-DAQmx Python library
- **Synchronization:** Synchronous acquisition with video (FLIR USB Backfly S camera)

---

## PXI-6143 Specifications

**Model:** PXI-6143  
**Type:** PXI Multifunction I/O Module  
**Description:** 8 AI (16-Bit, 250 kS/s/ch), 8 DIO

### Key Specifications:
- **Analog Input Channels:** 8 channels
- **Resolution:** 16-bit
- **Maximum Sampling Rate:** 250 kS/s per channel
- **Sampling Type:** Simultaneous sampling (all channels sampled at the same instant)
- **Digital I/O:** 8 channels
- **Counters:** Two 24-bit counters
- **Triggering:** Digital triggering support
- **Interface:** PXI bus (modular instrumentation platform)
- **Chassis:** PXIe-1071 (mentioned in research)

### Usage in VCL Research:
- **Environment:** E2 datasets
- **Configuration:** 8-channel audio recording
- **Sampling Rate Used:** 125 kHz
- **Chassis:** PXIe-1071 chassis
- **Synchronization:** Camera synchronization device (e3 Vision Hub + Camera, White Matter LLC) at 30 Hz

---

## Comparison: PCI-6143 vs PXI-6143

| Feature | PCI-6143 | PXI-6143 |
|---------|----------|----------|
| **Form Factor** | PCI card (desktop) | PXI module (rack/chassis) |
| **Channels** | 8 AI | 8 AI |
| **Resolution** | 16-bit | 16-bit |
| **Max Sample Rate** | 250 kS/s/ch | 250 kS/s/ch |
| **Digital I/O** | 8 DIO | 8 DIO |
| **Used in VCL** | E1 (4 mics @ 125 kHz) | E2 (8 mics @ 125 kHz) |
| **Connector** | BNC-2110 terminal block | PXI backplane |

---

## Technical Details from VCL Research

### E1 Environment Setup (PCI-6143):
```
Hardware Chain:
Microphones (Avisoft CM16/CMPA48AAF-5V)
  ↓
Avisoft Preamplifier
  ↓
BNC-2110 Terminal Block
  ↓
PCI-6143 (via BNC connection)
  ↓
Desktop PC (PCI bus)
```

**Software:**
- NI-DAQmx Python library (`nidaqmx-python`)
- Samples written to disk at 125 kHz
- Synchronized with video via external triggers

### E2 Environment Setup (PXI-6143):
```
Hardware Chain:
8 Microphones
  ↓
PXI-6143 IO Module
  ↓
PXIe-1071 Chassis
  ↓
Host Controller
```

**Differences from E1:**
- Uses camera synchronization device instead of external triggers
- 8-channel audio (vs 4-channel in E1)
- Modular PXI platform for scalability

---

## Why These Devices Were Chosen

1. **High Sampling Rate:** 250 kS/s per channel supports ultrasonic frequencies (30-110 kHz for mice)
   - Nyquist requirement: >220 kHz for 110 kHz signals
   - 125 kHz used in research provides ~2.3× oversampling

2. **Simultaneous Sampling:** All channels sampled at the same instant (not multiplexed)
   - Critical for beamforming and TDOA calculations
   - Eliminates phase errors between channels
   - PCI/PXI bus provides deterministic timing

3. **16-bit Resolution:** Adequate dynamic range for ultrasonic vocalizations
   - SNR of microphones (~64 dB) matches well with 16-bit ADC

4. **Reliable Hardware:** National Instruments provides robust, well-supported hardware
   - Extensive Python API (NI-DAQmx)
   - Proven in scientific applications

---

## Alternative Considerations

### For Your 64-Microphone Array Project:

**PCI-6143 Limitations:**
- Only 8 channels per card
- Would need 8× PCI-6143 cards = 64 channels
- PCI bus bandwidth limitations
- Desktop PC form factor

**PXI-6143 Limitations:**
- Only 8 channels per module
- Would need 8× PXI-6143 modules = 64 channels
- Requires PXI chassis (PXIe-1071 or similar)
- More expensive modular platform

**Why FPGA Approach is Better:**
- Single FPGA handles all 64 channels simultaneously
- Lower latency (hardware processing)
- Lower cost (~$694 vs ~$8,000+ for 8× NI cards)
- More flexible (can implement custom processing)

---

## References

- **NI PCI-6143:** https://www.ni.com/en-us/support/model.pci-6143.html
- **NI PXI-6143:** https://www.ni.com/en-us/support/model.pxi-6143.html
- **NI-DAQmx Python:** https://github.com/ni/nidaqmx-python
- **VCL Benchmark Supplemental:** NIHPP2024.09.20.613758V1-supplement-1.pdf

---

**Note:** These specifications are based on information from the VCL Benchmark supplemental document and NI website metadata. For complete technical specifications, refer to the official NI datasheets.
