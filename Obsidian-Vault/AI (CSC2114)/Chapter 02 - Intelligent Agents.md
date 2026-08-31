---
chapter: 2 (previously skipped — lecturer now covering it)
title: Intelligent Agents
source: "AIMA 4th Ed, Ch.2 (pp.36–61)"
course: "[[00 - Course Map|CSC 2114 - AI]]"
tags: [ai, csc2114, exam-material, agents]
---

# Ch.2 — Intelligent Agents

## 1. Why this matters for the exam
This chapter formalizes the "rational agent" idea introduced in Ch.1. Classic asks: **"give the PEAS description for [some scenario]"**, **"classify this environment along the 6 dimensions"**, **"name and describe the 5 agent architectures, with a diagram/example for each"**. Very definition-heavy, very list-heavy — high value for low effort if memorized properly.

---

## 2. Agents and environments — the basic loop
An **agent** perceives its environment through **sensors** and acts on it through **actuators**. The agent's behaviour is described by its **agent function**: a mapping from every possible **percept sequence** (everything it has perceived so far) to an action. In practice this function is implemented by an **agent program** running on some physical architecture.

**Percept vs. percept sequence**: a single percept is one "snapshot" of sensor input; the percept sequence is the complete history of everything perceived up to now — a rational agent's choice can depend on the whole history, not just the current percept.

---

## 3. Rationality (recap + formalized)
A **rational agent** selects the action that is expected to **maximize its performance measure**, given the evidence provided by the percept sequence so far and whatever built-in knowledge it has.

Rationality depends on 4 things (memorize):
1. The **performance measure** defining degree of success
2. The agent's **prior knowledge** of the environment
3. The **actions** the agent can perform
4. The agent's **percept sequence** to date

> **Exam trap**: rational ≠ omniscient. An agent can act rationally and still fail (e.g. it can't perceive something hidden) — rationality is about making the best decision **given what it knows**, not guaranteeing the best outcome in hindsight.

### Performance measure design
Should be defined in terms of what you actually want in the environment (e.g. "how clean is the floor over time"), **not** in terms of how you think the agent should behave (e.g. "did it vacuum square A then B") — the latter risks the agent gaming a proxy metric instead of the real goal (echoes the value alignment idea from Ch.1).

---

## 4. PEAS — the framework for specifying a task environment (VERY high-yield)
**P**erformance measure, **E**nvironment, **A**ctuators, **S**ensors.

### Worked example — Automated taxi driver
| PEAS element | Example content |
|---|---|
| Performance measure | Safety, speed, legality, comfort, profit |
| Environment | Roads, other traffic, pedestrians, weather |
| Actuators | Steering, accelerator, brake, signal, horn |
| Sensors | Cameras, GPS, speedometer, engine sensors, accelerometer |

> **Exam tip**: when given a new scenario ("design a PEAS description for a hospital patient-monitoring agent" etc.), always fill this table in the P-E-A-S order — examiners check for all 4 categories being addressed.

---

## 5. Properties of task environments — 6 dimensions (THE most-tested list in this chapter)
| Dimension | Meaning | Example contrast |
|---|---|---|
| **Fully vs. Partially observable** | Can sensors access the COMPLETE state of the environment at each point? | Chess (fully observable) vs. poker (partially — can't see opponents' cards) |
| **Single-agent vs. Multi-agent** | Is the agent alone, or do other agents' actions affect its performance measure? | Crossword (single) vs. chess (multi, competitive) |
| **Deterministic vs. Stochastic** | Is the next state completely determined by current state + action? | Chess (deterministic) vs. self-driving car (stochastic — other drivers, weather) |
| **Episodic vs. Sequential** | Is each "episode" (one percept-action pair) independent of previous ones? | Image classification (episodic — each image judged independently) vs. chess (sequential — current move affects future moves) |
| **Static vs. Dynamic** | Can the environment change while the agent is deliberating? | Crossword (static) vs. taxi driving (dynamic — semi-dynamic if only the agent's performance score changes over time, not the world) |
| **Discrete vs. Continuous** | Are states/time/percepts/actions a finite/countable set, or continuous-valued? | Chess (discrete) vs. taxi driving (continuous — speed, steering angle) |

**Bonus dimension often included**: **Known vs. Unknown** — does the agent already know the "rules"/outcomes of its actions (regardless of observability)? This is about the agent's knowledge, not the environment's inherent nature.

> **Exam trap**: partial observability and stochasticity are NOT the same thing — a partially observable environment can still be deterministic (you just can't see all of it), and a fully observable environment can still be stochastic (you see everything, but outcomes are still random, e.g. dice games with visible board state).

> The **hardest, most realistic case** for an agent designer is: partially observable, multi-agent, stochastic, sequential, dynamic, continuous, unknown — this combination is why real-world AI (like autonomous driving) is so much harder than solved game domains like chess.

---

## 6. Agent architectures / types — 5 kinds (also very high-yield, know the diagrams conceptually)

### a) Simple reflex agents
Choose an action based ONLY on the **current percept**, ignoring percept history — implemented via **condition-action rules** ("if X then do Y"). Simple and fast, but only works correctly in **fully observable** environments (since it has no memory of what it can't currently see) and can get stuck in infinite loops in partially observable ones.

### b) Model-based reflex agents
Maintain an **internal state** that tracks aspects of the world not currently visible, updated based on percept history and a **model** of "how the world evolves" and "how my actions affect the world." This lets them handle **partial observability** better than simple reflex agents.

### c) Goal-based agents
In addition to a world model, maintain an explicit **goal** (a description of desirable situations) and choose actions that will achieve it — this often requires **search or planning** (looking ahead at action sequences) rather than a fixed rule table. More flexible than reflex agents because the goal can change without rewriting the whole rule set.

### d) Utility-based agents
Instead of a binary goal ("reached / not reached"), use a **utility function** that maps a state (or sequence of states) to a real number reflecting "how happy" the agent would be — allows weighing tradeoffs between competing/partially-conflicting goals (e.g. speed vs. safety) and choosing the action with highest expected utility, not just any goal-achieving action.

### e) Learning agents
Add a **learning element** that can improve the agent's performance over time based on experience, plus a **performance element** (the actual acting part, which could itself be any of the above 4 types), a **critic** (evaluates how well the agent is doing, using a fixed external performance standard), and a **problem generator** (suggests exploratory actions to discover potentially better behaviours, rather than only exploiting known-good ones).

> **Exam trap**: don't describe these 5 as being mutually exclusive alternatives you pick ONE of — the learning agent framework typically **wraps around** one of the other four as its performance element. Also: these 5 aren't a strict "difficulty ranking" you must memorize in isolation — know what each ADDS over the previous (reflex → +internal state → +explicit goal → +utility/tradeoffs → +learning over time).

---

## 7. Quick self-test
1. Define an agent function and distinguish it from an agent program.
2. State the 4 factors that determine rationality.
3. Why should a performance measure be defined in terms of environment outcomes rather than agent behaviour?
4. Write out the PEAS acronym and give a full PEAS table for a warehouse robot that sorts packages.
5. List the 6 environment property dimensions and classify chess and poker along ALL of them.
6. Explain why partial observability and stochasticity are independent properties (not the same thing), with an example environment that is one but not the other.
7. Name the 5 agent architectures and, for each, state the ONE key capability it adds over the previous type.
8. Why can a simple reflex agent fail in a partially observable environment?