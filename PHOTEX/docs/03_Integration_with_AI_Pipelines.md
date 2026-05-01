# Integration with AI Pipelines

Notebook musings about where PHOTEX could theoretically sit relative to stacks people already swear at. Assume nothing ships until crates exist outside my hard drive imagination.

---

## Core idea

The sharp end of transformer inference hides inside forward passes juggling attention blocks and chunky MLPs. Everything outward from that hot core (tokenizer plumbing, logits sampling, batch schedulers, boring HTTP JSON) happily remains digital folklore we already tolerate.

Sketch below keeps API skin familiar while fantasizing swapping one backend slab for hypothetical optics babysitting quadratic matmul avalanche only.

```
┌──────────────────────────────────────────────────────────────┐
│                  CLIENT / APPLICATION                        │
│    (whatever inference consumer already trusts)               │
└──────────────────────────────────────────────────────────────┘
                             │
                     normal API façade
                             │
                             ▼
┌──────────────────────────────────────────────────────────────┐
│               INFERENCE ORCHESTRATOR                         │
│  routing · hashing · tokenizer glue · backlog babysitting    │
└──────────────────────────────────────────────────────────────┘
                             │
              ┌──────────────┴──────────────┐
              │                             │
              ▼                             ▼
┌─────────────────────┐       ┌─────────────────────────────┐
│   GPU / TPU stack   │       │ Imaginary PHOTEX backend    │
│  (baseline reality) │       │ optics-on-forward-pass only │
└─────────────────────┘       └─────────────────────────────┘
```

Clients stare at whichever API contract survives version drift. Transparent routing toggles hypothetical until hardware cooperates academically.

---

## Digital versus optical bookkeeping

| Stage | Where it realistically runs early drafts |
|-------|-----------------------------------------|
| Tokenization | Still digital CPUs |
| Embedding lookup tables | Still digital SRAM paths |
| **Forward avalanche (attention + MLP slabs)** | **Optical folklore target** maybe someday |
| Logits onward through sampling gymnastics | Still digital |
| Detoken fluff | Same tired CPUs |

Training plus fine tuning remain GPU burdens nobody pretends vanished.

---

## Deployment fairy tales classmates sketch

Hybrid routing fantasies bounce traffic between reassurance GPUs and flirtatious optics nodes when metrics stable. Hosted versus on-premises arguments mirror mundane cloud chatter without promising pricing pages.

Assume shadow validation plus regression nets still mandatory before courageous traffic shifts.

Anything deeper waits until hardware prototypes exist outside simulation.

---

## Summary chatter

Plug-in point hides behind whichever orchestrator already fronts inference today. Customer surfaces stay identical REST nonsense. GPUs pick up leftovers including training jitter. PHOTEX story targets forward hot path only assuming engineers someday align benches with marketing paragraph dreams.
