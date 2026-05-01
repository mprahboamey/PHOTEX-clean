# Integration with AI Pipelines

PHOTEX does not replace an entire inference stack. It targets one specific bottleneck. This document sketches where optical inference would slot in and what would stay exactly as-is.

---

## Core concept

The computationally expensive portion of transformer inference is the forward pass: attention blocks and MLP layers. Everything surrounding that core (tokenization, logits sampling, batch scheduling, API serving) is already handled well by existing digital infrastructure and would remain unchanged.

The proposed integration point is the forward pass only, replacing matrix multiplication with optical computation while keeping the surrounding stack intact.

```
┌──────────────────────────────────────────────────────────────┐
│                  CLIENT / APPLICATION                        │
└──────────────────────────────────────────────────────────────┘
                             │
                     standard API layer
                             │
                             ▼
┌──────────────────────────────────────────────────────────────┐
│               INFERENCE ORCHESTRATOR                         │
│  routing · tokenizer · batch management · load balancing     │
└──────────────────────────────────────────────────────────────┘
                             │
              ┌──────────────┴──────────────┐
              │                             │
              ▼                             ▼
┌─────────────────────┐       ┌─────────────────────────────┐
│   GPU / TPU stack   │       │   PHOTEX optical backend    │
│   (baseline)        │       │   (forward pass only)       │
└─────────────────────┘       └─────────────────────────────┘
```

The client-facing API remains the same regardless of which backend handles the forward pass.

---

## Digital vs. optical responsibilities

| Stage | Where it runs |
|-------|-----------------------------------------|
| Tokenization | Digital CPU |
| Embedding lookup | Digital SRAM |
| **Forward pass (attention + MLP)** | **Optical (projected target)** |
| Logits sampling and decoding | Digital |
| Detokenization | Digital CPU |

Training and fine-tuning remain on GPU.

---

## Deployment considerations

A hybrid routing layer could direct traffic between GPU and optical backends based on load or latency metrics. Shadow validation and regression testing would be required before shifting production traffic to any new backend.

Any deeper integration requires hardware prototypes outside simulation.

---

## Summary

The integration point is narrow: the optical backend handles the forward pass hot path only. The orchestration layer, API surface, and all surrounding digital operations remain unchanged. GPU handles training, fallback inference, and everything outside the forward pass.
