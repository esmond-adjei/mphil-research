# MPhil Research Topic Discovery and Evaluation

Prepared: 2026-10-20. Companion files: [`evidence-register.md`](evidence-register.md) (Deliverable E, sources cited as `[E##]`), [`search-log.md`](search-log.md) (method, queries, limitations) and [`../mphil_research_topic_evaluation.xlsx`](../mphil_research_topic_evaluation.xlsx) (scored matrix and per-criterion evidence log).

## Read this first: evidence limits

The scholarly APIs and full-text hosts (arXiv, OpenReview, Semantic Scholar, OpenAlex, Crossref) could not be reached from this research environment. All evidence comes from web-search results: proceedings and publisher listings, arXiv abstract pages, and in some cases secondary summaries. **No full paper text was read.** As a result:

- Bibliographic details are graded in the evidence register: V1 means a proceedings or publisher listing confirmed them, V2 means an arXiv preprint listing only, and V3 means only a secondary summary or vendor page supports them.
- Every "no prior work found" statement means only that about 50 targeted searches found nothing. It is not proof that the gap exists.
- Every topic below has a named list of closest competitors. Reading them in full is the mandatory first task, and it can change the ranking.

Labels used throughout: **[Verified]** means stated in a primary listing or abstract. **[Inference]** is my reasoning. **[Unresolved]** needs checking.

---

## Deliverable A: Executive summary

**What the literature shows.** Across the areas you listed (adaptive inference, resource-aware RL, edge AI, IoT, uncertainty, intelligent software), most of the obvious topics are already taken or crowded as of October 2026:

- Early-exit actors for RL already exist (BEXA, ICML 2026 [E01]).
- VLA quantization methods are crowded: at least eight 2025-26 papers plus a closed-loop benchmark [E29-E35].
- Microcontroller test-time adaptation is taken (TinyTTA, NeurIPS 2024; FORGE, 2026 [E60, E61]).
- Online risk-controlled early-exit thresholds under distribution shift are taken (UAT, NeurIPS 2025 [E63]).
- One-step diffusion-policy distillation is taken [E75].

The opportunities that survive tend to sit at a seam between two literatures that each assume away the other's main variable.

| Question | Answer | Score / 100 |
|---|---|---|
| **Strongest candidate overall** | **T-A: Delay-consistent adaptive computation for real-time RL control.** Efficient-policy research (early exits, quantization, compact policies) counts FLOPs or wall-clock savings but evaluates control as if inference were instantaneous [E01, E20, E22]. Real-time and delayed-RL research treats delay as fixed or exogenous, not as something the policy's own compute choice creates [E02-E05]. On embedded hardware, inference time *is* control delay [E06-E09]. | 80.0 (Strong, shortlist) |
| **Most novel** | T-A, for the same reasons. No paper found couples a state-dependent compute choice to the delay it induces. Its novelty depends on the full text of BEXA [E01], DeeR-VLA [E15] and ARLI [E16]. | 80.0 |
| **Most feasible** | T-F (quantized policies under sim-to-real stressors) and T-G (risk-controlled early exit under drift): commodity simulation, small models, no special hardware. Both **fail the novelty gate**. Among topics that pass the gate, T-A and T-B are the most feasible. | 69.0 / 67.0 (gate not met) |
| **Strongest practical and career value** | T-B, instrument-in-the-loop LLM agents for embedded firmware (AI-engineering and tooling skills, an obvious beneficiary). T-C, robustness of compressed VLAs (robot foundation models) is a close second. | 71.0 / 72.0 |

**Major uncertainties still open:**

1. Whether BEXA or DeeR-VLA already model the delay their compute choice induces. Only their abstracts were seen.
2. Whether compute-induced delay is large enough to matter on realistic hardware for state-based policies. This is the premise of T-A, and the month-2 minimum experiment tests it.
3. Your supervisor's expertise and the lab's actual hardware. Research-environment fit is scored 3 everywhere because neither is confirmed.
4. GPU specifics, which decide whether T-C is feasible.

**Missing information that could change the recommendations:**

| Missing fact | Why it matters |
|---|---|
| Supervisor's research area | Environment fit (10%) and the realistic scope of T-A versus T-B |
| GPU model, count and queue policy | T-C needs roughly 300-600 GPU-hours of 7B-model inference. T-A and T-D need many parallel CPU or GPU runs. |
| Which boards, sensors and instruments the lab owns (STM32, ESP32, Raspberry Pi, Jetson, logic analyzer) | T-A's hardware-in-the-loop validation and T-B's testbed |
| 6 months or 12? Is a publication required? | Determines whether the "full" or "fallback" scope in Deliverable D applies |
| Budget for LLM API calls | T-B's model coverage |
| Does your department accept an empirical or benchmark contribution as an MPhil thesis? | T-E, T-C and the negative-result paths of T-A |

---

## Deliverable B: Ranked candidate table

Scores use the mandated rubric: Overall = Σ(weight × rating) / 5, with weights 15/20/20/15/10/10/10. The spreadsheet computes the same values with formulas.

Ranking rules:

- Topics that pass every gate rank above topics that fail one.
- Within 2 points, ties go to the higher combined novelty plus feasibility, then to rigor.
- T-B (71.0) ranks above T-C (72.0) under this rule: novelty plus feasibility is 7 for T-B and 6 for T-C.

| Rank | Research topic | Primary research question | Proposed contribution | Sig | Nov | Feas | Rigor | Impact | Career | Env | Total | Confidence | Key risk |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | **T-A** Delay-consistent adaptive computation for real-time RL control | When inference time becomes control delay, does a state-dependent compute policy trained under delay-consistent dynamics beat the best fixed-size, delay-aware policy on the return-energy frontier? | A problem formulation in which the compute choice sets the delay; a delay-consistent training method; a test of whether efficient-policy rankings flip once measured latency is applied | 4 | 4 | 4 | 4 | 4 | 5 | 3 | **80.0** | Medium | The BEXA or DeeR-VLA full text already models delay; or the delay is too small to matter for state-based tasks |
| 2 | **T-B** Instrument-in-the-loop feedback for LLM firmware agents | Do decoded logic-analyzer and timing traces raise LLM agents' fix rate on firmware that compiles but misbehaves, compared with compiler and serial feedback, at an equal iteration budget? | Controlled study, open hardware-in-the-loop (HIL) harness and fault-seeded task suite | 4 | 3 | 4 | 3 | 4 | 4 | 3 | **71.0** | Medium-low | Fast-moving field (vendors already ship agent-instrument links); flaky hardware oracle |
| 3 | **T-C** Does compression amplify fragility? Perturbation robustness of quantized and pruned VLA policies | Is the success lost to quantization or pruning larger under controlled perturbations (LIBERO-Plus) than on nominal LIBERO, and can nominal-data diagnostics predict it? | First paired compression × perturbation evaluation; predictive diagnostic | 4 | 3 | 3 | 4 | 4 | 5 | 3 | **72.0** | Medium-low | Saturated area (VLAQuantBench, Sept 2026); heavy GPU load |
| 4 | **T-D** State-dependent planning budgets for latent world-model MPC (TD-MPC2) | Can a cheap state-dependent signal allocate MPPI iterations to match fixed-budget return at lower average or peak planning compute? | Modern revisit of metacontrol and adaptive planning on TD-MPC2, with latency-aware evaluation | 3 | 3 | 4 | 4 | 3 | 4 | 3 | **69.0** | Medium | Partly anticipated by Hamrick 2017 and AOP 2019; savings may be trivial |
| 5 | **T-E** Independent, hardware-grounded benchmark of label-free TinyML uncertainty monitors | Do the reported gains of QUTE, TCUQ and SNAP-UQ hold on common microcontrollers under realistic drift? | Independent replication and benchmark | 3 | 3 | 4 | 4 | 3 | 3 | 3 | **67.0** | Medium | Benchmark-only contribution; code availability for TCUQ and SNAP-UQ unknown |
| 6 | T-F Low-bit quantized control policies under sim-to-real stressors | Does quantization-aware training make policies more robust to dynamics shift and delay, not only input noise? | Tests Kresse & Lampert's hypothesis | 3 | **2** | 5 | 4 | 3 | 4 | 3 | 69.0 | Medium | **Novelty gate failed.** A public "quant-control-bench" artifact already sweeps friction and noise at nine precisions [E26]; see also SQIL [E25] |
| 7 (tie) | T-G Risk-controlled early exiting under realistic drift | Do risk-controlled exit rules keep their guarantees under temporal sensor drift? | Stress test and online recalibration | 3 | **2** | 5 | 4 | 3 | 3 | 3 | 67.0 | Medium | **Novelty gate failed.** UAT (NeurIPS 2025) already adapts exit thresholds online with risk guarantees under shift [E63] |
| 7 (tie) | T-H Sensor-failure-robust on-device activity recognition | Can one compact model degrade gracefully under missing sensors within a microcontroller memory budget? | Memory-bounded robustness method | 3 | **2** | 5 | 4 | 3 | 3 | 3 | 67.0 | Medium | **Novelty gate failed.** Crowded [E68-E71] |
| — | T-I Joint RL sensing and multi-exit intermittent inference on energy-harvesting nodes | Can one RL controller jointly schedule sensing and exit depth under harvested energy? | Joint controller | 3 | **NE** | 3 | 3 | 3 | 3 | 2 | **Not computed** (54-62 for any plausible novelty score) | Low | Systems venues (SenSys, IPSN) were not searched adequately; specialist expertise needed. Below 65 even at novelty 4, so reject or redesign either way. |

Confidence reflects how much the score could move once full texts and supervisor information are in. **Sensitivity of the leader:** T-A stays "Strong" (76.0) if novelty drops to 3. It drops to "Conditional" (72.0) only if novelty and feasibility both drop to 3.

### Screened out before scoring (evidence that the direction is taken or saturated)

| Direction | Why it was dropped | Evidence |
|---|---|---|
| Early-exit actor networks for RL | Done: budgeted early-exit actor with SAC and TD3 on MuJoCo | [E01] |
| New post-training quantization method for VLAs | At least eight 2025-26 methods plus a 94,574-episode closed-loop benchmark | [E29-E35, E77] |
| Test-time adaptation on microcontrollers | TinyTTA (NeurIPS 2024), FORGE (2026) | [E60, E61] |
| One-step or consistency distillation of diffusion policies for fast inference | ManiCM, RL-100, OFP (more than 100x faster action generation) | [E75] |
| Online adaptive early-exit thresholds | UAT (NeurIPS 2025) | [E63] |

---

## Phase C: Gap statements

### T-A: Delay-consistent adaptive computation (full statement)

- **Established knowledge.**
  - Early-exit actors cut actor inference cost with little loss of return under a compute budget [E01; Verified, abstract].
  - Network depth creates observation delay, which temporal skip connections can mitigate [E02; Verified].
  - Random delays can be corrected off-policy [E05] and in the real-time actor-critic formulation [E04].
  - Variable control rates [E11], action sequences for slow hardware [E12] and "learning when to act" [E13, E14] are established.
  - Control engineering has long quantified how latency and jitter degrade control performance [E10].
- **Existing limitation.** In efficient-policy work, the compute saving is reported as FLOPs or speed while closed-loop return is measured with instantaneous inference [E01, E20, E22; Inference from abstracts]. In delay work, delay is fixed or exogenous rather than a consequence of the policy's own state-dependent compute choice [E02-E05]. Riemer et al. note that state-adaptive computation time "is relatively uncommon in the literature" [E03; Verified via search excerpt of the paper].
- **Evidence that the coupling matters.**
  - Running on a Raspberry Pi instead of a desktop changed aerial-robot trajectories substantially [E06; numbers from a secondary summary].
  - OmniReset policies were smooth at about 40 ms end-to-end latency but jittery at 60 ms [E07].
  - A $10-class Cortex-M0 at 5 MHz caps policy update rate, and the authors select gaits statically per power budget [E08].
  - A low-cost quadruped had more than 50 ms of transport delay [E09].
- **Unresolved question.** When the compute choice determines the delay and energy of each action, does state-dependent compute help or hurt control compared with a fixed small network? Does accounting for this change which efficient architecture looks best?
- **Proposed contribution.**
  1. A compute-delay-coupled formulation with a hardware-profiled latency model.
  2. A delay-consistent adaptive-exit training method.
  3. An evaluation protocol showing whether zero-delay rankings of efficient policies survive measured latency.
- **Verification status.** Plausible but unconfirmed. Targeted searches (six query families, see the search log) found no direct prior work, but the BEXA [E01], DeeR-VLA [E15] and ARLI [E16] full texts are unread.
- **Novelty risk.**
  - BEXA may already simulate wall-clock delay.
  - DeeR-VLA has a "peak compute (latency)" criterion [E15].
  - ARLI and Real-Time EXPO-FT (Aug-Sep 2026) [E16, E17] handle latency for large VLA policies. A follow-up adding adaptive compute is plausible within months.

### T-B: Instrument-in-the-loop LLM firmware agents (full statement)

- **Established knowledge.** LLMs generate embedded firmware with partial success. EmbedBench reports DeepSeek-R1 at 55.6% pass@1 with schematics and at most 29.4% on ESP-IDF migration [E44; ICSE 2026, Verified]. Concise human-written "skills" documents push hardware-validated success close to perfect on IoT-SkillsBench (42 tasks, 3 platforms, 378 runs) [E43; Verified abstract]. A 2023 hardware-in-the-loop study reported GPT-4 producing working I2C interfaces in 66% of trials [E45].
- **Existing limitation.**
  - Benchmarks classify failures as compile, behavior, or correct [E43].
  - EmbedBench's feedback is compiler output and retrieval [E44].
  - No controlled study found measures whether giving the agent **instrument observations** (decoded bus traces, timing measurements) helps it repair *behavior* failures.
  - Vendors are shipping exactly this coupling (Saleae MCP; Embedder with SEGGER) with no independent reliability data [E46, E47; V3].
- **Unresolved question.** Does trace-level feedback improve behavior-correct repair rates, and for which fault classes (timing, protocol, configuration, logic)?
- **Proposed contribution.** A controlled multi-model study, an open low-cost HIL harness built on sigrok-compatible analyzers, and a fault-seeded task suite with a failure taxonomy.
- **Verification status.** Plausible, unconfirmed. One targeted search family; the LLM-agent literature moves fastest of all candidates.
- **Novelty risk.** High churn. A 2026-27 paper from the IoT-SkillsBench group, or tool-vendor evaluations, could pre-empt the study. The defensible core is the controlled fault-class analysis.

### T-C: Compressed VLA robustness (full statement)

- **Established knowledge.**
  - Low-bit VLA quantization preserves nominal LIBERO success (QuantVLA, Ω-QVLA, ActQuant, BitVLA, EaqVLA [E30-E35]).
  - VLAQuantBench evaluates post-training quantization in closed loop (409 runs, 94,574 episodes, plus a physical robot) with **task success as the primary outcome** [E29; Verified abstract].
  - VLAs are fragile under modest perturbations: LIBERO-Plus reports drops from 95% to below 30% [E27]. LIBERO-PRO raises similar concerns [E28].
  - For CLIP and VLMs, quantization can improve calibration and robustness to noise but not to covariate shift, depending on pretraining data [E37].
- **Existing limitation.** No study found evaluates compressed VLAs on perturbation suites. Token-pruning papers report *improved* out-of-distribution success [E38, E39], which conflicts with an intuitive "compression hurts robustness" story. The interaction is unknown.
- **Unresolved question.** Is the compression × perturbation interaction negative, neutral or positive, which axes drive it, and can nominal action-deviation diagnostics predict it?
- **Verification status.** Plausible, unconfirmed. VLAQuantBench's full text may include perturbation tests.
- **Novelty risk.** High. This is a crowded area with a short half-life, and the VLM analogue already exists [E37].

### T-D: Planning budgets for TD-MPC2 (summary)

- **Known.**
  - TD-MPC runs a fixed number of MPPI iterations; inference planning cost can be cut by 50% without loss on many tasks, while hard tasks benefit from more planning [E49].
  - TD-MPC2 keeps fixed-iteration planning [E50].
  - A learned metacontroller can allocate imagination compute [E51].
  - AOP stops CEM iterations early on relative improvement [E52].
  - Recent work adapts MPPI sample aggregation by value uncertainty [E53].
  - Model uncertainty can be a poor risk signal [E54].
- **Gap.** No study found evaluates state-dependent planning budgets for modern latent world models under real-time latency.
- **Status.** Plausible; partly anticipated. Novelty risk is medium.

### T-E: TinyML monitor benchmark (summary)

- **Known.** QUTE (ICML 2025) [E56], SNAP-UQ (ICLR 2026) [E57], TCUQ (preprint) [E58] and UR2M [E59] all report microcontroller-level label-free uncertainty with self-reported gains. TCUQ and SNAP-UQ come from one group.
- **Gap.** No independent cross-comparison on common hardware and drift protocols was found.
- **Status.** Plausible. The contribution is replication or benchmarking, which is defensible but modest.

### T-F, T-G, T-H, T-I (summary)

- **T-F.** Quantization-aware-trained integer policies show more input-noise robustness [E20]. Quantization-aware training beats post-training quantization in RL [E22]. A public model card already reports friction and noise frontiers for Go1 policies at nine precisions [E26], and SQIL reports low-bit policies recovering under environment variation [E25]. **Gap largely closed; fails the novelty gate.**
- **T-G.** Risk control for early exits [E62], adaptive thresholds with guarantees under shift [E63], conformal risk-controlled distillation [E64], and drift-aware conformal risk control [E67]. **Gap closed by UAT; fails the gate.**
- **T-H.** Sensor-failure robustness [E68], masking pretraining [E69], and a survey of missing-modality learning [E70]. **Crowded; fails the gate.**
- **T-I.** Intermittent and multi-exit inference [E72, E73] and RL sampling for energy-harvesting nodes [E74]. Novelty was **not evaluated** (insufficient search of systems venues).

---

## Deliverable C: Detailed profiles of the top five

### C.1 T-A: Delay-consistent adaptive computation for real-time RL control

**Working title.** *When Compute Costs Time: Delay-Consistent Adaptive-Computation Policies for Real-Time Control on Resource-Constrained Hardware*

**Problem statement.** Robots and IoT controllers on microcontrollers or single-board computers pay for every inference twice: in energy, and in the delay between observing and acting. Methods that make policies cheaper (early exits [E01], quantization [E20, E22], compact or logic-circuit controllers [E21]) are evaluated in simulators where inference is instantaneous. Delayed-RL methods [E02, E04, E05] treat delay as given. Nobody, as far as this search shows, lets the policy choose its compute per state *and* pays for that choice in the dynamics.

**Research question.** When per-step inference time is coupled to control delay through a hardware-measured latency model, does a state-dependent adaptive-exit policy trained under delay-consistent dynamics outperform:

- (a) the best fixed-depth network trained with the same delay model, and
- (b) a FLOP-budgeted early-exit policy trained without delay coupling?

"Outperform" means a better return-versus-energy (or return-versus-deadline-miss) frontier.

**Hypotheses (falsifiable).**

- **H1.** Rankings of efficient policy architectures under zero-delay evaluation differ from rankings under measured-latency evaluation (Kendall's τ < 0.8) on at least one hardware tier and at least half the tasks.
- **H2.** On at least half the tasks, the delay-consistent adaptive policy improves IQM return at matched mean energy over the best fixed-depth delay-aware baseline, with a 95% stratified-bootstrap confidence interval excluding zero.
- **Failure condition.** If confidence intervals overlap on most tasks, or a small fixed network dominates, H2 is rejected. "Adaptive computation does not pay once delay is accounted for" is still a publishable, thesis-worthy result if H1 holds.

**Objectives.**

1. Build a latency- and energy-profiled simulation wrapper for 2-3 hardware tiers.
2. Run the H1 ranking study across baselines.
3. Develop and evaluate the delay-consistent adaptive-exit method (H2).
4. Validate on a hardware-in-the-loop setup in which the policy runs on the microcontroller and the simulator runs on a PC.

**Method and experimental plan.**

- *Environments.* Gymnasium MuJoCo (Hopper, Walker2d, HalfCheetah, Ant, optionally Humanoid). Add one pixel-based DeepMind Control task (for example cheetah-run with a CNN encoder), where compute is large relative to the control period. Optionally use MuJoCo Playground or Brax for GPU-parallel training.
- *Latency model.*
  - Profile each exit or variant on target tiers: a Cortex-M4/M7 class board (STM32), an ESP32-S3, and a Raspberry Pi 4 or 5; add a Jetson if available.
  - Record full latency distributions (jitter, tail), not means.
  - Map latency to delay through MuJoCo sub-steps, so delays can be fractions of a control step. Energy per inference comes from datasheet current × time, or a USB power meter.
- *Method.*
  - The actor is a multi-exit MLP or CNN.
  - A gate selects the exit given the state augmented with the action buffer (DCAC-style [E05]).
  - The environment applies the chosen exit's sampled latency before the action takes effect.
  - Reward is task reward minus λ × energy, or a constrained formulation with a budget (Lagrangian).
  - Stage 1 trains a post-hoc gate on a pretrained multi-exit actor. Stage 2 fine-tunes jointly.
- *Baselines.*
  - Fixed S/M/L networks trained (i) with zero delay, (ii) with delay randomization drawn from the measured distribution (Air Learning style [E06]), and (iii) with delay-aware augmentation (DCAC / real-time actor-critic [E04, E05]).
  - A BEXA-style FLOP-budgeted early-exit actor [E01] (released code if available, otherwise a reimplementation, labeled as such).
  - Variable-time-step SEAC [E11] and action-sequence SRL [E12].
  - INT8-quantized fixed networks [E20, E22], since quantization also reduces latency.
- *Variables.*
  - Independent: method, hardware tier (latency distribution), control frequency, λ or budget.
  - Dependent: return, energy per episode, mean and p99 latency, deadline misses, action jerk.
- *Ablations.* Remove delay-consistency during training; remove state augmentation; replace the learned gate with a confidence threshold; use deterministic instead of stochastic latency.
- *Robustness.* Run-time latency shift (the hardware is 1.5-2x slower than profiled, for example thermal throttling or contention), dynamics randomization, and a sweep of control frequencies.
- *Statistics.* At least 10 seeds per configuration; report IQM, optimality gap and performance profiles with rliable [E19]; Kendall's τ with bootstrap confidence intervals for H1; publish per-run results.

**Datasets, tools, compute, hardware.** No datasets; simulators only. Tools: Gymnasium/MuJoCo, Stable-Baselines3 or CleanRL, rliable, TFLite-Micro or CMSIS-NN for microcontroller profiling.

Compute estimate (assumption-based):

- State-based SAC at 1M steps takes roughly 2-6 CPU-core or GPU-hours per run.
- 5 tasks × about 9 methods × 10 seeds ≈ 450 runs ≈ 1,000-2,500 compute-hours, so a cluster or JAX-based simulators are needed.
- Pixel tasks cost roughly 3-5x more.

Hardware cost is about $50-$150 in boards; an existing Raspberry Pi is sufficient.

**Expected contribution and relevance.** A clear formulation and evaluation protocol that the efficient-RL community currently lacks. Practical guidance for anyone deploying learned controllers on microcontrollers, drones or micro-robots [E06, E08]. Direct skill transfer to robotics, ML systems and edge-AI roles.

**Feasibility and scope.** Core work is simulation plus cheap profiling, with no robot required. The main engineering risk is training the hybrid discrete-continuous gate, which the staged approach mitigates. 6-month scope: the H1 study plus a post-hoc gate on two tiers. 12-month scope: the full method, ablations, hardware-in-the-loop and the pixel task.

**Three major risks.**

| Risk | Probability | Impact | Evidence / reason | Mitigation | Early warning | Fallback |
|---|---|---|---|---|---|---|
| Novelty pre-empted (BEXA, DeeR-VLA or a 2026 paper already couples compute with delay) | Medium | High | Only abstracts read; ARLI and EXPO-FT show the latency topic is active [E16, E17] | Read the full texts in month 1; set alerts on papers citing E01 and E02; frame the contribution as evaluation protocol plus delay-consistent training | BEXA's experiments include a delay or latency wrapper | Narrow to the H1 ranking study, which stands alone, or pivot to T-D |
| Effect too small: inference is far below the control period, so adaptivity has nothing to trade | Medium | High | MLP policies are tiny. The effect is plausible only where latency approaches the control period [E07, E08]. | Choose regimes deliberately: microcontroller tiers, high control rates (200-1000 Hz), pixel encoders | Month-2 minimum experiment shows less than 5% return difference between zero and measured delay | Report the negative result, then shift to the pixel task or to energy as the primary objective |
| Unstable training of gate plus actor under delay | Medium | Medium | Hybrid action spaces and delays both destabilize off-policy RL [E05] | Staged training; DCAC-style augmentation; Lagrangian budget | Seed variance much larger than between-method differences | Fixed-schedule "anytime" exits with a simple confidence gate |

**Scoring breakdown.**

| Criterion | Score | Justification |
|---|---|---|
| Significance | 4 | Documented performance loss from onboard latency [E06-E09]; established control theory [E10] |
| Novelty | 4 | Distinct from each neighbor's abstract; targeted searches found no direct prior work. Not 5 because full texts are unread. |
| Feasibility | 4 | Simulation plus cheap boards; no data collection. Not 5 because of compute volume and gate-training risk. |
| Rigor | 4 | Strong baselines, many seeds, rliable, explicit failure conditions |
| Impact | 4 | Clear beneficiaries in embedded robotics and IoT control |
| Career | 5 | RL, real-time systems, edge deployment and robotics in one project |
| Environment | 3 | GPUs confirmed; RL or robotics supervision unconfirmed |
| **Total** | **80.0** | |

Confidence: medium.

**Next searches and experiments.**

1. Read BEXA [E01], Anokhin et al. [E02], Riemer et al. [E03], DeeR-VLA [E15] and ARLI [E16] in full.
2. Search Google Scholar for papers citing E01 and E02 since 2026.
3. Search the "anytime control" and "control-scheduling co-design" literature (IEEE TAC, RTSS, EMSOFT) for learning-based variants.
4. Profile a 3-exit MLP on an STM32 or ESP32 (one day of work).

### C.2 T-B: Instrument-in-the-loop feedback for LLM firmware agents

**Working title.** *Seeing the Signal: Does Instrument-Level Hardware Feedback Help LLM Agents Repair Embedded Firmware Behavior Failures?*

**Problem statement.** LLM agents increasingly write firmware, but code that compiles often fails on the device because of timing, peripheral configuration or protocol details [E43, E44]. Human embedded engineers debug with instruments such as logic analyzers and oscilloscopes. Agents in published benchmarks see compiler output, serial logs or documentation. Tool vendors are connecting agents to instruments [E46, E47] without independent evidence that it helps.

**Research question.** At a fixed iteration budget, does adding decoded instrument traces to an agent's feedback increase the behavior-correct repair rate on fault-seeded firmware tasks, compared with compiler-plus-serial feedback? Which fault classes benefit?

**Hypothesis.** Trace feedback raises behavior-correct@k by at least 10 percentage points on timing and protocol faults, with no significant gain on pure-logic faults. Rejected if the gain is non-significant under a mixed-effects logistic model, or the same across fault classes.

**Objectives.**

1. Build a low-cost, automated HIL harness: dev board plus sigrok-compatible logic analyzer plus spec-checking oracle.
2. Build a fault-seeded task suite of 40-60 tasks across 2-3 platforms, reusing IoT-SkillsBench or EmbedBench tasks if their licenses allow.
3. Run the controlled comparison across feedback conditions and models.
4. Produce a failure taxonomy and cost analysis.

**Method.**

- Conditions:
  - A: compiler errors only.
  - B: A plus serial log.
  - C: B plus a raw trace excerpt.
  - D: B plus a decoded and summarized trace (protocol decode plus timing violations against the spec).
  - E: D plus skills documents (IoT-SkillsBench style [E43]).
- Models: two API models plus one or two open-weight models run locally on the lab GPUs. Pin model versions.
- At least 5 repeats per task-condition-model.
- Oracle: automated trace assertions (bus transactions, timing windows), audited manually on a random 10% sample.
- Analysis: mixed-effects logistic regression with random intercepts per task; report effect sizes with confidence intervals; cost in tokens and dollars; iterations to fix.

**Data, tools, hardware.**

- Boards: ESP32-S3, nRF52840 or RP2040, plus an STM32. Peripherals: I2C sensor, SPI display, PWM servo, UART device.
- Two or three 8-channel 24 MHz analyzers (about $10-$20 each, sigrok/PulseView), or one Saleae if the lab owns one. About $150-$300 in total.
- API budget assumption: 5 conditions × 50 tasks × 5 repeats × 3 models × about 6 iterations ≈ 22k agent turns. Order of $300-$1,500 depending on the models. Open-weight models reduce this.

**Baselines and metrics.** Conditions A, B and E are the baselines; E corresponds to the IoT-SkillsBench state of the art. Metrics: behavior-correct@k, iterations to fix, cost, oracle precision and recall from the audit, and the per-fault-class effect.

**Contribution and relevance.** Evidence for a design decision tool vendors are already making; an open benchmark harness. Strong AI-engineering and tooling career value, and it uses your embedded and IoT background.

**Feasibility.** High. Cheap hardware, no ethics approval, a minimum experiment possible in month 2. The main engineering effort is a reliable automated oracle.

**Three major risks.**

| Risk | Probability | Impact | Mitigation | Early warning | Fallback |
|---|---|---|---|---|---|
| Pre-empted by academic or vendor evaluations | Medium-high | Medium | Fault-class analysis and open harness as the defensible core; publish a preprint early | A paper reporting "trace feedback" ablations | Pivot to *when* agents should request measurements (active probing) |
| Flaky hardware oracle (false passes or fails) | Medium | High | Assertion-based checks, repeated captures, manual audit | Audit disagreement above 5% | Restrict to protocols with deterministic decoders (I2C, SPI, UART) |
| Model churn and nondeterminism undermine conclusions | High | Medium | Pinned versions, open-weight models, repeats, report variance | Large run-to-run variance | Emphasize the open-model results and the harness contribution |

**Scoring breakdown.**

| Criterion | Score | Justification |
|---|---|---|
| Significance | 4 | Documented compile-versus-behavior gap [E43, E44] |
| Novelty | 3 | Plausible gap, one search family, fast-moving field |
| Feasibility | 4 | Cheap hardware; oracle engineering is the main risk |
| Rigor | 3 | LLM nondeterminism and model churn limit how durable the conclusions are |
| Impact | 4 | Developer tooling with an obvious adoption pathway |
| Career | 4 | AI engineering and agents; less RL or robotics |
| Environment | 3 | Needs software-engineering or embedded supervision (unconfirmed) |
| **Total** | **71.0** | |

Confidence: medium-low.

**Next steps.** Read IoT-SkillsBench [E43] and EmbedBench [E44] in full. Search ICSE/FSE/ASE 2026, EMSOFT and arXiv cs.SE for "hardware-in-the-loop" plus "agent". Check whether the IoT-SkillsBench tasks and harness are released.

### C.3 T-C: Perturbation robustness of compressed VLA policies

**Working title.** *Does Compression Amplify Fragility? A Paired Evaluation of Quantized and Pruned Vision-Language-Action Policies under Controlled Perturbations*

**Problem statement.** Compression makes VLAs deployable on edge GPUs. Compression papers report nominal success [E29-E35], while VLAs are known to be brittle to camera, initial-state and lighting perturbations [E27, E28]. Deployment decisions need to know whether compression costs more under perturbation than on nominal tasks.

**Research question.** For a given VLA, is the success lost to compression larger under LIBERO-Plus perturbations than on nominal LIBERO, and on which perturbation axes?

**Hypothesis.** A negative interaction for activation-quantized (W4A4, W8A8) models on camera and lighting axes; neutral or positive for token pruning, consistent with [E38]. Rejected if the per-axis interaction confidence intervals include zero across axes.

**Method.**

- Models: OpenVLA-OFT, π0 or π0.5 (openpi), and a small VLA such as SmolVLA to reduce cost.
- Compression: round-to-nearest, GPTQ and AWQ at W8A8, W4A16 and W4A4, using the VLAQuantBench recipes [E29] where code exists; token pruning (VLA-Pruner [E41], FastV); plus a combined condition [E42].
- Evaluation: paired episodes (same seeds and initial states) on a stratified LIBERO-Plus subsample (for example 100 tasks per axis).
- Statistics: per-axis McNemar tests; mixed-effects logistic regression on compression × axis.
- Diagnostic: does nominal action deviation or KL predict the perturbed drop?

**Compute (assumption).** About 20-40k episodes at 10-60 s each ≈ 300-600 GPU-hours on A100-class hardware. Feasible only with confirmed GPU access.

**Risks.**

1. Pre-emption. High probability and high impact: VLAQuantBench or Bouguerra-style follow-ups [E29, E37]. Mitigate by reading VLAQuantBench in full in week 1.
2. Benchmark and reproduction friction with VLA codebases. Medium probability.
3. GPU queue limits. Medium probability.

**Fallback.** A single-model, single-method study with the diagnostic as the contribution.

**Scores.** 4/3/3/4/4/5/3, total 72.0. Confidence: medium-low.

**Next steps.**

- Read the full texts of VLAQuantBench and LIBERO-Plus.
- Search for "LIBERO-Plus" together with "quantiz" in papers citing [E27].

### C.4 T-D: State-dependent planning budgets for TD-MPC2

**Working title.** *When to Think Harder: State-Dependent Planning Budgets for Latent World-Model MPC under Real-Time Constraints*

**Research question.** Can a cheap, state-dependent signal choose the number of MPPI iterations so that TD-MPC2 keeps its fixed-budget return while cutting average and p99 planning compute or latency? The candidate signals are Q-ensemble disagreement, relative-improvement stopping, policy-prior/planner disagreement, and a learned gate.

**Hypothesis.** At least one signal matches the full-budget IQM return within a 5% confidence bound at 40% or lower mean iterations on at least half the DMControl tasks.

**Method.**

- Use released TD-MPC2 checkpoints [E50], so no world-model training is needed for the core study. This is a major feasibility advantage.
- Baselines: fixed budgets of 0-6 iterations; AOP-style early stopping [E52]; uncertainty-triggered allocation (tests whether the "uncertainty fails" result [E54] carries over to compute allocation); a learned metacontroller [E51].
- Report return versus compute and latency frontiers, and plug in T-A's latency model.

**Risks.**

1. Savings are trivial because TD-MPC already tolerates a 50% cut [E49].
2. Partial pre-emption by older metacontrol and AOP work [E51, E52].
3. A learned gate overfits per task.

**Fallback.** An empirical characterization of where planning helps.

**Scores.** 3/3/4/4/3/4/3, total 69.0. Confidence: medium.

**Next steps.**

- Check the TD-MPC2 checkpoint licenses.
- Search for "early termination MPPI learned dynamics 2025-2026" and for TD-MPC2 follow-ups such as Dream-MPC.

### C.5 T-E: Independent benchmark of label-free TinyML uncertainty monitors

**Working title.** *Do Label-Free Uncertainty Monitors for TinyML Hold Up? An Independent, Hardware-Grounded Benchmark under Realistic Drift*

**Research question.** Under common devices, tasks and drift protocols, do QUTE [E56], SNAP-UQ [E57], TCUQ [E58] and simple baselines (max-softmax, entropy, early-exit agreement) rank as their papers report on:

- accuracy-drop detection (AUROC, detection delay), and
- resource cost (flash, RAM, latency, energy)?

**Method.**

- Tasks: MLPerf-Tiny-style workloads (keyword spotting, visual wake words, anomaly detection, image classification).
- Drift: corruptions plus temporally gradual drift.
- Devices: STM32H7, ESP32-S3, nRF52.
- At least 5 seeds; pre-registered protocol.

**Risks.**

1. Code for TCUQ and SNAP-UQ unavailable, so reimplementation fidelity is uncertain.
2. Reviewers view a benchmark-only contribution as incremental.
3. Results simply confirm the papers. That is still useful, but carries a low publication ceiling.

**Fallback.** A two-method head-to-head with energy measurement.

**Scores.** 3/3/4/4/3/3/3, total 67.0. Confidence: medium.

**Next steps.** Check the repositories for each method. Search for any independent replication published after 2025.

---

## Deliverable D: Roadmaps for the two highest-ranked candidates

### D.1 T-A roadmap (12 months; a compressed 6-month version follows)

| Month | Work | Milestone / exit criterion |
|---|---|---|
| 1 | Full-text read of E01-E05, E15-E17; novelty memo for the supervisor; set up the simulator wrapper; buy or borrow 2 boards | Supervisor signs off the novelty memo; latency tables for 3 network sizes on 2 tiers |
| 2 | **Minimum viable experiment:** fixed S/M/L SAC on Hopper and Walker2d, 5 seeds, zero versus measured delay | **Decision point D1** (below) |
| 3 | Multi-exit actor; post-hoc gate; BEXA-style baseline | Gate beats a confidence threshold on 1 task |
| 4 | Delay-consistent joint training; DCAC augmentation; SEAC and SRL baselines | All baselines reproduce published returns within 10% at zero delay |
| 5 | Main grid: 5 tasks × methods × 10 seeds | Complete per-run results archived |
| 6 | Ablations and robustness (latency shift, jitter, dynamics) | **Decision point D2:** H1 and H2 decided with confidence intervals; workshop-paper draft |
| 7 | Hardware-in-the-loop validation: policy on the microcontroller, MuJoCo on a PC, over serial or USB | Simulation-versus-HIL return gap quantified |
| 8 | Extension: pixel task or INT8 exits (links T-F) | One extension complete |
| 9 | Conference paper (venue depends on timing: CoRL, ICRA, L4DC, or an RLC/NeurIPS workshop) | Submitted |
| 10-11 | Thesis writing; reviewer-driven experiments | Full draft |
| 12 | Revision, buffer, defense | Submitted thesis |

**Decision point D1 (end of month 2).**

- *Continue* if, on at least one hardware tier, compute-induced delay changes IQM return by at least 10%, or changes the size ranking, on at least 2 of 3 tasks.
- *Narrow* if the effect appears only on microcontroller tiers or at high control rates. Restrict the thesis to that regime.
- *Abandon or pivot* if no tier shows an effect, even at 500 Hz and on the pixel task. Write the H1 negative result as a short paper and move to T-D (same infrastructure).

**Compressed 6-month version.**

- Month 1: novelty memo and profiling.
- Month 2: minimum experiment and D1.
- Months 3-4: post-hoc gate and main grid (3 tasks).
- Month 5: ablations and a light HIL demonstration.
- Month 6: writing.

Drop joint training and the pixel extension.

**Thesis structure.**

1. Introduction
2. Background: control timing [E10], delayed and real-time MDPs [E02-E05], dynamic networks and efficient RL [E01, E20, E22]
3. Problem formulation: the compute-delay-coupled MDP
4. Latency and energy profiling methodology
5. Method: delay-consistent adaptive exits
6. Experiments (ranking study, adaptive versus fixed, ablations and robustness, HIL)
7. Discussion, limitations and threats to validity
8. Conclusion and future work

### D.2 T-B roadmap (12 months; a compressed 6-month version follows)

| Month | Work | Milestone / exit criterion |
|---|---|---|
| 1 | Full read of E43-E45; vendor scan; hardware purchase; sigrok capture-and-decode pipeline | Automated capture plus I2C, SPI and UART decode working on 1 board |
| 2 | **Minimum viable experiment:** 10 fault-seeded tasks on 1 board; conditions B and D; 1 API model and 1 open model; 5 repeats | **Decision point D1** (below) |
| 3-4 | Expand to 40-60 tasks on 2-3 platforms; fault taxonomy; oracle audit protocol | Oracle precision and recall above 95% on the audit sample |
| 5-6 | Main experiment: 5 conditions × 3 models × tasks × 5 repeats | Complete logs; cost ledger |
| 7 | Mixed-effects analysis; ablations (raw versus summarized traces, window length, skills interplay) | **Decision point D2:** hypothesis decided |
| 8 | Paper (ICSE, FSE or ASE; EMSOFT; MSR data track for the benchmark); open-source the harness | Submitted |
| 9-10 | Extension: agent-initiated measurement (when to probe) | Prototype plus pilot results |
| 11-12 | Thesis writing and defense | Submitted thesis |

**Decision point D1 (end of month 2).**

- *Continue* if the oracle audit disagreement is 5% or less and condition D differs from condition B by at least 10 percentage points on timing or protocol faults.
- *Narrow* to protocol faults only if the effect is confined there.
- *Abandon* the comparison framing if there is no effect and the oracle is unreliable. Fall back to a harness-and-benchmark contribution, or move to T-A.

**Compressed 6-month version.** Month 1 pipeline; month 2 minimum experiment; months 3-4 a 30-task suite and main runs with 2 models; month 5 analysis; month 6 writing.

**Thesis structure.**

1. Introduction
2. Background: LLM code generation, embedded development, HIL testing
3. Harness and task-suite design
4. Experimental design and fault taxonomy
5. Results
6. Threats to validity (model churn, oracle reliability)
7. Conclusion

---

## Risk register for the other shortlisted topics

Three largest risks each, as probability / impact.

**T-C**

| Risk | Prob / impact | Evidence | Mitigation | Early warning | Fallback |
|---|---|---|---|---|---|
| Pre-emption | High / high | [E29, E37] | Week-1 full read | VLAQuantBench full text includes perturbation suites | Diagnostic-only paper |
| GPU budget | Medium / high | Volume estimate | Stratified subsample, small VLA | Queue wait over 1 week | Single model |
| Reproduction friction | Medium / medium | Many codebases | Start from openpi and LeRobot integrations [E27] | Nominal LIBERO more than 5 pp below published | Use published checkpoints only |

**T-D**

| Risk | Prob / impact | Evidence | Mitigation | Early warning | Fallback |
|---|---|---|---|---|---|
| Savings trivial | Medium / high | [E49] | Choose hard, high-dimensional tasks | Fixed 1-iteration budget matches 6 iterations | Characterization study |
| Prior art | Medium / medium | [E51, E52] | Frame as a modern latent-MPC revisit plus latency | — | Merge into T-A |
| Gate overfits | Medium / low | — | Cross-task evaluation | — | Heuristic signals only |

**T-E**

| Risk | Prob / impact | Evidence | Mitigation | Early warning | Fallback |
|---|---|---|---|---|---|
| Missing code | Medium / high | Unknown availability | Contact the authors in month 1 | No reply in 3 weeks | Reimplement and flag fidelity |
| Seen as incremental | High / medium | — | Pre-registration plus energy measurement adds value | — | Workshop venue |
| Hardware variance | Low / medium | — | Fixed clocks and power meter | — | — |

---

## 9. Final recommendation

**1. Which topic first, and why?**
**T-A, delay-consistent adaptive computation for real-time RL control.** It has the best combination of defensible novelty and feasibility (4/4). Its core question, whether saving compute still pays once compute costs time, has a clean falsification test in month 2. It also fits your profile closely: RL, edge hardware, robotics and systems.

**2. Strongest evidence that the problem is real and unresolved.**

- Measured onboard latency degrades learned control: Air Learning on a Raspberry Pi [E06]; OmniReset's jitter at 60 ms [E07]; more than 50 ms of transport delay on a low-cost quadruped [E09]; microcontroller compute capping update rates [E08].
- The leading early-exit RL paper reports FLOP and wall-clock savings evaluated on MuJoCo [E01].
- Real-time-RL authors note that state-adaptive compute time is uncommon [E03].
- The two literatures do not meet in anything found.

**3. What would make it novel rather than incremental.** Coupling the compute choice to the dynamics: the delay the policy experiences must come from the exit it selects, using hardware-measured latency distributions, and the policy is trained under that coupling. Bolting an early exit onto SAC (done [E01]) or adding a fixed delay wrapper (done [E04, E05]) is incremental. The H1 ranking result ("conclusions about efficient policies change under measured latency") is a second, independent contribution.

**4. Smallest experiment that tests feasibility.** Train fixed small, medium and large SAC actors on Hopper and Walker2d, 5 seeds each. Profile them on one microcontroller and one Raspberry Pi. Evaluate with zero delay and with the measured latency distribution. About 2-3 weeks of work. If no configuration changes return by 10% or more, or changes the ranking, the premise is weak (see Decision point D1).

**5. Evidence that would make us reject the topic.**

- The full text of BEXA [E01], DeeR-VLA [E15] or a newer paper already trains adaptive compute under compute-induced delay.
- The minimum experiment shows that delay effects are negligible in every realistic regime, including high control rates and pixel inputs.
- The gate cannot be trained stably even in the post-hoc setting.

**6. What to confirm before committing.**

- A supervisor or co-supervisor comfortable with deep RL. Control-systems or real-time-systems expertise is a strong bonus.
- Access to 2-3 embedded boards and a basic power meter.
- Cluster time for about 1,000-2,500 compute-hours, or JAX simulators.
- Whether the department accepts a thesis where the main result could be negative (H2 rejected, H1 supported).
- Whether co-authoring with the supervisor on a workshop paper at month 6 is expected.

**7. Alternative if T-A fails its checks.**

- **If T-A fails the novelty check, pursue T-B.** It is ranked second and its risks do not depend on T-A's. A novelty failure in T-A would probably also weaken T-D, which shares T-A's framing.
- **If T-A fails only on effect size** (Decision point D1), pivot within the same infrastructure to **T-D**. TD-MPC2 planning has an inherently large and variable compute cost, so the compute-latency trade-off is guaranteed to be non-trivial there.

**Overall confidence: medium.** The ranking of T-A first is robust to a one-point novelty drop. The relative order of T-B, T-C and T-D is within noise and should be revisited after the month-1 full-text reads and the supervisor conversation.
