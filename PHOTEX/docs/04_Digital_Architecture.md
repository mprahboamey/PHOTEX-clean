# Digital Architecture

PHOTEX routes matrix-intensive operations to optical hardware while a thin digital layer handles the transformer operations that require global statistics, discrete lookups, or nonlinear activations that passive optical elements cannot perform.

---

## What remains digital

Several transformer operations depend on per-token statistics or global sequence reductions that cannot be straightforwardly performed by passive optical elements:

| Operation | Why it stays digital |
|-----------|----------------------------------|
| Layer normalization | Requires per-token mean and variance across the feature dimension |
| Softmax | Requires a global reduction over the full logit vector |
| GELU, SiLU activations | Require precise nonlinear shaping not achievable with passive optics |
| Residual addition | Straightforward vector add; no benefit from optical routing |
| Embedding lookup | Discrete table indexing, not a continuous field operation |

Operations that scale quadratically with sequence length — primarily the attention matrix multiply — are the primary optical targets.

---

## Estimated digital overhead

For a transformer block modeled on an 8B parameter architecture:

| Component | Approximate operation count |
|-----------|----------------|
| **Optical forward pass** | **~157 million** |
| Digital overhead (all operations above) | ~109 thousand |

The digital component represents approximately **0.07%** of total operations per block. At that ratio, the digital layer's power draw is closer to microcontroller-scale than H100-scale — though this estimate has not been validated on hardware.

---

## Scaling behavior

The quadratic scaling of the attention matrix multiply means the optical share of total computation grows with sequence length and model size. The digital overhead operations scale more favorably, so the architectural split becomes more advantageous at larger scales.

At very long context lengths, KV cache storage becomes a second bottleneck. One million tokens at one trillion parameters requires approximately four terabytes of KV cache, consuming hundreds of watts in HBM bandwidth alone. The hypothesis that volumetric holographic storage could also serve as a KV cache — reducing HBM bandwidth dependence — is noted here as a direction for future investigation. It has not been demonstrated in simulation or hardware.

---

## Engineering challenges

The projected numbers above are derived from simulation. Realizing them on hardware requires solving manufacturing yield, optical insertion loss, coherent stability over temperature, and packaging thermal management. These are the dominant engineering risks, not the architectural model.
