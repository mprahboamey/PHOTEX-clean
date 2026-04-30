# Wave Propagation and the Physics of Photonic Inference

Here is where I stash the bookkeeping for pretending PHOTEX could route serious inference through sculpted light paths. Computation means propagating modes through picky media until layers impose trainable tweaks and interference mocks attention vibes.

---

## 1. Wave Optics Fundamentals

### 1.1 The Wave Equation

Light propagation obeys the wave equation everybody quotes on day one lectures:

```
∇²E - (1/c²)∂²E/∂t² = 0
```

Monochromatic light collapses fuss into Helmholtz form:

```
(∇² + k²)E = 0
```

Where `k = 2π/λ` behaves as textbook wavenumber.

### 1.2 Complex Representation

Assume single tone fields so bookkeeping drops time oscillations into exp:

```
E(x, y, z, t) = A(x, y, z) × exp(j(ωt - k·r))
```

Computational folklore keeps complex amplitude peeled out:

```
u(x, y, z) = A(x, y, z) × exp(-jφ(x, y, z))
```

**A** carries magnitude, φ carries phase in radians, everyone pretends analytic signals survive sampling.

### 1.3 Intensity

Measured intensity tracks squared modulus because detectors care about watts not cute phase:

```
I(x, y, z) = |u(x, y, z)|² = A²(x, y, z)
```

---

## 2. Angular Spectrum Method (ASM)

Helmholtz wants z evolution. ASM jumps to Fourier space because each tilted plane wave component multiplies cleanly by propagation phase factor, FFT chain hits O(N log N) complexity instead of naïve brute integration.

### 2.1 Derivation

Reorder Helmholtz along z axis:

```
∂²u/∂z² = -(∂²u/∂x² + ∂²u/∂y² + k²u)
```

Transform horizontal slices:

```
U(fx, fy, z) = ∫∫ u(x, y, z) exp(-j2π(fx·x + fy·y)) dx dy
```

Derivative folklore gives:

```
∂²U/∂z² = [(2πfx)² + (2πfy)² - k²]U = -kz²U
```

Harmonic oscillator in z yields:

```
U(fx, fy, z) = U(fx, fy, 0) × H(fx, fy, z)
```

With transfer function:

```
H(fx, fy, z) = exp(j × kz × z)
```

and:

```
kz = sqrt(k² - (2πfx)² - (2πfy)²)
```

Real kz inserts pure retardation onto propagating bundles. Imaginary kz turns into exponential decay dumping evanescent tails.

### 2.2 Evanescent Waves

Whenever `(2πfx)² + (2πfy)² > k²`:

```
kz = j × sqrt((2πfx)² + (2πfy)² - k²)
```

Modes die away fast, negligible energy transport, computations drop them politely.

### 2.3 Algorithm

Take complex field snapshot `u(x, y, 0)`, FFT to spectrum, multiply spectral bins by propagation mask, invert FFT to land at next depth.

**Complexity**: O(N log N) propagation hop with N lateral pixels honest count.

### 2.4 PHOTEX Parameters

| Parameter | Value |
|-----------|-------|
| Wavelength | 405 nm |
| Pixel size | 1 µm |
| Layer spacing | 10 µm |
| Refractive index | 1.5 |

---

## 3. Phase Modulation: Diffractive Layers

A sheet multiplies traversing amplitude by programmable spiral factor:

```
u_out(x, y) = u_in(x, y) × exp(j × φ(x, y))
```

That mask φ embodies “weights” in my crystal cartoon. Hardware reality might flirt with SLMs, DOEs, metasurface stacks, sculpted crystal nonsense. Inference phase locks masks after training settles.

---

## 4. Optical Attention via Wave Interference

Overlapping coherent fields slap detector with intensity:

```
I = |E1 + E2|² = |E1|² + |E2|² + 2×Re(E1×E2*)
```

Cross term `2×Re(E1×E2*)` mirrors dot correlations when embeddings ride light analogous to queries and keys. Similar patterns interfere constructively while unlike ones attenuate without inventing transistor dot units.

PHOTEX leans this mechanism toward the inner product step inside transformer-style attention diagrams. Scaling such as `1/√d_k` plus softmax normalization still ride the digital scaffolding wrapped around whichever optical slab story remains honest.
