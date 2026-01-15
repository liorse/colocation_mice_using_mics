# Mountable Miniature Microphones: Communication Method - EXACT DETAILS

## Paper: "Mountable Miniature Microphones to Identify and Assign Mouse Ultrasonic Vocalizations"
**Authors:** Elena N. Waidmann et al.  
**Published:** Cell Reports Methods, 2025; 5(6):101081

---

## Answer: How They Establish Communication

### **WIRED CONNECTION** (Confirmed)

The microphones use a **wired connection** system, not wireless. Here are the exact details:

---

## Communication Architecture

### 1. **Microphone Component**
- **Model:** Knowles SPU0410LR5H-QB MEMS microphone
- **Size:** 2.75 mm (length) × 1.85 mm (width) × 1.00 mm (height)
- **Weight:** 0.03 g (microphone only)
- **Total weight with PCB and mount:** 1.17 g

### 2. **Circuit Board Assembly**
- **Custom Printed Circuit Board (PCB):** Flexible polyimide flex PCB (1 layer)
- **Components on PCB:**
  - MEMS microphone (Knowles SPU0410LR5H-QB)
  - **JST connector** (for signal output)
  - 0.1μF capacitor
- **Assembly:** Components soldered using reflow oven process

### 3. **Connection Chain**

```
MEMS Microphone (on mouse head)
    ↓
Custom PCB with JST connector
    ↓
Micro-JST connector cable
    ↓
5-pin XLR to screw terminal adapter (Sescom)
    ↓
Ultrasound Gate 416H (Avisoft Bioacoustics)
    ↓
Avisoft-RECORDER software (Windows PC)
```

### 4. **Exact Connection Details**

**From the paper (Methods section):**

> "The audio signal was transferred through a **micro-JST connector** to an **Ultrasound Gate 416H (Avisoft) recording system**"

> "Mini-microphones were connected to the USGH via a **5-pin XLR to screw terminal adapter (Sescom)**"

### 5. **Recording System**

- **Interface:** Ultrasound Gate 416H (USGH) recording interface
  - Part #: 34163, 34164 (Avisoft Bioacoustics)
  - 4-channel recorder
- **Software:** Avisoft-RECORDER (Windows PC)
- **Recording Format:** WAV files at 250 kHz sampling rate

---

## Cable Management

### **Problem:** Cable Tangling
- Two mice with cables can tangle during free movement
- Tangling impedes natural behavior

### **Solution:** Manual Cable Rotator
- **3D-printed piece** with separate channels for each cable
- Both mini-microphone cables pass through separate channels
- **Manual rotation** transfers tangling to other parts of the wire
- Works for pairs of 2 mice even over an hour of recording

**From paper:**
> "We used a manual cable rotator to avoid tangling between the mini-microphone cables of the two freely moving animals"

> "If cables began to tangle, we manually rotated a small 3D printed piece through which both mini-microphone cables pass in separate channels"

---

## Signal Path Details

### **Physical Connection:**
1. **MEMS microphone** captures ultrasonic sound (30-110 kHz)
2. **Electrical signal** output from microphone
3. **PCB** routes signal to JST connector
4. **Micro-JST cable** carries signal from mouse to recording system
5. **XLR adapter** converts JST to XLR format
6. **Ultrasound Gate 416H** receives signal via XLR input
7. **Avisoft-RECORDER** software digitizes and records

### **Signal Characteristics:**
- **Sampling Rate:** 250 kHz (for ultrasonic range)
- **Format:** WAV files
- **Channels:** 3 channels recorded simultaneously:
  - Male mini-microphone
  - Female mini-microphone  
  - Overhead microphone (ground truth)

---

## Power Supply

**Not explicitly stated in paper**, but based on MEMS microphone technology:
- MEMS microphones typically require **3.3V power**
- Power likely supplied **via the cable** (not battery on mouse)
- JST connector likely includes power and ground wires
- Power provided by Ultrasound Gate 416H interface

---

## Advantages of Wired System

1. **Low Cost:** $0.60 per microphone (no wireless transmitters)
2. **Lightweight:** Total weight only 1.17 g
3. **High Signal Quality:** No wireless interference
4. **Simple:** Direct connection, no complex protocols
5. **Reliable:** No battery management needed

---

## Limitations Mentioned

**From Discussion section:**

> "This method is best suited for pairs of vocalizing animals and involves **wired microphones**. For larger groups, absent a more complex commutator system to avoid wire tangling, overhead array-based methods are more suitable."

> "Similarly, if the USV behavior being studied requires completely wire-free behavior, an array-based method or an additional data logger would be necessary."

**Future Improvements Suggested:**

> "A non-wired or non-surgical harness would also allow more naturalistic conditions for the vocal and social behavior and would permit even longer recording sessions without complications from cable tangling."

> "However, future versions of our system could involve a wireless setup with appropriate high-speed data streaming or data logging."

---

## Comparison with Other Methods

### **Mountable Microphones (This Paper):**
- **Connection:** Wired (micro-JST → XLR → USGH)
- **Cost:** ~$0.60/mic + recording system
- **Weight:** 1.17 g total
- **Cable Management:** Manual rotator for pairs

### **Microphone Arrays (HyVL, SLIM):**
- **Connection:** Wired (fixed array, no cables on animals)
- **Cost:** $200-$10,000+
- **Weight:** N/A (not on animal)
- **Cable Management:** N/A (fixed installation)

---

## Technical Specifications Summary

| Component | Specification |
|-----------|-------------|
| **Microphone** | Knowles SPU0410LR5H-QB MEMS |
| **Connector Type** | Micro-JST |
| **Adapter** | 5-pin XLR to screw terminal (Sescom) |
| **Recording Interface** | Ultrasound Gate 416H (Avisoft) |
| **Recording Software** | Avisoft-RECORDER |
| **Sampling Rate** | 250 kHz |
| **Cable Management** | Manual rotator (3D-printed) |
| **Total Weight** | 1.17 g (mic + PCB + mount) |
| **Connection Type** | **WIRED** |

---

## Conclusion

**Answer:** The microphones establish communication through a **wired connection**:

1. **Micro-JST connector** on the PCB
2. **Cable** from mouse to recording system
3. **5-pin XLR adapter** (Sescom) converts to XLR format
4. **Ultrasound Gate 416H** recording interface receives the signal
5. **Avisoft-RECORDER** software records the audio

**No wireless components** - this keeps the cost low ($0.60/mic) and the system simple, but requires cable management for free-moving animals.

---

## References

- **Paper:** Cell Reports Methods 5, 101081 (2025)
- **DOI:** 10.1016/j.crmeth.2025.101081
- **Methods Section:** Lines 933-1016 (STAR★METHODS)
- **Key Resources Table:** Lines 951-968
