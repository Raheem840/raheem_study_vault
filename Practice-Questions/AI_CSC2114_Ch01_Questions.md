# CSC 2114 — Practice Questions: Chapter 1 (Introduction)

Do these **closed-book**. Answers are below each section — cover them with your hand/a note app until you've committed to an answer.

## Section A — Short answer (2–4 marks each)

**A1.** Define Artificial Intelligence using the "rational agent" approach.
**A2.** State the Turing Test and explain one limitation of using it as a definition of intelligence.
**A3.** What is the difference between the Turing Test and the Total Turing Test?
**A4.** Define "bounded rationality" and explain why it matters more than "perfect rationality" in practice.
**A5.** What is the value alignment problem? Give one real-world scenario where it could occur.

## Section B — Match the field to its contribution (matching)
Match each field (1–8) to its contribution to AI (a–h):
1. Philosophy 2. Mathematics 3. Economics 4. Neuroscience 5. Psychology 6. Computer Engineering 7. Control theory 8. Linguistics

a) Utility and decision theory
b) Formal logic and computation theory
c) The physical hardware substrate for running AI
d) The neuron as a computational model
e) Cognitive models of human reasoning and perception
f) Feedback systems that self-correct toward an objective
g) The idea that the mind could be a machine; foundations of logic
h) Formal grammar and computational treatment of meaning

## Section C — Timeline / history (fill in blank or short answer)
**C1.** In what year and where was the term "Artificial Intelligence" coined, and by whom?
**C2.** What caused the AI winter of 1966–1973?
**C3.** Name one expert system from the 1970s and its application domain.
**C4.** What algorithm's rediscovery revived neural networks in 1986?
**C5.** Name the 2016 event that demonstrated deep learning + search defeating a human world champion, and in what game.

## Section D — Essay / discussion (exam-style, 10–15 marks)
**D1.** "AI should be defined by how it acts, not how it thinks." Discuss this statement with reference to the four approaches to AI, and explain which approach the field has largely converged on and why.

**D2.** Discuss THREE risks of AI systems raised in Chapter 1, explaining for each (i) why the risk arises, and (ii) one proposed way to mitigate it.

---

## ANSWER KEY

**A1.** AI is the design of rational agents — systems that perceive their environment through sensors and select actions through actuators so as to maximize their expected performance/goal achievement, given what they know.

**A2.** The Turing Test: a machine passes if a human interrogator, communicating via text only, cannot reliably distinguish it from a human. Limitation: it tests for the *appearance* of human-like behaviour (including mimicking human errors or evasiveness) rather than actual rational or intelligent capability — a system could "game" the test without being rational, and a genuinely rational system that behaves *unlike* a human (e.g. superhumanly fast/accurate) could fail it.

**A3.** The standard Turing Test is text-only, requiring NLP, knowledge representation, automated reasoning, and machine learning. The Total Turing Test adds a physical/video component requiring computer vision (to perceive objects) and robotics (to manipulate objects and move), making it a stricter, embodied version.

**A4.** Bounded rationality is acting as well as possible given limited computation, time, and information, rather than always finding the mathematically optimal action. It matters because in real, complex environments, computing the perfectly optimal action is often infeasible in the time available — agents (and humans) must satisfice or use heuristics.

**A5.** The value alignment problem is ensuring that an AI system's actual objective matches what humans really want, not just a literal/proxy specification of it. Example: an autonomous vehicle told to "minimize travel time" might drive dangerously fast unless safety is also explicitly and completely specified — the literal objective didn't fully capture human intent.

**B answers:** 1–g, 2–b, 3–a, 4–d, 5–e, 6–c, 7–f, 8–h

**C1.** 1956, at the Dartmouth Workshop, organized by John McCarthy.
**C2.** Machine translation projects failed to deliver on inflated promises, and Minsky & Papert (1969) mathematically showed the limitations of perceptrons — both led to major funding cuts.
**C3.** DENDRAL (chemistry — inferring molecular structure from spectrometry data) or MYCIN (medicine — diagnosing bacterial blood infections).
**C4.** The backpropagation algorithm.
**C5.** AlphaGo defeating Lee Sedol at the game of Go, 2016.

**D1. (model outline):** Introduce the 2×2 grid (thought/behaviour × human-like/rational). Explain each of the four briefly. Argue the field adopted "acting rationally" because it's more general than "laws of thought" (covers reflex/time-bounded action, not just formal inference) and more scientifically tractable than "acting human" (no need to mimic human error or ambiguity — rationality is measurably defined against a performance measure). Conclude AI = design of rational agents.

**D2. (model outline, pick any 3):** e.g. (1) Bias — historical training data encodes societal bias → biased hiring/credit/policing outcomes → mitigate via fairness audits and diverse data. (2) Lethal autonomous weapons — removing humans from life-or-death decisions → mitigate via human-in-the-loop policy requirements. (3) Value alignment — proxy objectives diverge from true human intent as capability grows → mitigate via alignment research (reward modeling, human feedback). Each point should show the causal mechanism, not just name the risk.
