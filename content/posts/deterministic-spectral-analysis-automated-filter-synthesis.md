---
title: "Deterministic Spectral Analysis and Automated Filter Synthesis for Engineering DSP Pipelines"
date: 2026-02-14
tags: ["spectral analysis", "PSD", "STFT", "tonal noise", "harmonic interference", "notch filter", "IIR", "FIR", "DSP verification", "SignalForge"]
description: "A deterministic, engineering-grade workflow for PSD/STFT-based tonal & harmonic detection, stable IIR/FIR synthesis, and quantitative verification—exporting deployable coefficients and auditable reports."
---

## Introduction

In real-world DSP systems—embedded sensing, instrumentation, audio processing, vibration monitoring, and RF-adjacent pipelines—engineers routinely face **narrowband tonal interference**, **harmonic spurs**, and **frequency-drifting noise components** contaminating time-domain measurements.

Typical workflows rely on manual spectrum inspection and heuristic tuning: visually identifying peaks, guessing problematic frequencies, and iteratively adjusting filters until the output “looks cleaner.” While workable for simple stationary tones, this approach becomes unreliable when interference drifts over time, appears intermittently, or overlaps with broadband noise.

This article presents a **deterministic and auditable workflow** for spectral characterization and automated filter synthesis—focused on engineering repeatability rather than black-box optimization—and explains how SignalForge structures that pipeline end-to-end.

---

## Why DSP Filtering Fails in Practice

Most real-world filtering failures are not caused by filter structures themselves, but by incorrect spectral identification and non-repeatable tuning.

Common failure modes include:

- false tonal detection caused by PSD ripple noise  
- missed interference due to frequency drift  
- unstable high-Q notch placement  
- excessive signal distortion  
- lack of quantitative verification  

A robust DSP pipeline must begin with correct spectral characterization, proceed to stable synthesis, and finish with objective performance validation.

---

## PSD and STFT: Complementary Analysis Tools

### Power Spectral Density (PSD)

PSD reliably reveals stationary narrowband energy and remains the primary tool for detecting tonal interference.

However, PSD alone struggles when tones drift, appear intermittently, or overlap broadband noise.

### Short-Time Fourier Transform (STFT)

STFT introduces temporal resolution, allowing:

- frequency drift tracking  
- burst isolation  
- persistent vs transient differentiation  

Engineering-grade workflows combine PSD stability with STFT dynamics.

---

## Deterministic Tonal and Harmonic Characterization

Rather than subjective tuning, deterministic characterization explicitly extracts:

- tonal frequencies  
- prominence strength  
- temporal persistence  
- harmonic relationships  

This transforms spectral inspection into structured engineering data.

---

## Example: One Real Signal → Analysis, Design, and Deployable Output

Below is a real SignalForge processing run showing the full deterministic pipeline—from detection to verification to firmware-ready output.

### 1) Tonal detection and stability diagnostics

![Detected tonals and STFT noise-floor diagnostics produced by SignalForge](/cases/example-b/tonal_debug.png)

*Figure 1 — PSD/STFT-based tonal peak detection with stability diagnostics preventing false notches.*

### 2) Quantitative before/after verification

![Before and after PSD comparison showing tonal suppression after filtering](/cases/example-b/before_after_psd.png)

*Figure 2 — Objective PSD verification demonstrating tonal suppression without broadband distortion.*

### 3) Deployable C coefficient output

- **Download filter coefficients (C header):** [/cases/example-b/filter_coeffs.h](/cases/example-b/filter_coeffs.h)

Excerpt:

```c
#define FILTER_FS_HZ              (48000.00000000f)
#define FILTER_IIR_NUM_SECTIONS   (1u)
#define FILTER_FIR_NUM_TAPS       (101u)

static const float g_filter_iir_sos[FILTER_IIR_NUM_SECTIONS][6] =
{
    { 0.99869781f, -1.99128631f, 0.99869781f, 1.00000000f, -1.99128631f, 0.99739563f }
};
