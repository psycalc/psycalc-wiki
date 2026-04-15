# Consolidated Action Plan: Cognitive Matchmaker

**Last Updated:** 2026-04-15
**Status:** Research Complete — Ready for Implementation

---

## Executive Summary

Build a simulation-based dating concierge that:
1. Creates digital twins from personality assessments (BFI-2 validated)
2. Runs adversarial scenarios (Hang the DJ style)
3. Scores compatibility via repeated choice metric
4. Validates against real-world outcomes

**Core Principle:** AI as Copilot, not Oracle — human validates, AI suggests

**MVP Cost:** $0.56 - $2.62 for 100 scenarios
**Research Foundation:** CogniPair + Love First + Psychology Research

---

## Part 1: What We Have

### AI/Dating Research

| Paper | Key Finding | Confidence |
|-------|-------------|------------|
| CogniPair (ICLR 2026) | GNWT-Agent achieves 72% correlation with human attraction | High |
| Love First, Know Later (NeurIPS 2025) | Simulation > Profile matching for compatibility | High |
| Pairadigm (2026) | Bradley-Terry for pairwise comparison | Medium |

### Psychology Research (NEW)

| Paper | Key Finding | Application |
|-------|-------------|-------------|
| Nature Scientific Reports (2025) | GPT-4 emulates personality with high internal consistency | Persona quality |
| arXiv: LLM Psych Simulators | 300-500 words per persona — minimum for valid simulation | Persona depth |
| arXiv: Psychometric Approach | BFI-2 prompts > simple adjectives for persona | Persona method |
| Frontiers: Psypilot (2026) | AI as copilot, not oracle — human validates | Positioning |
| arXiv: S-Researcher (2026) | 100K agents, human-in-the-loop at every stage | Architecture |
| Current Psychology (2025) | AI for hypothesis → validate with humans | Validation method |

### Key Insights from Psychology Research

1. **BFI-2 > Simple Prompts** — Use validated personality scales, not just adjectives
2. **300-500 words minimum** — More detail = better simulation (scaling law)
3. **AI Inflates Moral Ratings** — Need calibration against human baseline
4. **Consistency Test Required** — Pre/post personality assessment to verify stability
5. **Copilot > Oracle** — Position as suggestion system, not decision-maker
6. **AI for Hypothesis → Humans for Validation** — Pilot with AI, verify with real pairs

---

## Part 2: MVP Scope

### What to Build (MVP-1)

```
┌─────────────────────────────────────────────────────────────┐
│                     MVP-1: Hang the DJ Test                  │
│                                                              │
│  Core Principle: AI as Copilot, not Oracle                   │
│                                                              │
│  Pipeline:                                                   │
│  1. BFI-2 Assessment → Detailed Persona (300-500 words)      │
│  2. Pre-simulation Consistency Test                          │
│  3. 100 Adversarial Scenarios                               │
│  4. Post-simulation Consistency Test                         │
│  5. Repeated Choice Scoring (99.8%)                         │
│  6. Human Validation of Output                               │
│                                                              │
│  Output: "Hypothesis: 99.8% compatibility — verify in real"  │
└─────────────────────────────────────────────────────────────┘
```

### What NOT to Build (Yet)

- Fully autonomous matching
- AI making decisions without human review
- Mass dating pool integration
- Automatic messaging
- Calendar booking

---

## Part 3: Skills (12 Total)

```
skills/
├── persona-generator.md        # BFI-2 based, 300-500 words
├── persona-cloner.md          # GNWT weights from Big Five
├── persona-validator.md      # Pre/post consistency test ⭐ NEW
├── global-workspace.md         # GNWT broadcast mechanism
├── simulation-runner.md        # 3-phase pipeline
├── observer-agent.md          # External LLM analysis
├── adversarial-designer.md     # Pressure scenarios
├── reward-model.md            # Bradley-Terry + IRL
├── choice-tracker.md           # Repeated choices
├── memory-persister.md         # 4-layer memory
├── compatibility-scorer.md      # 99.8% score
└── explanation-generator.md     # "Hypothesis" output
```

---

## Part 4: Persona Generation — Research-Backed Method

### The Problem

Simple prompts like "You are an introverted person" produce inconsistent, shallow personas.

### The Solution: BFI-2 Based Approach

Based on Huang et al. (2025) — Psychometric Approach:

```python
# Input: BFI-2 scores
persona_input = {
    "openness": {"score": 0.7, "items": [...]},
    "conscientiousness": {"score": 0.8, "items": [...]},
    "extraversion": {"score": 0.3, "items": [...]},
    "agreeableness": {"score": 0.6, "items": [...]},
    "neuroticism": {"score": 0.2, "items": [...]},
    "demographics": {"age": 28, "gender": "female", "occupation": "engineer"}
}

# Output: 300-500 word persona narrative
persona_narrative = generate_persona(persona_input)
```

### Validation Chain

```
BFI-2 Assessment → Prompt Engineering → LLM Persona → Validation

Step 1: BFI-2 gives validated personality dimensions
Step 2: Prompt includes all 5 dimensions + demographics
Step 3: Generate 300-500 word narrative
Step 4: Validate with user: "Does this describe you?" (target: 5.6/7.0)
```

### Critical Rules

1. **Minimum 300 words** — Below this, simulation quality degrades
2. **Include demographics** — Age, gender, occupation improve validity
3. **Use all 5 dimensions** — Don't skip neuroticism or agreeableness
4. **Validate output** — User rates persona accuracy

---

## Part 5: Persona Consistency Testing

### The Problem

LLM personas can drift — starting as one type, ending as another.

Source: NeurIPS 2025 — "reducing inconsistency by 55%"

### The Solution: Pre/Post Assessment

```python
def consistency_test(persona_narrative):
    # Pre-simulation: Test persona stability
    pre_assessment = assess_personality(persona_narrative)
    
    # Run 10 interactions
    interactions = run_test_conversations(persona_narrative, n=10)
    
    # Post-simulation: Did persona drift?
    post_assessment = assess_personality(persona_narrative)
    
    # Calculate drift
    drift = calculate_personality_distance(pre_assessment, post_assessment)
    
    # If drift > threshold, regenerate persona
    if drift > 0.2:
        return regenerate_persona(persona_narrative)
    
    return persona_narrative, drift
```

### Validation Criteria

| Drift Score | Action |
|-------------|--------|
| < 0.1 | Excellent — persona stable |
| 0.1 - 0.2 | Acceptable — note in output |
| > 0.2 | Regenerate persona |

---

## Part 6: Validation Methodology

### The Problem

AI simulation ≠ real-world outcomes. Need systematic validation.

### The Solution: AI for Hypothesis → Humans for Validation

Based on Current Psychology (2025) and S-Researcher (2026):

```
┌─────────────────────────────────────────────────────────────┐
│  Validation Pipeline                                          │
│                                                              │
│  Phase 1: AI Simulation (cheap, fast)                       │
│  ├── Run 100 scenarios per pair                             │
│  ├── Calculate repeated-choice score                         │
│  └── Output: "Hypothesis: 87% compatible"                   │
│                                                              │
│  Phase 2: Human Pilot (expensive, slow)                      │
│  ├── Recruit 10-20 real pairs                              │
│  ├── Collect actual dates                                    │
│  ├── Compare simulation to reality                           │
│  └── Calibrate prediction model                             │
│                                                              │
│  Phase 3: Ongoing (continuous improvement)                  │
│  ├── Collect feedback after each real date                   │
│  ├── Retrain/adjust scoring weights                         │
│  └── Improve future predictions                              │
└─────────────────────────────────────────────────────────────┘
```

### Ablation Tests

| Test | What It Measures |
|------|----------------|
| Profile matching only | Baseline (current dating apps) |
| + Typology features | Socionics contribution |
| + Simulation | Simulation contribution |
| Full pipeline | Total accuracy |

Target: Simulation adds +15% over profile matching

---

## Part 7: Implementation Roadmap

### Phase 0: Foundation (Week 1)

**Goal:** Technical stack ready

| Task | Deliverable |
|------|-------------|
| Setup Python project | `poetry init`, dependencies |
| LLM abstraction | `llm.py` with model switching |
| BFI-2 schema | `schemas/persona.py` with Big Five |
| Assessment form | BFI-2 questionnaire integration |
| Unit tests | Basic test suite |

**Cost:** $0
**Time:** 4-8h

### Phase 1: Persona Pipeline (Week 2)

**Goal:** BFI-2 → Validated persona

| Task | Deliverable | Research Source |
|------|-------------|----------------|
| BFI-2 to prompt | Structured → prompt | Huang et al. |
| Persona generator | 300-500 word narrative | LLM Psych Simulators |
| Consistency test | Pre/post assessment | NeurIPS 2025 |
| User validation | "Rate 1-7" interface | CogniPair |
| Socionics mapping | MBTI → Socionics | Type Mapper |

**Cost:** ~$0.10
**Time:** 8-16h

### Phase 2: Simulation (Week 3)

**Goal:** Run multi-turn conversations

| Task | Deliverable |
|------|-------------|
| Turn loop | 10-turn conversation |
| Memory injection | Cross-turn context |
| Parallel execution | Batch processing |
| Transcript logging | JSONL storage |

**Cost:** ~$2.00 (100 conversations)
**Time:** 16-24h

### Phase 3: Adversarial Scenarios (Week 4)

**Goal:** Test under pressure

| Task | Deliverable |
|------|-------------|
| Scenario designer | Forced separation, better option, distance |
| Stress injection | Escalating pressure |
| Choice tracking | accept/reject, seek_reunion, stay_loyal |
| Rebellion score | Did they resist system? |

**Cost:** ~$0.50
**Time:** 8-16h

### Phase 4: Scoring & Output (Week 5)

**Goal:** Hypothesis, not oracle

| Task | Deliverable |
|------|-------------|
| Repeated choice metric | 99.8% = 998/1000 |
| Observer analysis | External compatibility assessment |
| Calibration | Adjust for AI inflation |
| Explanation | "Hypothesis: likely compatible — verify in real" |

**Key:** Output as hypothesis, not verdict

**Cost:** ~$0.10
**Time:** 8-16h

### Phase 5: Human Validation (Week 6-8)

**Goal:** Verify simulation predicts reality

| Task | Method | Deliverable |
|------|--------|-------------|
| Recruit pairs | Partner pool | 10-20 real pairs |
| Run simulation | Full pipeline | Hypotheses |
| Collect dates | Real-world | Ground truth |
| Compare | Correlation | Score calibration |

**Cost:** ~$5.00 + recruitment effort
**Time:** 40-60h

---

## Part 8: Technical Architecture

### Stack

```
┌─────────────────────────────────────────────────────────────┐
│                      Frontend (Streamlit MVP)                │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      API Layer (FastAPI)                     │
│                                                              │
│  POST /assessments/bfi2     → BFI-2 questionnaire          │
│  POST /personas             → Generate validated persona  │
│  POST /simulate             → Run 100 scenarios          │
│  GET  /compatibility        → Hypothesis + explanation     │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Simulation Engine                          │
│                                                              │
│  bfi2_to_persona()      (BFI-2 → 300-500 word narrative)│
│  consistency_test()       (Pre/post personality stability)   │
│  simulation_runner()       (100 adversarial scenarios)       │
│  choice_tracker()         (Repeated choice metric)           │
│  observer_agent()        (External analysis)                │
│  compatibility_scorer()   (Calibrated score)               │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                       Storage (SQLite MVP)                   │
│                                                              │
│  assessments/          BFI-2 responses                     │
│  personas/              Generated + validated personas      │
│  transcripts/            Full simulation logs               │
│  outcomes/               Real-world validation data         │
└─────────────────────────────────────────────────────────────┘
```

### LLM Parameters (Research-Backed)

| Task | Model | Temperature | Source |
|------|-------|-------------|--------|
| Persona Generation | Gemini 2.5 Flash Lite | 0.8 | Love First |
| Consistency Test | gpt-4o-mini | 0.3 | Cheap + reliable |
| Conversation | Mistral-Nemo | 0.6 | Love First |
| Observer | gpt-4o | 0.3 | Low variance |
| Calibration | gpt-4o | 0.2 | Precise |

---

## Part 9: Success Metrics

### Simulation Quality

| Metric | Target | How to Measure |
|--------|--------|----------------|
| Persona accuracy | ≥5.6/7.0 | User rating |
| Consistency drift | <0.2 | Pre/post assessment |
| Token cost per pair | <$0.10 | API logs |

### Prediction Accuracy

| Metric | Target | Source |
|--------|--------|--------|
| Simulation vs real | r > 0.6 | Pilot data |
| vs Profile matching | +15% improvement | Ablation |
| vs Random | +30% improvement | Baseline |

### User Understanding

| Metric | Target | How to Measure |
|--------|--------|----------------|
| Comprehension | >80% | Survey |
| Trust in "hypothesis" framing | >70% | Survey |
| Actionable insight | >60% | Survey |

---

## Part 10: Key Principles (Research-Backed)

### 1. Copilot, Not Oracle
> "AI as workflow-embedded decision support rather than autonomous care" — Frontiers: Psypilot

**Implementation:** Every output includes "Verify in real life"

### 2. BFI-2 for Persona
> "BFI-2 prompts produce more human-like responses than simple adjectives" — Huang et al.

**Implementation:** Use validated personality scales, not just type labels

### 3. 300-500 Word Minimum
> "More detailed persona profile = better simulation" — arXiv: Scaling Law

**Implementation:** Enforce minimum length, reject shorter

### 4. Consistency Testing
> "LLM personas drift during simulation" — NeurIPS 2025

**Implementation:** Pre/post personality assessment required

### 5. Calibrate for Inflation
> "AI tends to inflate moral ratings" — Huang et al.

**Implementation:** Compare AI scores to human baseline, adjust

### 6. AI for Hypothesis → Humans for Validation
> "LLMs can serve as useful tools for preliminary research" — Nature Scientific Reports

**Implementation:** Pilot with AI, verify with real pairs

---

## Part 11: Risk Mitigation

| Risk | Mitigation |
|------|------------|
| LLM persona drift | Pre/post consistency test |
| Inflated compatibility scores | Calibration against human baseline |
| User over-trusts AI | "Hypothesis" framing, not "Verdict" |
| Simulation ≠ Reality | Mandatory human validation phase |
| Bias in scoring | Ablation tests, diverse pilot pairs |

---

## Part 12: File Structure

```
cognitive-matchmaker/
├── pyproject.toml
├── src/
│   ├── __init__.py
│   ├── llm/
│   │   ├── client.py           # Model abstraction
│   │   └── prompts.py          # BFI-2 based prompts
│   ├── skills/
│   │   ├── persona_generator.py # BFI-2 → narrative
│   │   ├── persona_validator.py  # Consistency test ⭐
│   │   ├── simulation_runner.py # Multi-turn
│   │   ├── observer_agent.py   # External analysis
│   │   ├── choice_tracker.py    # Repeated choice
│   │   └── compatibility_scorer.py # Calibrated score
│   ├── schemas/
│   │   ├── bfi2.py             # BFI-2 assessment
│   │   ├── persona.py          # Persona + consistency
│   │   └── assessment.py       # Final output
│   └── api/
│       └── routes.py
├── data/
│   ├── personas/               # Generated personas
│   ├── transcripts/            # Simulation logs
│   └── outcomes/               # Validation data
└── tests/
```

---

## Appendix: Research Sources

| Source | Key Contribution |
|--------|------------------|
| CogniPair (ICLR 2026) | GNWT-Agent, 72% correlation |
| Love First (NeurIPS 2025) | 3-phase pipeline, reward modeling |
| Nature Scientific Reports | GPT-4 personality emulation |
| arXiv: LLM Psych Simulators | 300-500 word minimum |
| arXiv: Psychometric Approach | BFI-2 for AI agents |
| Frontiers: Psypilot | Copilot, not oracle |
| arXiv: S-Researcher | Human-in-the-loop |
| Current Psychology | AI for hypothesis → validate with humans |

---

## Next Steps

1. [ ] Review this plan
2. [ ] Decide: Build solo or find team?
3. [ ] Clone/start repo
4. [ ] Week 1: Setup technical stack
