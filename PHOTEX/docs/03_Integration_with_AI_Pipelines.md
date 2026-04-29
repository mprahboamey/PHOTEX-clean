# Integration with AI Pipelines

PHOTEX is designed to slot into existing AI infrastructure as an optical inference backend — without changing how applications, frameworks, or workflows are built today.

---

## The Core Idea

The compute-intensive part of AI inference is the forward pass: attention, MLP layers, the math. Everything else — tokenization, sampling, routing, orchestration — is already handled by the stack you run today.

PHOTEX targets only that forward pass. The rest of your pipeline stays exactly as-is.

---

## Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                  CLIENT / APPLICATION                        │
│    (any inference client, framework, or API consumer)        │
└──────────────────────────────────────────────────────────────┘
                             │
                    Standard API interface
                             │
                             ▼
┌──────────────────────────────────────────────────────────────┐
│               INFERENCE ORCHESTRATOR                         │
│  Routing · Load balancing · Tokenization · Model mapping     │
└──────────────────────────────────────────────────────────────┘
                             │
              ┌──────────────┴──────────────┐
              │                             │
              ▼                             ▼
┌─────────────────────┐       ┌─────────────────────────────┐
│   GPU / TPU Backend  │       │     PHOTEX Optical Backend  │
│  (existing stack)    │       │   Optical forward pass only │
└─────────────────────┘       └─────────────────────────────┘
```

The orchestration layer speaks the same API contract as today's inference services. Clients don't need to know what's underneath.

---

## What Stays Digital vs Optical

| Stage | Where It Runs |
|-------|---------------|
| Tokenization | Digital |
| Embedding lookup | Digital |
| **Forward pass (attention + MLP)** | **Optical (PHOTEX)** |
| Logits → token sampling | Digital |
| Detokenization | Digital |
| Training and fine-tuning | GPU (unchanged) |

The optical device handles the expensive part. The bookkeeping stays digital.

---

## Deployment Models

| Mode | Description |
|------|-------------|
| **Managed IaaS** | PHOTEX as a hosted API. Send prompts, receive completions — same interface as any inference service. |
| **Hybrid** | Orchestrator routes hot-path inference to PHOTEX; GPU handles fallback and cold paths. |
| **On-premises** | PHOTEX hardware deployed in your data center. Control plane and routing are local or hybrid. |

All three modes share the same API contract. Switching between them is an orchestration decision, not an application change.

---

## Adoption Path

The typical pattern is phased: validate outputs in shadow mode, then incrementally shift inference traffic as confidence builds. The goal is full inference offload to PHOTEX, with GPUs reserved for training and fallback.

Details on integration and deployment are available to qualified partners.

---

## Summary

| Question | Answer |
|----------|--------|
| **Where does PHOTEX plug in?** | As the optical backend for the inference forward pass, behind a standard API layer. |
| **What changes for the customer?** | Minimal — the API contract stays the same. |
| **What stays on GPU?** | Training, fine-tuning, tokenization, sampling, and fallback. |
| **Business model** | Inference-as-a-Service: customer sends prompts, PHOTEX handles the optical compute. |
