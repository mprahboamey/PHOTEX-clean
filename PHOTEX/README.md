# PHOTEX

**AI inference, performed by light.**

PHOTEX is an optical neural network concept focused on photonic inference using volumetric holographic media. Instead of performing inference with switching transistors, PHOTEX explores computation through wave propagation and interference.

## Project Status

This repository currently contains technical documentation and benchmark assumptions for the PHOTEX architecture. It is research-stage and documentation-first.

## At a Glance

| Metric | NVIDIA H100 | PHOTEX (Projected) |
|--------|-------------|--------------------|
| **Parameters** | ~20-35B | **90T** raw, ~45T usable |
| **Latency** | ~7-10 ms | **~1.3 ns** |
| **Energy per pass** | ~mJ | ~pJ-fJ |
| **Throughput** | 100-150 TPS | **7.5M-375M TPS** |

> Values shown for PHOTEX are architecture projections/simulated assumptions as documented in this repository.

## What We Are Building

PHOTEX proposes a compact photonic inference accelerator that uses volumetric holographic crystals (stacked in 3D and angle-addressed) to store and execute model weights.

- **Wave interference as compute**: key operations are executed optically instead of transistor-based matrix pipelines.
- **Extreme parameter density**: angular and spatial multiplexing target much higher density per unit volume.
- **Nanosecond-class inference path**: forward-pass latency is driven by optical propagation distance.
- **Low energy operation**: photonic compute is intended to operate in picojoule-to-femtojoule regimes.

## Documentation

| Document | Description |
|----------|-------------|
| [Wave Propagation and ASM](docs/01_Wave_Propagation_and_ASM.md) | Helmholtz equation, angular spectrum propagation, and optical attention foundations |
| [Benchmark Results](docs/02_Benchmark_Results.md) | Simulated comparisons for parameter density, energy, throughput, and latency |
| [Integration with AI Pipelines](docs/03_Integration_with_AI_Pipelines.md) | How PHOTEX can fit into existing AI infrastructure as an inference backend |
| [Digital Architecture](docs/04_Digital_Architecture.md) | Required digital control systems and scaling behavior outside the crystal |

## How It Works

1. **Wave optics as compute**: light propagates through media using the Angular Spectrum Method (ASM), with FFT-based complexity.
2. **Weights encoded as phase structures**: parameters are stored as holographic phase patterns.
3. **Attention via interference**: interference terms such as `Re(E1 * conj(E2))` provide physical correlation behavior.
4. **Capacity scales with volume**: storage is volumetric rather than limited to planar die area.

## Contributing

Contributions that improve technical clarity are welcome.

1. Fork the repository.
2. Create a feature branch.
3. Open a pull request with a concise description of your change.

## License

This project is licensed under the MIT License. See [`LICENSE`](LICENSE).

## References

These references provide background context and do not imply external validation of PHOTEX results.

- Goodman, J. W. (2005). *Introduction to Fourier Optics*
- Lin, X., et al. (2018). "All-optical machine learning using diffractive deep neural networks." *Science*
- Miller, D. A. B. (2017). "Attojoule optoelectronics." *Journal of Lightwave Technology*
