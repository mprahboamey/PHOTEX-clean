# PHOTEX

**AI inference, performed by light.**

PHOTEX is a personal notebook posing an optical neural network picture: inference imagined through volumetric holographic media, wave propagation, and interference—not only through transistor bookkeeping. Documentation and toy benchmark framing live here because I wanted somewhere to anchor the optics math while it’s still messy.

Citation metadata for whoever indexes that sort of thing: [`CITATION.cff`](CITATION.cff).

**Mprah-Boamey** — messy scratchpad collection: github.com/mprahboamey

## Project Status

Technical documentation plus benchmark-ish assumptions spelled out openly. Half lab report, half daydream sketch.

## At a Glance

| Metric | NVIDIA H100 | PHOTEX (Projected) |
|--------|-------------|--------------------|
| **Parameters** | ~20-35B | **90T** raw, ~45T usable |
| **Latency** | ~7-10 ms | **~1.3 ns** |
| **Energy per pass** | ~mJ | ~pJ-fJ |
| **Throughput** | 100-150 TPS | **7.5M-375M TPS** |

> Values shown for PHOTEX are architecture projections/simulated assumptions as documented in this repository.

## Sketch of the architecture

Imagine a compact photonic story where volumetric holographic crystals (stacked in 3D and angle-addressed) hold model weights encoded as phase-ish structures—not a roadmap slide, just the picture in my head laid out cleanly.

- **Wave interference as compute**: key pieces argued optically instead of only transistor matrix pipelines.
- **Parameter packing story**: angular and spatial multiplexing in the thought experiment crank density per volume.
- **Latency picture**: propagation distance sets a tiny forward-pass intuition.
- **Energy picture**: photonic folklore lands in picojoule-ish scale in the scribbles.

## Documentation

| Document | Description |
|----------|-------------|
| [Wave Propagation and ASM](docs/01_Wave_Propagation_and_ASM.md) | Helmholtz equation, angular spectrum propagation, and optical attention foundations |
| [Benchmark Results](docs/02_Benchmark_Results.md) | Simulated comparisons for parameter density, energy, throughput, and latency |
| [Integration with AI Pipelines](docs/03_Integration_with_AI_Pipelines.md) | Loose notes on plugging the thought experiment into ordinary AI stacks |
| [Digital Architecture](docs/04_Digital_Architecture.md) | Control planes and digital bits that would sit outside any crystal toy model |

## Related repository

The software-only tile bank experiment lives here if you want weights repacked into mmap tiles without the full optics folktale layer:

[HoloWeights](https://github.com/mprahboamey/holoweights)

## How it hangs together (at a high level)

1. **Wave optics as compute**: ASM-style propagation, FFT complexity, hand-wavy but written down.
2. **Weights as phase structures**: parameters live in holographic-phase stories in the sketch.
3. **Attention via interference**: interference terms like `Re(E1 * conj(E2))` as the physical correlation picture.
4. **Volume not just a die**: storage metaphors lean 3D rather than a single plane.

## Contributing

If something is unclear and you want to clarify wording, cool.

1. Fork the repository.
2. Branch.
3. Pull request with a short note about what you tweaked.

## License

MIT. See [`LICENSE`](LICENSE).

## References

Background reading; nothing here claims outside validation of PHOTEX numbers.

- Goodman, J. W. (2005). *Introduction to Fourier Optics*
- Lin, X., et al. (2018). "All-optical machine learning using diffractive deep neural networks." *Science*
- Miller, D. A. B. (2017). "Attojoule optoelectronics." *Journal of Lightwave Technology*
