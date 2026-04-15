# MVP Cost Estimation

## MVP Scope

```
persona-generator + persona-cloner + simulation-runner + choice-tracker
```

**Test case:** 2 personas, 100 scenarios (for proof-of-concept)

---

## LLM Cost Reference (April 2026)

| Model | Input | Output | Notes |
|-------|-------|--------|-------|
| **gpt-4o** | $2.50/1M | $10/1M | Main simulation model |
| **gpt-4o-mini** | $0.15/1M | $0.60/1M | Observer, lighter tasks |
| **gemini-2.5-flash-lite** | $0.075/1M | $0.30/1M | Persona generation |
| **mistral-nemo** | $0.20/1M | $0.20/1M | Conversation simulation |
| **claude-sonnet-4.6** | $3/1M | $15/1M | Premium option |

---

## Cost Breakdown by Skill

### 1. persona-generator

```
Input: 2 users × structured assessment
Output: 2 × 400-word narratives

Per persona:
  - Prompt: ~500 tokens (structured data)
  - Response: ~500 tokens (narrative)

Cost per persona:
  Gemini 2.5 Flash Lite
  Input: 500/1M × $0.075 = $0.0000375
  Output: 500/1M × $0.30 = $0.00015
  Total: $0.00019 per persona

Total: 2 × $0.00019 = $0.00038
```

### 2. persona-cloner (GNWT weight calculation)

```
Input: 2 narratives × ~500 tokens
Processing: Extract traits, calculate module weights
Output: GNWT agent config

No LLM call needed - deterministic transformation
(Or optional: $0.00001 if using small model)

Total: $0.00
```

### 3. simulation-runner

```
Per conversation:
  - 2 agents × 10 turns × conversation
  - Each turn: ~50 input tokens (context) + ~100 output tokens (response)

Turn calculation:
  Input per agent per turn: ~150 tokens (query + history)
  Output per agent per turn: ~80 tokens

Per turn (both agents):
  Input: 2 × 150 = 300 tokens
  Output: 2 × 80 = 160 tokens

Per conversation (10 turns):
  Input: 10 × 300 = 3,000 tokens
  Output: 10 × 160 = 1,600 tokens

Using gpt-4o (temp=0.9):
  Input: 3,000/1M × $2.50 = $0.0075
  Output: 1,600/1M × $10 = $0.016
  Per conversation: $0.0235

Total for 100 scenarios: 100 × $0.0235 = $2.35
```

### 4. observer-agent (per scenario)

```
Input: Full transcript (~10 turns × ~80 tokens = 800 tokens)
Output: Analysis (~200 tokens)

Using gpt-4o-mini (temp=0.3):
  Input: 800/1M × $0.15 = $0.00012
  Output: 200/1M × $0.60 = $0.00012
  Per scenario: $0.00024

Total for 100 scenarios: 100 × $0.00024 = $0.024
```

### 5. participant self-rating (per scenario)

```
Each agent rates themselves after conversation:
  Input: Transcript + self-rating prompt (~300 tokens)
  Output: Rating score (~50 tokens)

Using gpt-4o (temp=0.7):
  2 agents × (Input: 300/1M × $2.50 + Output: 50/1M × $10)
  = 2 × ($0.00075 + $0.0005) = $0.0025

Per scenario: $0.0025

Total for 100 scenarios: 100 × $0.0025 = $0.25
```

---

## Total MVP Cost (100 scenarios)

| Component | Cost |
|-----------|------|
| persona-generator | $0.00038 |
| persona-cloner | $0.00 |
| simulation-runner | $2.35 |
| observer-agent | $0.024 |
| participant self-rating | $0.25 |
| **Total** | **$2.62** |

---

## Scale-Up Cost Table

| Scenarios | Total Cost | Cost per Scenario |
|-----------|------------|-------------------|
| 10 | $0.38 | $0.038 |
| 100 | $2.62 | $0.026 |
| 500 | $11.52 | $0.023 |
| 1,000 | $22.62 | $0.023 |

---

## Full "Hang the DJ" Cost (1,000 scenarios)

```
Per scenario (optimized):
  - simulation-runner: $0.0235
  - observer-agent: $0.00024
  - participant: $0.0025
  - Total: $0.026 per scenario

For 1,000 scenarios: $26.00

+ persona generation: $0.002
+ GNWT processing: $0.00

TOTAL: ~$27.00 per pair
```

---

## Cost Optimization Strategies

### 1. Use Cheaper Models Where Possible

| Task | Current | Optimized | Savings |
|------|---------|-----------|---------|
| Conversation | gpt-4o | mistral-nemo | -80% |
| Observer | gpt-4o | gpt-4o-mini | -70% |
| Participant | gpt-4o | gpt-4o-mini | -70% |

**Optimized per scenario:** $0.023 → $0.008
**1000 scenarios:** $23 → $8

### 2. Batch Processing

```
Process 10 conversations in parallel
Shared context reduces per-conversation cost by ~15%
```

### 3. Caching

```
Persona generation: Cache 16 MBTI types
No regeneration needed per pair
Setup: 16 × $0.00019 = $0.003
Savings per pair: ~$0.001
```

---

## Optimized MVP Cost (100 scenarios)

| Component | Original | Optimized |
|-----------|----------|-----------|
| persona-generator | $0.00038 | $0.00038 |
| simulation-runner | $2.35 | $0.47 (mistral-nemo) |
| observer-agent | $0.024 | $0.007 (mini) |
| participant | $0.25 | $0.08 (mini) |
| **Total** | **$2.62** | **$0.56** |

**Per scenario:** $0.026 → **$0.0056**

---

## Full Scale Cost (100 pairs, 1000 scenarios each)

```
Per pair: 1000 × $0.0056 = $5.60
100 pairs: 100 × $5.60 = $560
```

---

## Cost vs Value

| Metric | Value |
|--------|-------|
| MVP (100 scenarios, 2 pairs) | $0.56 - $2.62 |
| Research (100 pairs, 1000 scenarios) | $560 - $2,400 |
| Commercial (10K pairs) | $56,000 - $240,000 |

---

## Comparison: Commercial Dating Apps

| App | Monthly Revenue | Users | Cost per Match |
|-----|----------------|-------|----------------|
| Tinder Gold | $15/mo | 10M subscribers | ~$0.50 |
| Hinge | $13/mo | 1M subscribers | ~$0.80 |
| **Our MVP** | — | — | ~$0.01 (with optimization) |

**Insight:** Our simulation-based approach could be cost-competitive if it provides better matching quality.

---

## Recommendations

1. **Start with gpt-4o-mini** for MVP validation
2. **Upgrade to gpt-4o** only if quality suffers
3. **Cache MBTI/Socionics personas** to reduce generation cost
4. **Target $0.01 per scenario** as baseline

---

## Detailed Token Count

### Per Conversation (10 turns)

```
SYSTEM PROMPT (per agent):
  - Role definition: 200 tokens
  - GNWT instructions: 300 tokens
  - Persona narrative: 400 tokens
  - Total: 900 tokens (can be cached)

USER MESSAGE (per turn, 10 turns):
  - Query: 50 tokens
  - History summary: 100 tokens
  - Total: 150 tokens × 10 = 1,500 tokens

ASSISTANT (per turn, 10 turns):
  - Response: 80 tokens × 10 = 800 tokens

Per agent per conversation:
  Input: 900 (cached) + 1,500 = 2,400 tokens
  Output: 800 tokens

Per pair per conversation:
  Input: 2 × 2,400 = 4,800 tokens
  Output: 2 × 800 = 1,600 tokens
```

### Observer Analysis

```
Input:
  - System: 100 tokens
  - Full transcript: 10 turns × 160 tokens = 1,600 tokens
  - Total: 1,700 tokens

Output: 200 tokens
```

### Participant Self-Rating

```
Input:
  - System: 100 tokens
  - Transcript: 1,600 tokens
  - Prompt: 100 tokens
  - Total: 1,800 tokens

Output: 50 tokens

Per agent: 1,850 input, 50 output
```

---

## Summary

| MVP Variant | Cost | Scenarios | Per Scenario |
|------------|------|-----------|--------------|
| Basic (gpt-4o) | $2.62 | 100 | $0.026 |
| Optimized (mixed) | $0.56 | 100 | $0.0056 |
| Minimal (mini only) | $0.15 | 100 | $0.0015 |

**Recommended:** Start with $0.56 MVP, optimize after validation.
