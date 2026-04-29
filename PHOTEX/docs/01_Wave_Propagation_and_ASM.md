# Wave Propagation and the Physics of Photonic Inference

PHOTEX performs computation by propagating light through structured optical media. This document establishes the mathematical foundation: how light fields evolve in space, how that evolution is computed efficiently, how crystal layers encode trainable transformations, and how wave interference physically implements attention.

---

## 1. Wave Optics Fundamentals

### 1.1 The Wave Equation

Light propagation is governed by the wave equation:

```
∇²E - (1/c²)∂²E/∂t² = 0
```

For monochromatic waves (single frequency), this simplifies to the **Helmholtz equation**:

```
(∇² + k²)E = 0
```

where `k = 2π/λ` is the wavenumber.

### 1.2 Complex Representation

Monochromatic waves are represented as complex exponentials:

```
E(x, y, z, t) = A(x, y, z) × exp(j(ωt - k·r))
```

For computational purposes, we work with the complex amplitude:

```
u(x, y, z) = A(x, y, z) × exp(-jφ(x, y, z))
```

where **A** is the real, positive amplitude and **φ** is the phase in radians.

### 1.3 Intensity

The intensity (power per unit area) is proportional to the squared amplitude:

```
I(x, y, z) = |u(x, y, z)|² = A²(x, y, z)
```

---

## 2. Angular Spectrum Method (ASM)

To propagate a light field from one plane to another, we need to solve the Helmholtz equation as a function of z. The Angular Spectrum Method does this in the frequency domain — where each spatial frequency component propagates independently — enabling O(N log N) computation via FFT rather than direct spatial integration.

### 2.1 Derivation

Starting from the Helmholtz equation rearranged for z:

```
∂²u/∂z² = -(∂²u/∂x² + ∂²u/∂y² + k²u)
```

Taking the 2D Fourier transform in the (x, y) plane:

```
U(fx, fy, z) = ∫∫ u(x, y, z) exp(-j2π(fx·x + fy·y)) dx dy
```

Using the derivative property of the Fourier transform (FT[∂²f/∂x²] = -(2πfx)²F), the equation becomes:

```
∂²U/∂z² = [(2πfx)² + (2πfy)² - k²]U = -kz²U
```

This is a simple harmonic oscillator in z, with solution:

```
U(fx, fy, z) = U(fx, fy, 0) × H(fx, fy, z)
```

where the **propagation transfer function** is:

```
H(fx, fy, z) = exp(j × kz × z)
```

and:

```
kz = sqrt(k² - (2πfx)² - (2πfy)²)
```

For propagating waves (kz real), H applies a pure phase shift. For evanescent components (kz imaginary), H becomes a decaying exponential — these are filtered out.

### 2.2 Evanescent Waves

When `(2πfx)² + (2πfy)² > k²`:

```
kz = j × sqrt((2πfx)² + (2πfy)² - k²)
```

These evanescent waves decay exponentially with distance and do not carry energy. They are excluded from the propagation computation.

### 2.3 Algorithm

1. **Input**: Complex field `u(x, y, 0)` at plane z = 0
2. **FFT**: Compute angular spectrum `U(fx, fy, 0) = FFT[u(x, y, 0)]`
3. **Multiply**: Apply transfer function `U(fx, fy, z) = U(fx, fy, 0) × H(fx, fy, z)`
4. **IFFT**: Recover propagated field `u(x, y, z) = IFFT[U(fx, fy, z)]`

**Complexity**: O(N log N) per propagation step, where N is the number of spatial pixels.

### 2.4 PHOTEX Parameters

| Parameter | Value |
|-----------|-------|
| Wavelength | 405 nm |
| Pixel size | 1 µm |
| Layer spacing | 10 µm |
| Refractive index | 1.5 |

---

## 3. Phase Modulation — Diffractive Layers

A diffractive layer introduces a position-dependent phase shift to the passing field:

```
u_out(x, y) = u_in(x, y) × exp(j × φ(x, y))
```

where φ(x, y) is the phase mask. This is the mechanism by which model weights are encoded in PHOTEX: each diffractive layer is a spatially varying phase structure written into the crystal. The phase mask is learned during training and fixed at inference time — light passing through it undergoes the transformation it encodes.

Phase masks can be physically realized using Spatial Light Modulators (SLMs), Diffractive Optical Elements (DOEs), metasurfaces, or holographically structured crystal.

---

## 4. Optical Attention via Wave Interference

When two waves superpose, the resulting intensity includes an interference term:

```
I = |E1 + E2|² = |E1|² + |E2|² + 2×Re(E1×E2*)
```

The cross-term `2×Re(E1×E2*)` is the key. For complex vectors Q and K encoding query and key respectively, `Re(Q·K*)` is equivalent to this interference term — it is maximized when the two fields are aligned in phase and amplitude (constructive interference) and minimized when they are out of phase (destructive interference).

This is dot-product attention, computed physically. Similar tokens produce constructive interference at the detector; dissimilar tokens cancel. No circuits compute this — the wave physics does.

PHOTEX uses this mechanism to implement the inner product step of transformer attention inside the crystal. The 1/√d_k scaling and softmax normalization are handled in the digital pipeline surrounding the optical core.
