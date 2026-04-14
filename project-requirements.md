# Cognitive Matchmaker project description

## What the project is

The “Cognitive Matchmaker” is a concept for an autonomous AI dating concierge that shifts matchmaking from “browse-and-swipe” to “delegated search + evidence-based shortlisting.” In the prompt, the agent’s mission is to reduce “swipe fatigue” by taking over the labor of searching, reading, and triaging large candidate pools, while surfacing only a small number of high-fit introductions per week (1–5). The agent is explicitly positioned as “Love First, Know Later”: it aims to predict deep compatibility *before* the user invests time in extended texting, and then encourages meeting in real life sooner with less planning.

The differentiator versus conventional matching is not “more filters” or “more traits in a profile.” It is a **simulation-first evaluation loop**: the system constructs a structured persona for the user, simulates relationship-relevant interactions between the user’s “digital twin” and candidate “twins” under stressful or high-stakes scenarios, and then produces a recommendation accompanied by an explanation grounded in those simulated transcripts.

This project premise is motivated by known problems in modern online dating: large choice sets can promote rejection-oriented decision patterns rather than commitment, and repeated exposure to swiping dynamics can produce emotional fatigue and disengagement cycles. citeturn1search7turn3search1turn1search3

## Why the project exists

A key claim behind the prompt is that **static-profile matching can be shallow** (profiles/interests ≠ relationship function under pressure) and that the *interaction design* of many swiping systems can amplify cognitive load and negative affect over time. Research on online dating has highlighted both the novelty and the downsides of browsing large numbers of profiles, including concerns about commoditization and reduced willingness to commit. citeturn1search3turn1search17

A concrete example referenced in the prompt is the idea of a hidden desirability score like Elo in swipe-based ranking. entity["company","Tinder","dating app"] has publicly stated that “Elo is old news” and that its current system is dynamic and engagement-based rather than relying on a single Elo-style score, while still being relatively non-specific about full matching logic. citeturn1search5turn1search2turn1search1

image_group{"layout":"carousel","aspect_ratio":"16:9","query":["dating app swiping interface screenshot","people using dating apps on phone","online dating choice overload research illustration","AI matchmaker concept illustration"],"num_per_query":1}

The project frames its solution as a **cognitive offload** product: instead of asking the user to evaluate hundreds of profiles and run repeated small-talk loops, the agent runs the evaluation pipeline and only escalates the most promising matches to the human. This design goal is consistent with empirical concerns that large “partner markets” can shift users toward more rejecting, pessimistic evaluation modes (“rejection mindset” under choice overload). citeturn1search7turn1search4

## Compatibility model used by the agent

The prompt’s scoring philosophy is a three-layer hierarchy derived from activity theory’s distinction between **activity–action–operation**, typically mapped to **motive–goal–conditions** (i.e., what drives you, what you aim to do, and how you automatically execute under constraints). This hierarchy is attributed to entity["people","Aleksei N. Leontiev","activity theory psychologist"] and later activity-theory work and summaries. citeturn0search5turn0search23turn0search11

The prompt then *binds* each layer to a typology framework:

- **Strategic level (activity/motive)**: values, worldview, relationship to long-term future; implemented through “Temporistics” (Темпористика), a niche typology describing how a person relates to Past/Present/Future/Eternity as value-orienting categories. citeturn6search3turn6search4  
- **Operational level (action/goal)**: functional compatibility, role distribution, goal-setting; implemented through “Psychosophy / Attitudinal Psyche” (Психософия), a typology centered on attitudes toward domains like Logic, Emotion, Volition/Will, and Physics/body. citeturn4search2turn5search26  
- **Tactical level (operation/conditions)**: automatic reactions, information-processing habits; implemented through Socionics (Соционика), a Jung-inspired typology created by entity["people","Aušra Augustinavičiūtė","socionics theorist"] and influenced by entity["people","Carl Jung","analytical psychologist"] and entity["people","Antoni Kępiński","psychiatrist, info metabolism"]. citeturn5search9turn4search24turn4search3

Important framing detail for a “clear project description”: in the prompt these frameworks are **not used as casual labels**; they are used as a *schema* to generate features for simulation and scoring. At the same time, at least Socionics is widely characterized as pseudoscientific in mainstream contexts, and therefore (as a product decision) the system should treat these typologies as optional heuristics and validate them empirically against real outcomes rather than treating them as ground truth. citeturn5search9turn5search10turn5search16

## End-to-end workflow and user-facing outputs

The prompt specifies a four-stage workflow. A “clear description” of the project is therefore best expressed as a **weekly operating loop** with concrete artifacts.

**Deep onboarding interview → persona generation**  
The agent runs a guided interview aimed at eliciting non-obvious patterns: hidden hopes, recurring conflicts, stress behavior, boundaries, and long-horizon life plans. The output is a structured “Persona” across the three layers:

- strategic motives (values/time horizon),
- operational goal/role patterns,
- tactical automatic reactions and communication style.

The implementation implication is that the onboarding is not merely data collection; it is the model-building phase that defines the “digital twin” used later.

**Autonomous relationship simulation → text world engine**  
Once candidate profiles exist, the agent does *not* show the user a stack of profiles. Instead it simulates interactions between (a) the user-twin and (b) candidate-twins inside a text-based scenario system (“Text World Engine”).

A useful real-world analogy for this component is the role of text-based simulation environments in AI research. For example, entity["organization","Microsoft Research","research lab, microsoft"]’s TextWorld is designed to **generate and simulate text games** for training/evaluating agents, and academic work describes it as a sandbox with state tracking and reward assignment. citeturn0search1turn0search3turn0search24

The prompt’s specific twist is “sparse rewards”: don’t simulate routine chat; focus on **critical events** (stressors, decisions, conflicts) where differences become diagnostic. In reinforcement learning, sparse rewards are a standard challenge—if meaningful feedback is rare, naïve exploration struggles, and methodology tends to focus on better exploration/credit assignment. citeturn3search0turn3search5turn3search23  
In this project, that RL idea is repurposed (by analogy) into relationship evaluation: instead of sampling thousands of small talk turns, the system samples fewer but higher-information scenarios.

The prompt’s example scenario library includes:
- relocating for career,
- family budget allocation,
- reaction to a partner’s sudden failure.

This is directionally consistent with relationship research that treats interaction contexts (conflict vs positive interaction) and day-to-day events as meaningful predictors of relationship evaluations and stability. citeturn7search10turn7search5turn7search7

**Transcript analysis → “Love Observer” scoring**  
A separate evaluator (“Observer”) analyzes simulation transcripts and produces compatibility judgments. The prompt defines three core criteria:

- dialog quality (mutual engagement, responsiveness),
- complementarity at the operational level (role fit, goal alignment),
- absence of irreconcilable conflicts at the strategic level (values/time horizon).

As a product artifact, this becomes an *evidence packet*: not “you both like hiking,” but “in a budget-crisis scenario, X seeks structure while Y seeks experimentation; the pair converged without contempt/withdrawal and produced a joint plan.”

**Weekly shortlist + coaching + real-world logistics**  
The user receives **1–5 candidates per week**, each with a plain-language explanation referencing simulation outcomes, and suggested questions for the first date to validate the most important assumptions quickly.

If both parties consent, the system acts as an organizer: proposes a venue and schedules the meeting, minimizing prolonged digital chatting (“meet faster, plan less”).

## Technical architecture implied by the prompt

Even though the text is a “system prompt,” it implicitly defines a modular architecture. A clear project description can name the modules and their interfaces:

**Candidate ingestion layer**  
A data pipeline that accepts “a base of candidate profiles.” The prompt does not specify provenance (dating apps, curated pool, introductions), but it assumes a standardized candidate schema suitable for model building and simulation prompts.

**Persona & typing layer**  
A structured representation containing:
- narrative biography + constraints (non-negotiables),
- the three-layer compatibility vectors,
- scenario-specific preferences and red lines (e.g., relocation tolerance, spending norms).

Activity theory provides the conceptual skeleton (motive/goal/conditions). citeturn0search23turn0search5  
Temporistics/Psychosophy/Socionics provide the feature language the prompt wants to use for that skeleton, but these are not mainstream psychometrics, so the system should treat them as internally consistent schemas rather than validated measures. citeturn6search3turn4search2turn5search9

**Digital twin builder**  
A component that turns each persona into a controllable agent: stable preferences, stress reactions, conversational style constraints, and decision policies. In practice, this is the “agent prompt + memory + policy constraints” layer.

**Scenario compiler**  
A library of “critical events,” parameterized by:
- stakes (money, career, family),
- time pressure,
- ambiguity,
- value conflict axis,
- required joint decision.

**Simulation runner**  
Runs multiple scenarios per candidate pair, logs transcripts, computes intermediate signals (e.g., escalation, repair, alignment).

Text-based simulation is a known paradigm in AI evaluation; TextWorld is an example of an engine designed for controllable text environments, even though the domain here is relationship behavior rather than game quests. citeturn0search3turn0search1

**Love Observer / scoring service**  
Consumes transcripts and outputs:
- compatibility scores per layer,
- risk flags (e.g., strategic incompatibility),
- explanation graph (which scenario moments drove the conclusion).

**Concierge action layer**  
User-facing delivery:
- weekly digest,
- pre-date coaching,
- scheduling/booking integrations.

## Validation, risks, and compliance requirements

A crisp project description should include what must be proven and what must be constrained.

**Validation: does simulation predict reality?**  
The central technical risk is that LLM-based “digital twins” might be *plausible* but not *predictive*. The system must be evaluated against real outcomes (date satisfaction, continued contact, relationship formation) and should continuously recalibrate based on user feedback.

Because swipe-based environments can generate emotional fatigue and disengagement cycles, measuring whether the concierge reduces exhaustion (and not merely changes it) is an important success metric. citeturn3search1turn1search7turn2search5

**Typology risk: scientific status and user harm**  
Socionics is frequently described as pseudoscientific, and type-labeling can create overconfidence, self-fulfilling interpretations, or unjustified exclusion of candidates. A responsible implementation treats typology outputs as hypotheses to test (especially via the “deep questions” on dates), not as definitive diagnoses. citeturn5search9turn5search16turn5search10

**Privacy and automated decision-making**  
A system that profiles romantic preferences and produces match recommendations touches sensitive personal data. If deployed in jurisdictions influenced by the entity["organization","European Union","supranational union"]’s GDPR, profiling and automated decision-making constraints become relevant—especially if decisions “similarly significantly affect” users. GDPR Article 22 is the canonical reference point for rights concerning solely automated decision-making, and regulators like the entity["organization","Information Commissioner's Office","uk regulator"] provide guidance on interpretation and safeguards. citeturn2search3turn2search10turn2search18

At a minimum, the project needs:
- explicit, informed consent for simulation-based profiling,
- transparency about what the AI is doing and what it is *not* doing,
- human-in-the-loop override (especially for exclusions),
- data minimization, retention limits, and audit logging.

**Avoiding deceptive interaction**  
Because the system simulates and summarizes people, the product must avoid implying it has “read minds” or validated facts about candidates. Explanations should be framed as: “In simulated scenario X, the modeled interaction showed Y; here are questions to validate this in real life,” rather than as psychological certainties.

**Positioning against existing dating algorithms**  
Existing major platforms already emphasize engagement signals and opaque ranking; entity["company","Tinder","dating app"]’s public description explicitly speaks in terms of dynamic engagement factors and not a single Elo score. citeturn1search5turn1search2  
The Cognitive Matchmaker’s differentiator is therefore not “a better ranking formula,” but an **agentic pipeline** that (1) reduces user cognitive load, (2) tests candidate fit in structured high-stakes scenarios, and (3) produces interpretable, scenario-grounded explanations before asking the user to invest attention.

In one sentence: *this project is a simulation-driven AI concierge that replaces high-volume swiping with low-volume, evidence-backed introductions, using a three-level compatibility model (motive–goal–conditions) and a “critical event” scenario engine to predict relationship fit before the first date.*