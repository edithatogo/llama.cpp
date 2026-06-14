# Native llama.cpp ColBERT Architecture Support

## Overview

Define and, in a future implementation pass, add native ColBERT architecture support to llama.cpp. ColBERT uses contextualized late interaction, where retrieval quality depends on token-level embeddings and MaxSim scoring rather than only a single pooled vector.

This track is intentionally separate from the current Ollama LFM2-ColBERT conversion/docs PR. The Ollama PR focuses on converting SentenceTransformers-style LFM2-ColBERT checkpoints and preserving embedding metadata. This llama.cpp track is future runtime architecture work and must not be merged into that conversion/docs scope.

## Target Model

- **LFM2-ColBERT-350M** by Liquid AI (1024-dim embeddings)
- HuggingFace: `LiquidAI/LFM2-ColBERT-350M`

## Scope

In scope for this future track:

- Native llama.cpp runtime architecture and loader behavior needed for ColBERT-style models.
- GGUF metadata and tensor naming needed to distinguish pooled-vector compatibility from token-level late-interaction output.
- Conversion contract for ColBERT checkpoints into GGUF, including dense projection head metadata.
- Reference-output parity plan against a trusted Python/Hugging Face implementation.
- Server/API behavior for exposing pooled embeddings and, if accepted upstream, token-level embeddings or MaxSim-ready outputs.

Out of scope for this track:

- Current Ollama conversion/docs PR #16195.
- Uploading or redistributing third-party model weights.
- Ad hoc binary patching of GGUF files.
- Product-specific retrieval ranking UI or application logic.
- Claiming native MaxSim support before a token-level output contract and parity tests exist.

## Functional Requirements

- [ ] Define the native architecture contract for ColBERT-like models in llama.cpp.
- [ ] Define GGUF metadata keys for pooling mode, normalization, dense projection dimensions, token-level embedding output, and MaxSim compatibility.
- [ ] Decide whether ColBERT should be represented as a new architecture, an LFM2 embedding variant, or a capability flag layered on an existing architecture.
- [ ] Define tensor naming for LFM2-ColBERT dense projection and normalization tensors.
- [ ] Implement loader changes only after the architecture/tensor contract is approved.
- [ ] Implement conversion changes only after the target GGUF metadata contract is approved.
- [ ] Add parity tests against reference Python/Hugging Face outputs for pooled and token-level embeddings.
- [ ] Document API/server behavior and limitations before proposing upstream runtime changes.

## Non-Functional Requirements

- Backward compatible with existing model formats
- No regression to existing LFM2, LFM2-MoE, or embedding model loading.
- Test fixtures must be license-safe and small enough for upstream review.
- Performance should be measured separately for pooled embedding output and token-level late-interaction output.
- The implementation must fail closed when a GGUF lacks required ColBERT metadata.
- Quantization behavior must be documented and validated before support claims.

## Acceptance Criteria

1. The track contains an approved architecture design note covering model type/capability representation, metadata, tensor naming, and API behavior.
2. A license-safe fixture strategy exists for validating conversion and runtime output.
3. llama.cpp can load a valid ColBERT GGUF without breaking existing LFM2/LFM2-MoE models.
4. Pooled embedding output matches the reference implementation within an explicitly documented tolerance.
5. Token-level embedding or MaxSim-ready output behavior is either implemented and tested, or explicitly deferred with a documented API gap.
6. Conversion tooling emits the required metadata and rejects incomplete ColBERT checkpoints with actionable diagnostics.
7. Documentation explains the difference between pooled vector embeddings and ColBERT late-interaction retrieval.

## Current State

This is a granular future track. No native llama.cpp runtime changes are being implemented as part of the current Ollama conversion/docs PR.
