# 6g-channel-estimation-dataset

## 🔍 **Overview of the Dataset**

The dataset is generated using MATLAB's **5G Toolbox**, specifically the reference example titled:

> **"Deep Learning Data Synthesis for 5G Channel Estimation"**

This example demonstrates how to **synthesize training data** for a **Convolutional Neural Network (CNN)**-based model to perform **channel estimation** in a **5G communication system**.

---

## 📡 **Channel Estimation Context**

### Purpose:

In wireless communication (5G in this case), **channel estimation** is essential for determining how a transmitted signal is affected by the channel, so the receiver can correctly interpret the received signal. You’re using **Deep Learning (CNN)** to estimate these channel characteristics.

---

## 🧠 **Neural Network Input Format**

1. **Signal Type**:

   * **SISO** (Single-Input, Single-Output) system: only one transmit and one receive antenna.
   * Uses the **PDSCH** (Physical Downlink Shared Channel) and **DM-RS** (Demodulation Reference Signal) for training.

2. **Dataset Size**:

   * **256 training samples** (i.e., signal is transmitted and received 256 times).
   * Each sample is a **resource grid** representing a time-frequency signal layout with:

     * **612 subcarriers**
     * **14 OFDM symbols**
     * **1 antenna**

3. **Original Data Format**:

   * Each sample is a **complex matrix** of shape: `612 x 14` (real + imaginary parts).

4. **Converted Input Format for CNN**:

   * The complex matrix is **split into two real-valued matrices**:

     * One for **real** part
     * One for **imaginary** part
   * New shape becomes: `612 x 14 x 2` (height x width x channels)

     * This is similar to an **image with 2 channels** (instead of RGB, it's real and imaginary).

5. **Training Input Tensor**:

   * For 256 samples, final shape becomes:

     * **`612 x 14 x 1 x 512`** or **`612 x 14 x 2 x 256`**
     * Often reshaped into **4D arrays** to fit CNN input:

       * `[Height x Width x Channels x Number of Samples]`
       * That is: **`612 x 14 x 2 x 256`**

---

## ⚙️ **Channel Diversity in the Dataset**

To ensure the CNN generalizes well, each dataset is generated under **varying channel conditions**:

| **Parameter**                   | **Variation / Range**             |
| ------------------------------- | --------------------------------- |
| **Delay Profiles**              | TDL-A, TDL-B, TDL-C, TDL-D, TDL-E |
| **Delay Spreads**               | 1 to 300 nanoseconds              |
| **Doppler Shifts**              | 5 to 400 Hz                       |
| **SNR (Signal-to-Noise Ratio)** | 0 to 10 dB                        |

* Each training sample simulates a unique **channel realization**, providing a rich training set.

---

## 🏷️ **Training Labels**

* Each input sample (resource grid with DM-RS symbols) has a corresponding **label**:

  * **Perfect channel values**, which are the ground-truth values the CNN is trying to learn to estimate.

---

## ⚙️ **Parameter Tuning Capabilities (5G Toolbox)**

The MATLAB 5G Toolbox provides high flexibility. You can configure various communication parameters, such as:

| **Parameter**         | **Examples / Options**               |
| --------------------- | ------------------------------------ |
| Carrier Frequency     | User-defined                         |
| Subcarrier Spacing    | 15, 30, 60, etc. kHz                 |
| Number of Subcarriers | 612 (in your case)                   |
| Cyclic Prefix Type    | Normal / Extended                    |
| Number of Antennas    | 1 (SISO), or extend to MIMO          |
| Channel Paths         | Multipath fading models (TDL models) |
| Bandwidth             | Configurable                         |
| Modulation            | QPSK, 16QAM, 64QAM, etc.             |
| Code Rate             | Adjustable via LDPC coding           |

---

## 🧩 **Why Convert to Real-Valued Input?**

Neural networks like CNNs are designed to process **real-valued data**, similar to how they process images (pixels are real numbers). Since your signal is complex (has real and imaginary parts), you:

1. **Split complex matrix** into two real-valued matrices.
2. Feed them into CNN as **2-channel input**.

This technique is common in signal processing applications involving DL.

---

## 🧠 **Training Summary**

* **Inputs**: Real-valued 4D arrays from complex-valued resource grids (shape: `612 x 14 x 2 x 256`)
* **Labels**: Perfect channel matrices
* **Model**: CNN
* **Goal**: Estimate the radio channel effects based on the DM-RS and PDSCH signals
* **Environment**: MATLAB 5G Toolbox

- [Dataset](https://github.com/EsraaMokhtar/6g-channel-estimation-dataset/blob/main/data.mat)
- [Defensive Distillation Implementation](https://github.com/EsraaMokhtar/6g-channel-estimation-dataset/blob/main/util_defdistill.py)
