# Benchmark Results (PHOTEX vs NVIDIA H100)

PHOTEX simulation outputs compared against NVIDIA H100 published specifications, using Llama-3 8B as the throughput reference model. PHOTEX columns derive from stated geometry and simulation parameters. H100 columns reference published datasheets. All PHOTEX values are architecture projections. No physical hardware has been measured.

**Simulation snapshot:** February 2025
**Throughput reference model:** Llama-3 8B

---

## 1. Simulation Parameters

| Parameter | Value |
|-----------|-------|
| Wavelength | 405 nm |
| Pixel size | 1 µm |
| Layer spacing | 10 µm |
| Angular increment | 0.1° |
| Angular range | 90° (π/2) |
| Refractive index | 1.5 |
| Speed of light | 2.99792458×10⁸ m/s |

---

## 2. Parameter Capacity

### 2.1 Volumetric Multiplexing

Parameter capacity is derived from the physical geometry of a 1 cm³ holographic crystal under the stated simulation parameters:

| Item | Computation |
|:--|:--|
| Z-layers | 0.01 m / 10 µm = **1,000** |
| Angular lanes | 90° / 0.1° = **900** |
| Effective multiplex depth | **900,000** combined slices |
| Pixels per slice | `(1 cm / 1 µm)² = 10⁸` (**100 million**) |
| Raw parameters per cm³ | **90 trillion** |

This figure assumes manufacturing tolerances hold at the simulation resolution. Practical yield will be lower.

### 2.2 With Error Correction

Allocating half the raw capacity to redundancy yields a conservative usable estimate of **45 T parameters per cm³**. Neither figure has been validated on physical hardware.

### 2.3 Comparison with H100

H100 parameter capacity is derived from HBM capacity at FP16 precision.

| System | Parameter capacity |
|--------|--------------------|
| NVIDIA H100 SXM (80 GB HBM) | ~20B |
| NVIDIA H100 SXM (141 GB HBM) | ~35B |
| PHOTEX hypothetical 1 cm³ crystal | **90 T** raw · **45 T** usable (projected) |

The projected density advantage is approximately **700× to 4,500× per cm³**. The H100 package including HBM spans hundreds of cubic centimeters; a direct volume comparison narrows this gap.

---

## 3. Energy

### 3.1 Layer Comparison

| Metric | Electronic (linear layer) | Optical (diffractive layer) |
|--------|--------------------------|------------------------------|
| Operations | 1,048,576 | ~12,288 (simulation-derived) |
| Energy | ~104.86 pJ | ~0.01 pJ |
| Projected reduction | baseline | ~99.99% (simulated) |

### 3.2 Wave Propagation (256×256, 5 steps)

| Metric | Electronic | Optical |
|--------|------------|---------|
| Energy | ~1.5 pJ | ~0.01 pJ |
| Projected reduction | baseline | ~99% (simulated) |

### 3.3 Full Device Comparison

| System | Energy per forward pass |
|--------|-------------------------|
| NVIDIA H100 | order millijoules |
| PHOTEX (projected) | ~pJ–fJ regime (simulated) |

Projected energy reduction is approximately 99% per forward pass under these simulation assumptions.

---

## 4. Speed and Throughput

### 4.1 Token Throughput

| System | Forward pass window | Tokens per second |
|--------|---------------------|-------------------|
| NVIDIA H100 (top bin) | ~6.7 ms | 150 TPS |
| NVIDIA H100 (published range) | ~10 ms | 100 TPS |
| PHOTEX (time-of-flight model) | 1.334 ns (modeled) | **~750 million TPS** (projected) |

### 4.2 With Overhead

| Scenario | TPS |
|----------|-----|
| Raw optical time-of-flight | 750 M |
| After 50% overhead correction (projected) | **375 M** |
| Conservative estimate including averaging and digital post-processing | **7.5 M** |

These projections are highly speculative and are pending physical validation.

### 4.3 Scale Benchmark

Forward pass measured across a 1024×1024 grid through multiple diffractive layers, used as a proxy for a full forward graph.

| Platform | Forward pass | Throughput |
|----------|--------------|-----------------------|
| CPU/GPU simulation | ~443 ms | ~2.3 passes/sec |
| PHOTEX time-of-flight (modeled) | 1.334 ns | ~750 M passes/sec (projected) |

Projected speedup: ~340,000,000×. This compares software simulation time against theoretical optical time-of-flight. Both figures are reported so neither gets cited in isolation.

### 4.4 Time-of-Flight Latency

Latency derived from τ = L·n/c, with n = 1.5 and c = 3×10⁸ m/s.

| Path length | Latency estimate |
|:--|:--|
| 1 cm | ~50 ps |
| 2 cm | ~100 ps |
| 3 cm | ~150 ps |
| 4 cm | **~200 ps** |

The 1.334 ns figure used in throughput benchmarks corresponds to a longer modeled optical path than the 4 cm physical device. Both figures are reported to avoid selective citation.

---

## 5. Noise and Accuracy

### 5.1 Density and Error Correction

Half the raw 90 T capacity is allocated toward redundancy, yielding the 45 T usable estimate cited in Section 2.2.

### 5.2 Multi-Run Noise Averaging

Analog phase jitter is modeled as noise on each inference pass. Running N independent passes and averaging the output logits reduces noise standard deviation by 1/√N and variance by 1/N.

| Parameter | Value |
|-----------|-------|
| Averaging runs | 100 |
| Single-shot accuracy with noise (simulated) | ~85–95% |
| Post-averaging accuracy (simulated) | >99% |

---

## 6. Summary

| Metric | NVIDIA H100 | PHOTEX 1 cm³ (projected) |
|--------|-------------|--------------|
| Parameters | ~20–35B | **45–90 T** |
| Latency | ~7–10 ms | **~1.3 ns** |
| Energy per pass | ~mJ | **~pJ–fJ** |
| Throughput | 100–150 TPS | **7.5 M – 375 M TPS** |

| Advantage | Factor |
|-----------|--------|
| Parameter density | 700×–4,500× |
| Energy | ~99% reduction |
| Throughput | 50,000×–3.75M× |
| Latency | 5–7 million× lower |

All PHOTEX figures are projections derived from the simulation parameters in Section 1. They have not been validated against physical hardware.
