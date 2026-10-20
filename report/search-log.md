# Search Method, Log and Limitations

## Method

1. **Phase A, broad scan.** 14 query families covering your stated interests and adjacent areas. I deliberately included areas outside RL and robotics: LLM agents for firmware, TinyML monitoring, missing-modality sensing, and energy-harvesting IoT.
2. **Phase B/C, targeted verification.** For each promising direction:
   - (a) searched for papers that already solve the proposed problem ("competitor search");
   - (b) chained to the closest competitors' venues and follow-ups;
   - (c) confirmed bibliographic details through proceedings or publisher listings where possible.
3. **Scoring.** Applied the rubric in your workbook (`Scoring Rubric` sheet) and its mandatory gates. A score of NE was used where evidence was insufficient (T-I novelty).

## Access limitations (material)

- `arxiv.org`, `export.arxiv.org`, `openreview.net`, `api.semanticscholar.org`, `api.openalex.org` and `api.crossref.org` were unreachable from this environment: the network proxy refused them, and the page-fetch tool could not resolve them.
- The only working source was a web-search tool. It returns result titles and URLs plus an automatically generated synthesis of the result pages. Some details therefore came from abstract pages, proceedings listings or secondary summaries rather than from the papers.
- No full text was read. Every novelty statement is limited to what abstracts and listings show. The evidence register grades each item V1, V2 or V3.
- Google Scholar, IEEE Xplore, ACM DL, ScienceDirect and SpringerLink were reached only indirectly, when search results surfaced their pages.
- Systems venues (SenSys, IPSN, MobiSys, EMSOFT, RTSS) were under-covered. This mainly affects T-I and the control-scheduling co-design side of T-A.

## Query families run (about 50 searches, 2026-10-20)

| # | Query family (abridged) | Main finding |
|---|---|---|
| 1 | early-exit calibration under distribution shift / corruptions | Multi-exit robustness under corruptions [E66]; SAFE-KD [E64] |
| 2 | conformal / risk control for early exits | Jazbec et al. NeurIPS 2024 [E62] |
| 3 | online conformal risk control, adaptive exit thresholds under drift | **UAT, NeurIPS 2025 [E63] closes T-G**; PromptShift-CRC [E67] |
| 4 | quantized RL policies on microcontrollers / integer hardware | Kresse & Lampert [E20]; Bohm et al. [E23]; Liu et al. [E08] |
| 5 | quantization-aware training, sim-to-real, robustness of quantized policies | Kresse hypothesis; QuaRL [E22]; **quant-control-bench model card [E26]**; SQIL [E25] |
| 6 | VLA quantization on LIBERO | QuantVLA, Ω-QVLA, ActQuant, BitVLA, EaqVLA, Mix-QVLA, FoldQuantVLA [E30-E35, E77] |
| 7 | quantized VLA robustness / LIBERO-Plus / compression and generalization | LIBERO-Plus [E27], LIBERO-PRO [E28], **VLAQuantBench [E29]**, GridS [E38], RLRC [E39] |
| 8 | test-time adaptation on microcontrollers | TinyTTA [E60], FORGE [E61] (screened out) |
| 9 | variable control rate / when to act | SEAC, MOSEAC [E11], SRL [E12], TempoRL [E13], Lazy-MDPs [E14] |
| 10 | early-exit policy networks in RL | **BEXA, ICML 2026 [E01]** (screens out the naive topic) |
| 11 | inference latency and delay in robot RL | Air Learning [E06], ARLI [E16], DA-DP [E18], OmniReset [E07], DCAC [E05], Real-Time RL [E04] |
| 12 | compute-delay coupling, network size versus delay, anytime control | Anokhin et al. [E02]; Riemer et al. [E03] quote; Cervin et al. [E10]; no direct T-A competitor found |
| 13 | early exits in closed-loop control / latency-aware early-exit RL 2026 | DeeR-VLA [E15]; EXPO-FT [E17]; no direct competitor found |
| 14 | TD-MPC2 adaptive planning compute / when to plan | TD-MPC [E49], metacontrol [E51], AOP [E52], adaptive RL-MPC [E53], uncertainty-as-risk critique [E54] |
| 15 | LLMs for embedded firmware with hardware-in-the-loop | EmbedBench [E44], IoT-SkillsBench [E43], Englhardt et al. [E45] |
| 16 | LLM agents with logic-analyzer or oscilloscope feedback | Only vendor and trade-press items [E46, E47]; RTLFixer-type HDL work |
| 17 | TinyML uncertainty / conformal on microcontrollers | QUTE [E56], SNAP-UQ [E57], TCUQ [E58], UR2M [E59] |
| 18 | quantization and calibration / out-of-distribution detection | Bouguerra et al. ICML 2026 [E37]; ViT quantization and OOD (Kuang & Wong, arXiv 2602.01459) |
| 19 | sensor failure / missing modality, wearable HAR | [E68-E71] (crowded) |
| 20 | energy harvesting, adaptive sampling, intermittent multi-exit inference | Zygarde [E72], Wu et al. [E73], RL sampling [E74] (systems venues under-covered) |
| 21 | one-step diffusion or consistency policy distillation | ManiCM, RL-100, OFP [E75] (screened out) |
| 22 | RL evaluation reliability | rliable [E19] |
| 23 | bibliographic verification queries | Venue and author confirmation for E01-E07, E10, E13-E15, E19, E20, E43, E44, E49, E51, E56, E57, E62-E65, E68 |

## Recommended follow-up searches (to run with full-text access)

1. Google Scholar "cited by" for E01 (BEXA) and E02 (Anokhin et al.), filtered to 2026.
2. Full-text keyword search inside E01, E15 and E16 for "delay", "latency", "wall-clock" and "real-time".
3. IEEE Xplore: ("anytime" OR "variable computation time") AND ("reinforcement learning" OR "neural network controller") AND ("jitter" OR "delay"), 2020-2026, in TAC, CSL, RTSS and EMSOFT.
4. Full text of VLAQuantBench (E29): check for perturbation-suite evaluations.
5. arXiv cs.SE and cs.AR, plus ICSE/FSE/ASE 2026: "hardware-in-the-loop" AND ("LLM" OR "agent") AND ("logic analyzer" OR "trace").
6. Code availability for E56-E58.
