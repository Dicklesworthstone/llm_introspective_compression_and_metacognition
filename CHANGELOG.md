# Changelog

All notable changes to LLM Introspective Compression and Metacognition.

This project has no formal releases or version tags. The changelog is organized chronologically by commit. The repository is a research paper/proposal (not a library), so changes track content evolution rather than API surfaces.

Repository: <https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition>

---

## 2026-02-21 -- License and Social Preview

### License

- **Added MIT License with OpenAI/Anthropic Rider** restricting use by OpenAI, Anthropic, their affiliates, and anyone acting on their behalf without express written permission from Jeffrey Emanuel. The rider controls in case of conflict with other license terms. ([`70147f7`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/70147f7eee4ac74ec0c79b61d9a5b88933896dcc))

### Repository Metadata

- **Added GitHub social preview image** (`gh_og_share_image.png`, 1280x640) for consistent link previews when sharing the repository URL on social media. ([`1ecac17`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/1ecac179f20edbd8edd0b51d8bdc7eec40ae3fc2))

---

## 2025-04-01 -- Initial Publication

The entire paper was published in a rapid sequence of commits on April 1, 2025. The content covers a novel proposal for sidecar transformer architectures that compress and reconstruct internal model states, enabling reasoning backtracking, thought trajectory optimization, and metacognitive control.

### Paper Content (README)

- **Initial commit**: Created repository with a placeholder README describing the project scope -- "A novel approach for transformer model introspection that enables saving, compressing, and manipulating internal thought states for advanced capabilities like reasoning backtracking, latent thought optimization, and metacognitive control." ([`855969c`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/855969c76b4aeb52754eceec6fdcd989738dafa0))

- **Full paper written into README** (1042 lines), replacing the placeholder with the complete research proposal. Key sections introduced: ([`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087))
  - **Problem framing**: Lack of introspection and ephemeral cognition in transformers
  - **Theoretical foundation**: Low-dimensional manifold hypothesis for transformer activations
  - **Sidecar architecture**: Lightweight encoder/decoder pairs (`E`, `D`) riding alongside a frozen host transformer (`T_main`), compressing hidden states `h_t` and KV caches into latent codes `z_t`
  - **Three compression architectures** with full PyTorch implementations:
    - Layer-specific encoder/decoder (per-layer fidelity)
    - Grouped layer compressor (cross-layer efficiency)
    - Unified state compressor (single latent space, most parameter-efficient)
  - **KV cache compressor**: Conv1d-based sequence-aware compression with metadata encoding
  - **TransformerStateCompressor**: Unified manager integrating hidden-state and KV-cache compression
  - **Applications**: Reasoning backtracking, RL over thought trajectories, causal debugging, latent space exploration, memory-efficient checkpointing
  - **Addendum -- Metacognitive Operating System**: ThoughtGraph DAG for versioned cognition, Controller for branching proposals, self-coaching thought loops, curriculum learning, strategy distillation and transfer
  - **MetacognitiveAgent**: Full agent implementation combining encoder, decoder, controller, and coach
  - **References**: 8 cited papers (Tree of Thoughts, Self-Backtracking, Compressive Transformers, Universal Transformers, MuZero, Dreamer, etc.)

- **Minor title/byline edits**:
  - Changed byline from "and some collaborators" to "(and various collaborators of the electronic persuasion)" ([`e62fca8`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e62fca865abe1d51393051e8e731e9917c8cf66d))
  - Changed title from "Toward Real-Time Introspective Compression" to "Real-Time Introspective Compression for Transformers" ([`7d56020`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/7d560203e0ec0c86fe4bb93f0fcb9670d3aaaeba))

### Illustration

- **Added hero illustration** (`llm_introspection_illustration.webp`) depicting the introspective compression concept. ([`99d73a0`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/99d73a0fd7f813e87328adb391d0244656458cd5))

### PDF

- **Added PDF version** of the paper (`introspective_compression_for_llms.pdf`, ~140 KB). ([`035a761`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/035a761598ea1eb65b29424c1934261a9ca92bab))
- **Updated PDF** with minor revisions (142,614 -> 142,973 bytes). ([`ffbc621`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/ffbc6210ddae3cac9e26bffb09219dd7959498f0))

---

## Commit Index

| Date | Hash | Summary |
|------|------|---------|
| 2026-02-21 | [`70147f7`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/70147f7eee4ac74ec0c79b61d9a5b88933896dcc) | Add MIT License with OpenAI/Anthropic Rider |
| 2026-02-21 | [`1ecac17`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/1ecac179f20edbd8edd0b51d8bdc7eec40ae3fc2) | Add GitHub social preview image |
| 2025-04-01 | [`ffbc621`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/ffbc6210ddae3cac9e26bffb09219dd7959498f0) | Update PDF |
| 2025-04-01 | [`035a761`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/035a761598ea1eb65b29424c1934261a9ca92bab) | Add PDF of paper |
| 2025-04-01 | [`7d56020`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/7d560203e0ec0c86fe4bb93f0fcb9670d3aaaeba) | Finalize title |
| 2025-04-01 | [`e62fca8`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e62fca865abe1d51393051e8e731e9917c8cf66d) | Revise byline |
| 2025-04-01 | [`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087) | Write full paper into README |
| 2025-04-01 | [`99d73a0`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/99d73a0fd7f813e87328adb391d0244656458cd5) | Add illustration |
| 2025-04-01 | [`855969c`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/855969c76b4aeb52754eceec6fdcd989738dafa0) | Initial commit |
