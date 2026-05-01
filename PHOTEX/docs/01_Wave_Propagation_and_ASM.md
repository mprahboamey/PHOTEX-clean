# Wave Propagation and Photonic Inference

This document covers the physical models underlying PHOTEX: wave optics, the Angular Spectrum Method, diffractive phase modulation, and interference-based attention.

---

## 1. Wave Optics Fundamentals

### 1.1 The Wave Equation

Light propagation follows the scalar wave equation:

```
∇²E - (1/c²)∂²E/∂t² = 0
```

For monochromatic light, this reduces to the Helmholtz equation:

```
(∇² + k²)E = 0
```

where `k = 2π/λ` is the wavenumber.

### 1.2 Complex Representation

A monochromatic field is represented as:

```
E(x, y, z, t) = A(x, y, z) × exp(j(ωt - k·r))
```

The complex amplitude is:

```
u(x, y, z) = A(x, y, z) × exp(-jφ(x, y, z))
```

where **A** is the magnitude envelope and φ is the phase in radians.

### 1.3 Intensity

Intensity is the squared modulus of the complex amplitude:

```
I(x, y, z) = |u(x, y, z)|² = A²(x, y, z)
```

---

## 2. Angular Spectrum Method (ASM)

The ASM propagates a field from one plane to another by decomposing it into plane wave components in the Fourier domain. Each component acquires a propagation phase factor independently, reducing the computation to an FFT, a pointwise multiply, and an inverse FFT — O(N log N) per propagation step.

### 2.1 Derivation

Starting from the Helmholtz equation along the z axis:

```
∂²u/∂z² = -(∂²u/∂x² + ∂²u/∂y² + k²u)
```

Taking the 2D Fourier transform of the transverse plane:

```
U(fx, fy, z) = ∫∫ u(x, y, z) exp(-j2π(fx·x + fy·y)) dx dy
```

This gives:

```
∂²U/∂z² = -kz²U
```

The solution is:

```
U(fx, fy, z) = U(fx, fy, 0) × H(fx, fy, z)
```

where the transfer function is:

```
H(fx, fy, z) = exp(j · kz · z)
```

and:

```
kz = sqrt(k² - (2πfx)² - (2πfy)²)
```

When kz is real, the component propagates. When kz is imaginary, the component is evanescent and decays exponentially with distance.

### 2.2 Evanescent Waves

For spatial frequencies satisfying `(2πfx)² + (2πfy)² > k²`:

```
kz = j × sqrt((2πfx)² + (2πfy)² - k²)
```

These components carry negligible energy over distances of more than a few wavelengths and are excluded from the simulation.

### 2.3 Algorithm Summary

1. Take the 2D FFT of the input field `u(x, y, 0)`
2. Multiply by the transfer function `H(fx, fy, z)`
3. Take the inverse FFT to obtain `u(x, y, z)`

**Complexity:** O(N log N) per propagation step.

### 2.4 PHOTEX Simulation Parameters

| Parameter | Value |
|-----------|-------|
| Wavelength | 405 nm |
| Pixel size | 1 µm |
| Layer spacing | 10 µm |
| Refractive index | 1.5 |

---

## 3. Phase Modulation: Diffractive Layers

Each diffractive layer applies a trainable phase mask to the traversing field:

```
u_out(x, y) = u_in(x, y) × exp(j × φ(x, y))
```

The phase mask φ functions as the stored weights of the optical network. Physical implementations could use spatial light modulators, diffractive optical elements, or metasurface structures. In PHOTEX, masks are optimized via backpropagation during training.

---

## 4. Optical Attention via Wave Interference

Two coherent fields E₁ and E₂ produce an interference intensity:

```
I = |E1 + E2|² = |E1|² + |E2|² + 2×Re(E1×E2*)
```

The cross-term `2×Re(E₁×E₂*)` is proportional to the inner product of the two field amplitudes, weighted by the cosine of their phase difference. This is physically equivalent to a dot product.

PHOTEX uses this mechanism to implement the Q·Kᵀ inner product in transformer self-attention: query and key vectors are encoded as complex optical fields, and their interference pattern at the detector computes the attention score. Softmax normalization and scaling by `1/√d_k` are applied in the digital post-processing layer.
