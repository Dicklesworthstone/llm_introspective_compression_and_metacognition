# Changelog

All notable changes to **LLM Introspective Compression and Metacognition**.

This repository contains a research proposal (not a library); there are no formal version tags or GitHub releases. Changes are organized by capability area within each date section, with live commit links.

Repository: <https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition>

---

## 2026-02-21 -- Licensing and Repository Presentation

### Intellectual Property

- **MIT License with OpenAI/Anthropic Rider** -- Added a custom license that grants standard MIT permissions to the public while explicitly revoking all rights from OpenAI, Anthropic, their affiliates, and anyone acting on their behalf, unless Jeffrey Emanuel provides express written permission. The rider is self-enforcing: it auto-terminates all permissions on breach and entitles the licensor to injunctive relief and attorney fee recovery. ([`70147f7`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/70147f7eee4ac74ec0c79b61d9a5b88933896dcc))

### Social Sharing

- **GitHub social preview image** (`gh_og_share_image.png`, 1280x640) -- Provides consistent Open Graph previews when the repository URL is shared on social media platforms. ([`1ecac17`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/1ecac179f20edbd8edd0b51d8bdc7eec40ae3fc2))

---

## 2025-04-01 -- Initial Publication

The entire research proposal was published in a rapid sequence of nine commits on April 1, 2025. The sections below are organized by the capability areas introduced, not by commit order.

### Core Theoretical Framework

Introduced the two foundational problems the paper addresses and the theoretical insight that motivates the solution:

- **Lack of introspection** -- Transformer LLMs have no built-in mechanism to access their own internal activations (feed-forward outputs, attention maps, KV tensors).
- **Ephemeral cognition** -- Internal states are discarded after each forward pass; naive recording is computationally prohibitive (hundreds of MB per sequence for GPT-3-scale models).
- **Low-dimensional manifold hypothesis** -- Despite high dimensionality, transformer activations likely occupy a structured, lower-dimensional subspace shaped by pretraining dynamics, architectural constraints, semantic priors, and task-driven optimization. This makes learned compression feasible.

([`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087))

### Sidecar Compression Architecture

Proposed a lightweight sidecar model that rides alongside a frozen host transformer (`T_main`), encoding internal state into compact latent codes `z_t`:

- **Sidecar Encoder (`E`)** -- Takes the current token, prior latent code `z_{t-1}`, and a tap into a subset of `T_main`'s hidden states to produce `z_t`.
- **Sidecar Decoder (`D`)** -- Reconstructs hidden states and KV tensors from `z_t`.
- **Training objective** -- Weighted reconstruction loss (`hidden MSE + KV MSE`) plus a regularization term `R(z_t)` encouraging structured, low-entropy latent representations.

([`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087))

### Three Compression Architectures (with full PyTorch implementations)

Provided complete, runnable PyTorch code for three distinct architectural strategies, each with different trade-offs:

1. **Layer-Specific Encoder/Decoder** (`LayerSpecificEncoderDecoder`) -- One encoder-decoder pair per transformer layer. Highest per-layer fidelity; supports parallel training. Recommended for research applications requiring precise introspection.
2. **Grouped Layer Compressor** (`GroupedLayerCompressor`) -- Compresses groups of K layers jointly. Captures some cross-layer dependencies while reducing parameter count. Recommended as the default approach.
3. **Unified State Compressor** (`UnifiedStateCompressor`) -- A single attention-based encoder-decoder for all layers, with learned layer embeddings. Most parameter-efficient; best at capturing cross-layer relationships. Recommended for memory-constrained environments.

([`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087))

### KV Cache Compression

Introduced a specialized `KVCacheCompressor` module addressing the unique challenges of key-value caches (variable length, critical role in autoregressive generation):

- **Conv1d-based sequence-aware compression** -- Convolutional encoder/decoder preserves sequential structure during compression and reconstruction.
- **Metadata encoding** -- Separate network encodes layer index, head index, and position information to guide reconstruction.
- **Configurable compression ratio** (default 0.25x dimensionality).

([`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087))

### Unified Compression Manager

`TransformerStateCompressor` -- An integrating class that:

- Accepts any of the three hidden-state compression architectures via a `compressor_type` parameter.
- Combines hidden-state and KV-cache compression into a single `compress_state` / `decompress_state` interface.
- Includes `evaluate_reconstruction` for measuring per-layer MSE and per-head KV reconstruction quality.
- Preliminary benchmarks suggest 8-16x hidden-state compression and 4x KV-cache compression are achievable.

([`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087))

### Application: Reasoning Backtracking

Described rewinding a model to any past internal state to explore alternative continuations -- critical for deduction, search, and hypothesis testing. Example use case: multi-hop QA where the model misinterprets a clue and backtracks to reweight attention.

([`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087))

### Application: RL Over Thought Trajectories

Proposed optimizing latent codes `z_t` directly via reinforcement learning, enabling meta-level control over *how* the model thinks rather than just *what* it outputs. Includes pseudocode for a perturbation-based policy loop over saved cognitive states.

([`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087))

### Application: Causal Debugging and Latent Exploration

- **Causal debugging** -- Trace hallucinations or logic errors back to the internal state where drift began; compare faulty and corrected paths via latent diffs.
- **Latent space exploration** -- Edit or interpolate in `z_t` space to explore counterfactuals ("What if the model interpreted this term differently?").
- **Memory-efficient checkpointing** -- Checkpoint and resume long-running agent loops or multi-turn planning with minimal storage.

([`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087))

### Metacognitive Operating System (Addendum)

Extended the core proposal into a vision for a full metacognitive operating system for transformers:

- **ThoughtGraph** -- A directed acyclic graph where each `z_t` is a node representing a cognitive state. Edges represent continuation, intervention, or counterfactual alteration. Effectively version control for cognition.
- **Controller** -- A neural network that proposes multiple latent continuations from any node (speculative branching), enabling "try four continuations," "backtrack to step 7," or "merge insights from different branches."
- **Self-Coaching Thought Loops** -- Replay branches, compare outcomes, and train a coach module to guide future latent trajectories. Implements curriculum learning that targets the most challenging reasoning tasks.
- **Strategy Distillation** (`StrategyDistiller`) -- Compress successful reasoning patterns into transferable strategy embeddings. Evaluates generality via cross-task similarity, transfer gain, perturbation robustness, reuse ratio, and strategy lifespan.
- **MetacognitiveAgent** -- A complete agent combining encoder, decoder, controller, and coach with `branch_and_score`, `edit_and_retry`, and task interaction capabilities.

([`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087))

### Challenges and Limitations

Documented known open problems: compression-fidelity trade-off, computational overhead of sidecar inference, KV cache compression difficulty, training data requirements for generalization, latent space quality for RL/editing, and the gap between MSE-based and functionally equivalent reconstruction metrics.

([`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087))

### References

Cited 8 papers grounding the proposal in existing work: Tree of Thoughts (Yao et al. 2023), Self-Backtracking (Yang et al. 2025), Reasoning with Latent Thoughts (Saunshi et al. 2025), Compressive Transformers (Rae et al. 2020), Dynamic Memory Compression (Nawrot et al. 2024), Universal Transformers (Dehghani et al. 2019), MuZero (Schrittwieser et al. 2020), and Dreamer (Hafner et al. 2020).

([`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087))

### Visual and Document Assets

- **Hero illustration** (`llm_introspection_illustration.webp`) depicting the introspective compression concept. ([`99d73a0`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/99d73a0fd7f813e87328adb391d0244656458cd5))
- **PDF of the paper** (`introspective_compression_for_llms.pdf`, ~140 KB). ([`035a761`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/035a761598ea1eb65b29424c1934261a9ca92bab))
- **Revised PDF** with minor corrections (142,614 -> 142,973 bytes). ([`ffbc621`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/ffbc6210ddae3cac9e26bffb09219dd7959498f0))

### Title and Byline Refinements

- Changed title from "Toward Real-Time Introspective Compression for Transformers" to "Real-Time Introspective Compression for Transformers" (removing the tentative "Toward"). ([`7d56020`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/7d560203e0ec0c86fe4bb93f0fcb9670d3aaaeba))
- Changed byline from "and some collaborators of the electronic persuasion" to "(and various collaborators of the electronic persuasion)". ([`e62fca8`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e62fca865abe1d51393051e8e731e9917c8cf66d))

### Repository Bootstrapping

- **Initial commit** -- Created repository with a placeholder README describing the project scope. ([`855969c`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/855969c76b4aeb52754eceec6fdcd989738dafa0))

---

## Commit Index

| Date | Hash | Summary |
|------|------|---------|
| 2026-02-21 | [`70147f7`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/70147f7eee4ac74ec0c79b61d9a5b88933896dcc) | MIT License with OpenAI/Anthropic Rider |
| 2026-02-21 | [`1ecac17`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/1ecac179f20edbd8edd0b51d8bdc7eec40ae3fc2) | GitHub social preview image |
| 2025-04-01 | [`ffbc621`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/ffbc6210ddae3cac9e26bffb09219dd7959498f0) | Revised PDF |
| 2025-04-01 | [`035a761`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/035a761598ea1eb65b29424c1934261a9ca92bab) | PDF of paper |
| 2025-04-01 | [`7d56020`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/7d560203e0ec0c86fe4bb93f0fcb9670d3aaaeba) | Finalize title |
| 2025-04-01 | [`e62fca8`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e62fca865abe1d51393051e8e731e9917c8cf66d) | Revise byline |
| 2025-04-01 | [`e380bdc`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/e380bdc2dbaa911532b80dae08ca7aa226317087) | Full paper written into README |
| 2025-04-01 | [`99d73a0`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/99d73a0fd7f813e87328adb391d0244656458cd5) | Hero illustration |
| 2025-04-01 | [`855969c`](https://github.com/Dicklesworthstone/llm_introspective_compression_and_metacognition/commit/855969c76b4aeb52754eceec6fdcd989738dafa0) | Initial commit |
