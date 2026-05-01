# PHOTEX

**AI inference using light.**

PHOTEX simulates optical neural networks in PyTorch. The model is a holographic crystal stack: trainable diffractive layers act as weights, wave propagation via the Angular Spectrum Method handles computation, and wave interference implements self-attention.

The benchmark tables compare projected optical performance against published NVIDIA H100 figures. The projections are striking. They come from simulation, and this documentation says so clearly.

Citation: [`CITATION.cff`](CITATION.cff)

**Mprah-Boamey** · `github.com/mprahboamey`

---

## Project status

Simulation framework complete. Benchmarks run. Technical documentation and a provisional patent application have been drafted. No physical hardware exists yet.

---

## At a glance

| Metric | NVIDIA H100 | PHOTEX (Projected) |
|--------|-------------|--------------------|
| **Parameters** | ~20–35B | **90T** raw, ~45T usable |
| **Latency** | ~7–10 ms | **~1.3 ns** |
| **Energy per pass** | ~mJ | ~pJ–fJ |
| **Throughput** | 100–150 TPS | **7.5M–375M TPS** |

> All PHOTEX values are architecture projections or simulator outputs. No physical device has been measured.

---

## Architecture

The core device is a volumetric holographic crystal stack. Stacked diffractive layers impose trainable phase shifts on a coherent optical field. Angular multiplexing addresses up to 900 independent channels through the same physical volume. Wave interference between encoded query and key fields computes dot-product attention physically.

A 1 cm³ crystal stores an estimated 90 trillion parameters under the stated geometry: 1,000 Z-layers at 10 µm spacing × 900 angular channels × 100 million pixels per layer.

---

## Documentation

| Document | Contents |
|----------|----------|
| [Wave Propagation + ASM](docs/01_Wave_Propagation_and_ASM.md) | Helmholtz equation, Angular Spectrum Method, interference-based attention |
| [Benchmark Results](docs/02_Benchmark_Results.md) | Parameter density, energy, throughput, and latency projections vs H100 |
| [Integration with AI Pipelines](docs/03_Integration_with_AI_Pipelines.md) | How PHOTEX fits into a standard inference stack |
| [Digital Architecture](docs/04_Digital_Architecture.md) | Which operations remain digital and why |

---

## Related

Software-only weight tiling and memory-mapping experiments (no photonics): [HoloWeights](https://github.com/mprahboamey/holoweights)

---

## Contributing

Issues and pull requests are welcome.

---

## License

MIT. See [`LICENSE`](LICENSE).

---

## References

- Goodman, J. W. (2005). *Introduction to Fourier Optics*
- Lin, X., et al. (2018). "All-optical machine learning using diffractive deep neural networks." *Science*
- Miller, D. A. B. (2017). "Attojoule optoelectronics." *Journal of Lightwave Technology*
