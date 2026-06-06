# Real-Time UART Protocol Visualizer and Decoder — LabVIEW

Software-based UART communication simulator built in NI LabVIEW 2016. Converts text input to ASCII byte arrays, simulates byte-by-byte transmission with configurable timing, and visualizes data in real time on an oscilloscope-style waveform chart. Decodes received bytes back to readable text — demonstrating the complete UART encode → transmit → decode pipeline.

---

## Overview

UART transmission occurs internally at high speed and cannot be directly observed without specialized hardware. This simulator makes the process visible — each character is converted to its ASCII value and transmitted as a discrete voltage-level step on a waveform chart, with configurable delay between bytes to simulate baud rate timing.

Built as a course-based project for the LabVIEW Programming Laboratory (22PC2EI311), VNRVJIET.

---

## Demo

| Input | ASCII Values | Waveform | Decoded Output |
|-------|-------------|----------|----------------|
| `a` | 97 | Single step at amplitude 97 | `a` |
| `hello` | 104, 101, 108, 108, 111 | 5 steps in ~100–111 range | `hello` |
| `~!~!` | 126, 33, 126, 33 | Alternating high-low steps | `~!~!` |

---

## Screenshots

### Front Panel — Single Character ("a")
![Single char transmission](docs/screenshot_single_char.png)

### Block Diagram — VI Logic
![Block diagram](docs/screenshot_block_diagram.png)

### Front Panel — Special Characters ("~!~!")
![Special chars](docs/screenshot_special_chars.png)

### Front Panel — Word ("hello")
![Word transmission](docs/screenshot_hello.png)

---

## System Architecture

```
User Input (String)
    │
    │  String To Byte Array
    ▼
ASCII Byte Array [72, 101, 108, 108, 111]
    │
    │  For Loop — iterates each byte
    ▼
To Double Precision Float
    │
    │  Wait (150ms) — simulates baud rate timing
    ▼
Waveform Chart (Raw BitStream)
    │  Oscilloscope-style strip chart
    │  Y-axis: 30–130 (printable ASCII range)
    │  Step-line plot, black background
    ▼
Byte Array To String
    │
    ▼
Decoded Output (String Display)
```

---

## Front Panel Controls

| Control | Type | Description |
|---------|------|-------------|
| Data To Send | String input | Text to transmit |
| Baud Rate | Numeric | 9600 (simulated) |
| Data Bits | Numeric | 8 |
| Stop Bits | Numeric | 1 |
| Parity | Enum | None / Even / Odd |
| Start Transmission | Boolean button | Begin simulation |
| Stop | Boolean button | Halt execution |

## Front Panel Indicators

| Indicator | Type | Description |
|-----------|------|-------------|
| Raw BitStream | Waveform Chart | Oscilloscope-style ASCII byte visualization |
| Decoded Output | String display | Reconstructed text from byte array |
| Parity Error | Numeric | Error counter (0 = no errors) |
| Framing Error | Numeric | Frame error counter |
| Status | Boolean LED | Transmission active indicator |

---

## LabVIEW VIs Used

| VI | Purpose |
|----|---------|
| String To Byte Array | Convert input text to ASCII byte values |
| For Loop | Iterate through each byte sequentially |
| To Double Precision Float | Convert bytes to chart-compatible format |
| Wait (ms) | Simulate baud rate timing between bytes |
| Local Variable | Update waveform chart inside loop |
| Byte Array To String | Decode received bytes back to text |
| Waveform Chart | Real-time oscilloscope-style display |

---

## Waveform Chart Configuration

- **Y-axis range:** 30–130 (covers full printable ASCII range 32–126)
- **Update mode:** Strip chart (scrolling)
- **Plot style:** Step-line
- **Background:** Black (oscilloscope appearance)
- **X-axis:** Time (samples)

---

## Test Results

| Test Case | Input | Expected ASCII | Chart Steps | Decoded Output | Pass |
|-----------|-------|---------------|-------------|----------------|------|
| 1 | `a` | 97 | 1 step @ ~97 | `a` | ✅ |
| 2 | `hello` | 104,101,108,108,111 | 5 steps | `hello` | ✅ |
| 3 | `~!~!` | 126,33,126,33 | 4 alternating steps | `~!~!` | ✅ |
| 4 | `Hi` | 72, 105 | 2 steps | `Hi` | ✅ |
| 5 | `!!` | 33, 33 | 2 equal steps | `!!` | ✅ |

Zero decoding errors across all test cases.

---

## Software Requirements

| Software | Version |
|----------|---------|
| NI LabVIEW | 2016 Robotics Edition |

No additional toolkits or hardware required.

---

## Limitations

- Byte-level visualization only — individual start/stop/parity bits not shown
- Timing uses 150ms/byte for visibility — real 9600 baud is ~1ms/byte
- Unidirectional TX simulation only — no RX channel
- Software simulation only — no hardware serial port interfacing

---

## Applications

- Educational tool for understanding UART at byte level
- Pre-hardware debugging aid for STM32, Arduino, ESP32 UART development
- Visual demonstration of ASCII encoding and serial protocol fundamentals
- Reference for MODBUS RTU, GPS NMEA, Bluetooth HC-05 communication patterns

---

## Author

**Abhiram Kurella** (Kurella Satya Sai Abhiram — 23071A1095)
**Kankipati Sree Mahit** (23071A1089)
B.Tech Electronics and Instrumentation Engineering
VNR Vignana Jyothi Institute of Engineering & Technology (2023–2027)
Guided by: Dr. S S Bharati Sagi, Assistant Professor, Dept. of EIE
abhiram.kurella@gmail.com
