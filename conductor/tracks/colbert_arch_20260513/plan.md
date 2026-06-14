# Implementation Plan: Native llama.cpp ColBERT Architecture Support

## Phase 0: Track Boundary and Review Readiness

- [x] Task: Split native llama.cpp ColBERT architecture work from the current Ollama conversion/docs PR
    - [x] Record that PR #16195 remains conversion/docs scoped.
    - [x] Record that native runtime work belongs in this future llama.cpp track.
    - [x] Keep all code changes out of this setup pass.
- [x] Task: Make the future track granular enough to implement safely later
    - [x] Define scope and explicit out-of-scope boundaries in `spec.md`.
    - [x] Define dependency gates in `metadata.json`.
    - [x] Break implementation into architecture, converter, runtime, API, validation, and upstream-readiness phases.

## Phase 1: Architecture Contract

- [ ] Task: Study existing embedding and late-interaction-adjacent architecture patterns
    - [ ] Review current llama.cpp embedding model loader paths.
    - [ ] Review current LFM2 and LFM2-MoE architecture handling.
    - [ ] Review GGUF metadata conventions for pooling, normalization, and embedding length.
- [ ] Task: Decide the representation model
    - [ ] Compare a new ColBERT architecture against an LFM2 embedding capability flag.
    - [ ] Document the compatibility impact for existing LFM2 GGUF files.
    - [ ] Choose the least invasive upstreamable representation.
- [ ] Task: Define ColBERT GGUF metadata
    - [ ] Specify pooling mode metadata.
    - [ ] Specify normalization metadata.
    - [ ] Specify dense projection input/output dimensions.
    - [ ] Specify token-level output or MaxSim readiness metadata.
- [ ] Task: Conductor - Automated Review and Checkpoint 'Architecture Contract' (Protocol in workflow.md)

## Phase 2: Converter Contract and Fixtures

- [ ] Task: Define license-safe fixture strategy
    - [ ] Identify whether synthetic tensor fixtures are sufficient for converter tests.
    - [ ] Identify whether any real checkpoint-derived fixture can be redistributed.
    - [ ] Record license/provenance constraints before adding binary fixtures.
- [ ] Task: Define converter output contract
    - [ ] Map SentenceTransformers module layout to GGUF tensor names.
    - [ ] Specify required metadata for dense projection heads.
    - [ ] Specify failure diagnostics for incomplete checkpoints.
- [ ] Task: Add converter tests
    - [ ] Test root and nested safetensors module inputs.
    - [ ] Test duplicate tensor-name rejection after replacements.
    - [ ] Test missing dense projection metadata failure.
- [ ] Task: Conductor - Automated Review and Checkpoint 'Converter Contract and Fixtures' (Protocol in workflow.md)

## Phase 3: Native Runtime Loader

- [ ] Task: Implement loader support after the architecture contract is approved
    - [ ] Add or extend model architecture registration.
    - [ ] Add tensor-name handling for dense projection and normalization tensors.
    - [ ] Preserve existing LFM2 and LFM2-MoE loading behavior.
- [ ] Task: Implement pooled embedding output
    - [ ] Support mean pooling when metadata requires it.
    - [ ] Support normalized and unnormalized output according to metadata.
    - [ ] Report embedding dimensions correctly.
- [ ] Task: Define token-level output behavior
    - [ ] Decide whether server/API can expose token-level embeddings directly.
    - [ ] If not, document MaxSim as deferred API work.
    - [ ] Add fail-closed diagnostics for unsupported token-level requests.
- [ ] Task: Conductor - Automated Review and Checkpoint 'Native Runtime Loader' (Protocol in workflow.md)

## Phase 4: Validation and Performance

- [ ] Task: Add reference-output parity tests
    - [ ] Compare pooled outputs against Python/Hugging Face reference outputs.
    - [ ] Compare token-level outputs if implemented.
    - [ ] Define numeric tolerances per dtype/quantization mode.
- [ ] Task: Add regression tests
    - [ ] Test existing LFM2 and LFM2-MoE model loading.
    - [ ] Test malformed/missing ColBERT metadata.
    - [ ] Test batch embedding behavior.
- [ ] Task: Add performance notes
    - [ ] Benchmark pooled output separately from token-level output.
    - [ ] Record memory impact for representative sequence lengths.
    - [ ] Record quantization behavior and limitations.
- [ ] Task: Conductor - Automated Review and Checkpoint 'Validation and Performance' (Protocol in workflow.md)

## Phase 5: Upstream Readiness

- [ ] Task: Prepare upstream patch series
    - [ ] Keep architecture contract, converter tests, runtime loader, and docs reviewable.
    - [ ] Avoid mixing Ollama-specific behavior into llama.cpp.
    - [ ] Link to the separate Ollama conversion/docs PR only as context.
- [ ] Task: Documentation
    - [ ] Explain pooled vector embeddings versus ColBERT late-interaction retrieval.
    - [ ] Document required GGUF metadata.
    - [ ] Document unsupported paths and future API gaps.
- [ ] Task: Submit or stage PR to ggml-org/llama.cpp
    - [ ] Rebase onto current upstream main before submission.
    - [ ] Run focused converter/runtime tests.
    - [ ] Run broader regression tests appropriate for touched paths.
- [ ] Task: Conductor - Automated Review and Checkpoint 'Upstream Readiness' (Protocol in workflow.md)
