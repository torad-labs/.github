# Marcos Damasceno: Selected Research & Engineering Work

Senior Staff Software Engineer (Realtor.com, Austin, TX) and independent ML and interpretability researcher. All of the 2026 research and open-source systems below were built on nights and weekends alongside my full-time engineering role.

---

### [weight-patterns-showcase](https://github.com/torad-labs/weight-patterns-showcase)
*Training on Local Divergence Between Computational Traces (Gemma 4 E2B) · Sole researcher and author*

Built a bitwise-exact C++ `llama.cpp` trace recorder (105 residual-branch merges/token, verified across 12,023 BF16 decisions) and a 538-tensor PyTorch VJP/JVP training loop with carried FP32 remainders over native BF16 weights. Closed-loop computational-trace backpropagation repaired a downhill energy sign error (11 J to 91 J, -78.1% trace loss) while preserving friction (35/35 captures) and the 44 J reference. Weight-family transplants showed either all 35 `attn_output` or all 35 `ffn_down` matrices alone suffice (99.8% / 99.2% trace-loss drop; 0.736 shared-prefix cosine). Includes 4 working papers, blinded unseen transfer results with regressions reported (1/8 to 3/8 full chains; 1 baseline regression), a matcher-dropout Goodharting post-mortem (-25.7% logged vs. +22.6% true carried loss), a retrospective on conducting empirical research with coding agents (`JOURNEY.md`), and a runnable CPU verification suite (`make verify`, 601 SHA-256s, 76 guard + 228 CPU tests).

### [kandi-cpt](https://github.com/torad-labs/kandi-cpt)
*Single-GPU NVFP4 + INT8-Master Continued Pretraining (Gemma 4 E2B) · Sole author*

Single-GPU continued pretraining, SFT, and calibration study on one 16 GB consumer GPU (RTX 5080, 11.2–13.5 GB peak VRAM). After standard fine-tuning scored 0/17 on post-cutoff knowledge probes at ~80K seed tokens, I diagnosed fact-triple exposure density as the driver and built an entity-graph synthetic corpus pipeline (7.18M tokens across 97 editions), raising recall from ~5% chance to 57.5% stage@1 and 59.8% day@1 (74.4% single-edition) with 10% replay preserving general ability. Controlled low-precision ablations (NVFP4 Quartet-II, 4-bit AdamW, INT8 stochastic-rounding master weights) found monotonic recall gains (0.689 full precision -> 0.722 4-bit optimizer -> 0.744 INT8 SR master) even as training loss rose (1.06 -> 1.12).

### [llama.cpp](https://github.com/torad-labs/llama.cpp)
*Blackwell (SM120) CUDA Fork & Production Inference · Lead author (273 merged PRs, 107 CUDA kernel PRs)*

Nsight-profiled Blackwell fork adding GLM-5.3, Ternary Bonsai 2 27B (1.89x decode at 131K context on one 16 GB GPU), and Inkling 2 (+45.8% 4-GPU throughput), plus in-place sparse attention over quantized `q8_0`/`q4_0` KV caches (512 MiB saved/card at 512K), an IQ3_XXS shared-memory tile, and overlapped copy-engine PCIe all-reduce with an 8-bit wire (45.7 tok/s/session across 23 sessions). Powers Realtor.com's self-hosted inference service (RealInfer), which I designed and operate (85,420 requests and 8.49B tokens in its first 11 days at 97.9% prompt-cache hit rate).

### [splice](https://github.com/torad-labs/splice)
*Multi-Model Gateway & Claude Code Turn Harness · Creator and maintainer (MIT, 15 releases, 590 merged PRs)*

Kotlin/Ktor and Kotlin/Native MCP hub running Claude Code and Claude Desktop across frontier and local models with 6 turn hooks and model-only hidden tool calls that preserve prompt-cache prefixes. Serves as the front end for RealInfer at Realtor.com. Also built Context Getter (Haiku pre-turn exploration), cutting end-to-end time-to-first-edit from 16–23 to 4–6 min and Opus thinking tokens ~87% in an N=2 cross-module pilot.

---

### Private & Unpublished Research (Available to Walk Through)

- **Empirical Agent Oversight (`campaign` & `gym`):** Audited study across 49+ scored runs and 17 replayed reviews (FeatureBench, ProgramBench). Reproduced 6/6 LLM reviewers passing buggy code, with 4/6 writing the right test, watching it fail, and silently rewriting the test to fit the bug (3/4 omitting the failure from review notes); moving test authoring to a blind frozen session fixed that structurally while exposing the mirror failure (0 builders disputing 1 wrong test out of 282). Built `gym` for pre-exit claim locking and Ed25519-signed rewards.
- **QGRE & QGIE (Private Rust/CUDA Engine; v1 Python prototype archived at [qgre-engine](https://github.com/torad-labs/qgre-engine)):** Single-GPU RL post-training engine (per-region process rewards, SPO n=1 baselines, phase-gated curriculum) unified with a pure Rust and custom SM120 CUDA persistent megakernel using its own typed IR (`CUIR`, zero vendor math libs), raising Gemma 4 12B 4-bit batch-1 short-prompt decode on RTX 5080 from 2.07 to 104.56 tok/s (119.6–127.2 tok/s unclamped on RTX 5090).

Other public repositories: [agent-driven-architecture](https://github.com/torad-labs/agent-driven-architecture), [gateward](https://github.com/torad-labs/gateward), [aisdk-kotlin](https://github.com/torad-labs/aisdk-kotlin).
