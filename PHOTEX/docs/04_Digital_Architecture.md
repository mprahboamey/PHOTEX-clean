# Digital Architecture

The PHOTEX crystal handles matrix multiplications — the dominant, quadratically-scaling cost of transformer inference. A thin digital shell wraps the optical core to handle a small set of operations that are structurally incompatible with passive light propagation.

---

## What Stays Digital

Not everything in a transformer can be expressed as light passing through a medium. An operation stays digital if it requires knowing the current input's statistics, requires a global reduction over all values before producing any output, or is a discrete lookup rather than a continuous transformation.

In practice, this means:

| Operation | Why Digital |
|-----------|-------------|
| Layer normalization | Requires input-dependent mean and variance — changes every token |
| Softmax | Requires a global sum over all sequence positions before normalizing |
| Nonlinear activations (GELU, SiLU) | Curve shape incompatible with the crystal's optical nonlinearity |
| Residual connections | Beam routing complexity; trivial as a digital vector add |
| Embedding lookup | Discrete index — not a continuous field transformation |

Everything that scales quadratically with model size — the matrix multiplies — goes in the crystal. Everything that stays digital scales linearly with embedding dimension or sequence length.

---

## How Small the Digital Shell Is

For a single transformer block at 8B-model scale:

| Component | Operations |
|-----------|-----------|
| **Optical (matrix multiplies)** | **~157 million** |
| Digital (all operations above) | ~109 thousand |

**The digital shell is approximately 0.07% of total compute per block.**

This is not a GPU workload. It is embedded hardware — a microcontroller drawing milliwatts, not kilowatts.

---

## Scaling to Trillion-Parameter Models and Long Context

As models scale, the digital shell remains proportionally small. The matrix multiplies grow as d² (quadratic); the digital operations grow as d or n (linear). The larger the model, the more dominant the optical side becomes.

The one challenge that emerges at scale — and that defeats GPU systems at long context — is not compute but memory: the KV cache (the stored keys and values from every past token, needed for attention over long sequences). At 1M token context on a 1T-parameter model, the naive KV footprint approaches 4 TB. On a GPU system, reading this on every forward step requires hundreds of watts of HBM bandwidth.

PHOTEX's optical architecture addresses this at the device level. The same physical principles that store model weights can absorb context state — eliminating the memory bandwidth bottleneck rather than managing it. Softmax normalization and attention scoring are handled through optical techniques that keep the digital shell in the same power envelope at million-token context as at short context.

Details on these mechanisms are available to qualified partners.

---

## The Net Effect

At any model size, at any context length, the digital shell remains sub-watt embedded compute. Data-center scale power draw does not re-enter the picture. The optical device scales up; the digital wrapper does not.
