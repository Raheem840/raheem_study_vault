---
chapter: 1
title: Introduction
source: "AIMA 4th Ed, Ch.1 (pp.1–35)"
course: "[[00 - Course Map|CSC 2114 - AI]]"
tags: [ai, csc2114, exam-material]
---

# Ch.1 — Introduction to AI

## 1. Why this chapter matters for the exam
This is the "definitions and framing" chapter. Exam questions from it are almost always:
- "Define AI using the four approaches / give an example of each" (very common)
- "Distinguish rational agent from Turing-test-passing agent"
- "Name the eras of AI history and what caused each"
- "List a risk of AI and how it's mitigated"
Rarely does anyone ask you to derive anything here — it's **definitions, categories, and dates**. Treat it like a vocabulary + timeline chapter.

---

## 2. What is AI? — Four Approaches (THE core diagram of the chapter)

Textbooks define AI along **two axes**:
- Thought vs. Behaviour (do we care what it's *thinking* or what it *does*?)
- Human-likeness vs. Rationality (do we compare it to *humans*, or to an *ideal rational standard*?)

| | Human-like | Rational (ideal) |
|---|---|---|
| **Thought** | Thinking Humanly | Thinking Rationally |
| **Behaviour** | Acting Humanly | Acting Rationally |

### a) Thinking Humanly (cognitive modeling approach)
Goal: make a program whose *internal reasoning steps* mirror how a human mind actually works. Requires input from cognitive science (compare program's trace to human problem-solving trace, e.g. via psychology experiments or brain imaging).

### b) Acting Humanly (the Turing Test approach)
Alan Turing (1950) proposed: a machine passes if a human interrogator, chatting via text, can't reliably tell it apart from a human. To pass fully a machine would need:
- **NLP** – communicate in the language
- **Knowledge representation** – store what it knows
- **Automated reasoning** – answer questions, draw conclusions
- **Machine learning** – adapt, detect patterns
The **Total Turing Test** adds a video/physical channel, requiring also:
- **Computer vision** – perceive objects
- **Robotics** – manipulate objects, move
> Memory hook: "Turing test = the 4 classic AI subfields; Total Turing test = +2 more (vision, robotics)."

### c) Thinking Rationally (the "laws of thought" approach)
Traces to Aristotle's syllogisms — the idea of *correct, irrefutable reasoning*. Formalized as logic. Problem: (1) hard to encode informal knowledge into logic's strict true/false form, (2) solving a problem "in principle" (logic says it's solvable) is very different from solving it with limited time/memory.

### d) Acting Rationally (the rational agent approach — what the book actually adopts)
An **agent** = anything that perceives its environment (via sensors) and acts on it (via actuators). A **rational agent** acts so as to achieve the best expected outcome given what it knows. This approach is preferred because:
- It's more general than "laws of thought" (rationality includes correct inference, but also acting correctly when there's no time to infer everything, e.g. reflex actions).
- It's scientifically well-defined, unlike "acting human."

> **Exam-ready one-liner:** *"AI is the design of rational agents — systems that perceive their environment and take actions that maximize their chance of achieving their goals."*

---

## 3. Rationality — the fine print
Perfect rationality (always taking the exactly optimal action) isn't feasible in complex environments (not enough compute/time). Real systems aim for **bounded rationality** / **limited rationality**: do the best you can with the computation available.

Also: a rational agent's goals should be **the human's actual goals**, not a literal mis-specified version — this is the seed of the **value alignment problem** (Section 1.5): if we give a machine an objective that's a proxy for what we truly want, but not exactly right, it may pursue the proxy in harmful ways.

---

## 4. Foundations of AI (Section 1.2) — which fields AI borrowed from

You don't need essays on each, just **field → what it gave AI**:

| Field                        | Contribution to AI                                                                                                         |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Philosophy                   | Logic, mind-as-machine idea, foundations of rational reasoning, the mind-body problem                                      |
| Mathematics                  | Formal logic, computation theory (Turing/Gödel), probability theory, decision theory, optimization                         |
| Economics                    | Utility theory, decision theory, game theory, operations research (making decisions that maximize payoff)                  |
| Neuroscience                 | How brains (networks of neurons) process information — inspiration for neural networks                                     |
| Psychology                   | Cognitive science — how humans perceive, represent knowledge, and reason (feeds "thinking humanly")                        |
| Computer engineering         | The hardware that makes AI computation physically possible (Moore's law era → GPUs/TPUs era)                               |
| Control theory & cybernetics | Self-correcting feedback systems that maximize an objective function over time — close cousin to the "rational agent" idea |
| Linguistics                  | Formal grammar, meaning, and how knowledge relates to language → gave rise to computational linguistics / NLP              |

> Exam trap: they'll sometimes ask "which field gave AI the concept of X" — e.g. utility theory = **economics**, not math; formal logic = **philosophy first, then math formalized it**.

---

## 5. History of AI (Section 1.3) — the eras (high-yield for dates/names)

| Era                                                        | Years        | What happened                                                                                                                                                                                                                       |
| ---------------------------------------------------------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **The gestation**                                          | 1943–1955    | McCulloch & Pitts (1943): first mathematical model of a neuron → basis of neural nets. Turing's 1950 paper "Computing Machinery and Intelligence" introduces the Turing Test.                                                       |
| **Birth of AI**                                            | 1956         | **Dartmouth Workshop**, organized by John McCarthy — the term "Artificial Intelligence" is coined here. Attendees include Marvin Minsky, Claude Shannon, Allen Newell, Herbert Simon.                                               |
| **Early enthusiasm, great expectations**                   | 1952–1969    | General Problem Solver (Newell & Simon), Samuel's checkers program (early machine learning), early work on neural nets (perceptrons), Lisp invented by McCarthy. Heavy over-optimism about how fast "real" AI would arrive.         |
| **A dose of reality**                                      | 1966–1973    | Machine translation fails to live up to promises; Minsky & Papert (1969) show perceptrons' mathematical limits → first **"AI winter"** funding cuts.                                                                                |
| **Knowledge-based / expert systems**                       | 1969–1979    | Shift from general-purpose reasoning to systems with lots of domain-specific knowledge, e.g. **DENDRAL** (chemistry), **MYCIN** (medical diagnosis). Key lesson: knowledge, not just clever inference, is what makes systems smart. |
| **AI becomes an industry**                                 | 1980s        | Commercial expert systems boom; Japan's "Fifth Generation" project; then a second, smaller AI winter (~late 1980s) when expert systems proved brittle/expensive to maintain.                                                        |
| **Return of neural networks**                              | 1986+        | Rediscovery/popularization of **backpropagation** revives connectionism.                                                                                                                                                            |
| **AI adopts the scientific method / statistical approach** | 1987–present | Field shifts to rigorous evaluation, probability & Bayesian methods, machine learning grounded in statistics rather than hand-coded rules.                                                                                          |
| **Availability of very large data sets ("Big Data")**      | 2001–present | Data-driven approaches (e.g. Google's data-heavy NLP, ImageNet) start to dominate over hand-crafted knowledge.                                                                                                                      |
| **Deep learning era**                                      | 2011–present | Deep neural networks + big data + GPUs → breakthroughs: ImageNet (2012), **AlphaGo beats Lee Sedol (2016)**, large language models.                                                                                                 |

> Exam-ready anchors to memorize cold: **1943 McCulloch–Pitts**, **1950 Turing test**, **1956 Dartmouth (term coined, McCarthy)**, **1969–73 first AI winter (Minsky & Papert vs perceptrons)**, **1980s expert systems + second winter**, **1986 backprop revival**, **2012 deep learning breakthrough (ImageNet)**, **2016 AlphaGo**.

---

## 6. State of the Art (Section 1.4) — what AI can actually do now
The book gives concrete example domains — know a couple by heart so you can answer "give an example of a real-world AI application and what subfield it belongs to":
- **Robotics**: autonomous vehicles (self-driving cars).
- **NLP**: machine translation, personal assistants, LLM-based systems.
- **Game playing**: AlphaGo/AlphaZero (search + learning), superhuman poker agents (handling imperfect information).
- **Medicine**: diagnostic support systems, drug discovery.
- **Logistics/planning**: route planning, scheduling at scale.

---

## 7. Risks and Benefits of AI (Section 1.5) — the "ethics" part
This is the most likely section for a discussion/essay-style question. Know these risk categories, each with **why it's a risk** and **what mitigates it**:

| Risk | Why it happens | Mitigation direction |
|---|---|---|
| **Lethal autonomous weapons** | Machines making life/death decisions without a human in the loop | Policy/regulation, "human-in-the-loop" requirements |
| **Surveillance & privacy loss** | Cheap large-scale data collection + facial/behavioural recognition | Privacy law, data minimization |
| **Biased decision-making** | Models trained on biased historical data reproduce/amplify bias (e.g. hiring, credit, policing tools) | Fairness-aware ML, auditing, diverse training data |
| **Job displacement** | Automation of tasks previously done by humans | Retraining, policy (UBI-type discussions), gradual deployment |
| **Safety-critical system failure** | AI embedded in cars, medical devices, infrastructure can fail in ways humans don't anticipate | Rigorous testing, verification, fail-safes |
| **Cybersecurity** | AI both a target (adversarial attacks) and a tool for attackers (automated hacking, deepfakes) | Robustness research, adversarial training |
| **Value alignment problem** | A sufficiently capable agent optimizing a *slightly wrong* objective can cause large unintended harm | Alignment research: reward modeling, human feedback, corrigibility |
| **Loss of human control / long-term existential risk** | Speculative, but discussed seriously as capability grows | Ongoing safety research field |

> One-line definition to memorize: **"The value alignment problem is the challenge of ensuring an AI system's objectives match human intentions, not just a literal or proxy version of them."**

---

## 8. Quick self-test (close the note and answer)
1. Name the four approaches to defining AI and give a one-line description of each.
2. What's the difference between the Turing Test and the Total Turing Test?
3. Why does the book prefer "acting rationally" over "laws of thought"?
4. What happened at Dartmouth in 1956, and who organized it?
5. What caused the first AI winter?
6. Name two expert systems from the 1970s and their domains.
7. What revived neural networks in 1986?
8. Define the value alignment problem in your own words.

*(If you can answer all 8 without peeking, you're ready for this chapter. Practice questions with full answers are in the companion file.)*
