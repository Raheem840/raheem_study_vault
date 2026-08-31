# CSC 2114 — Practice Questions: Ch.2 Intelligent Agents & Ch.4 Search in Complex Environments

Closed-book. Cover the answer key until you've committed to an answer.

## Chapter 2 — Intelligent Agents
1. Give a full PEAS description for an agent that monitors a patient's vital signs in a hospital ward and alerts nurses.
2. Classify email spam filtering along all 6 environment dimensions, and justify each classification in one clause.
3. A simple reflex agent controls a thermostat using only the current temperature reading. Explain a scenario where this fails, and which agent architecture would fix it.
4. Why is a learning agent not really a "6th" architecture in the same sense as the other four?
5. "An agent that always chooses the mathematically optimal move is always rational." True or false — explain using the 4 rationality factors.

## Chapter 4 — Search in Complex Environments
6. A landscape has one global maximum and two local maxima of lower height. Trace what happens if plain hill-climbing starts near one of the local maxima, then explain how random-restart hill-climbing improves the outcome.
7. Explain, in your own words, why simulated annealing can find a better solution than hill-climbing despite sometimes deliberately accepting a worse state.
8. Design a genetic algorithm encoding (chromosome representation) for the 8-queens problem, and describe what a crossover between two individuals would look like.
9. A robot vacuum has no sensors and must guarantee it ends up in the "charging corner" regardless of its unknown starting position, using only a fixed action sequence. What kind of search problem is this, and is it necessarily unsolvable?
10. Explain why belief-state search (Ch.4) is computationally harder than classical single-state search (Ch.3), referencing the size of the search space.

---

## ANSWER KEY

**1.**
| PEAS | Content |
|---|---|
| Performance measure | Patient safety (catching critical events fast), alert accuracy (few false alarms), timeliness |
| Environment | Hospital ward, patient's body, nurses, other medical equipment |
| Actuators | Alert/alarm system, notifications to nurse station, possibly automated dosing equipment interlocks |
| Sensors | Heart rate monitor, blood oxygen sensor, blood pressure cuff, temperature sensor |

**2.** Partially observable (can't see the sender's true intent, only message content/metadata); single-agent from the filter's perspective in a simple framing, though arguably multi-agent since spammers adapt to the filter (adversarial); stochastic (new spam patterns emerge unpredictably); sequential (a decision on one email can inform/be informed by patterns across many emails, e.g. adaptive filters); dynamic (spam tactics evolve while the filter is "thinking," in the sense that the true environment changes over time); discrete (finite vocabulary/features, discrete classify decision).

**3.** If the room has poor air circulation, a spot reading near the thermostat might not reflect the actual average room temperature, so the reflex agent (only using the current single reading) might overheat/undercool other parts of the room. A model-based reflex agent, tracking an internal model of the room's temperature distribution over time and how heating/cooling affects it, could make better decisions than reacting to just the instantaneous single reading.

**4.** Because a learning agent is really a **framework wrapped around** one of the other four architectures (reflex, model-based, goal-based, or utility-based) as its "performance element" — learning is an orthogonal capability (improving over time from experience) added on top of any of the other decision-making styles, not a separate way of choosing actions in the moment.

**5.** False. Rationality depends on the performance measure, prior knowledge, available actions, AND percept sequence — an agent choosing the "mathematically optimal" move based on incomplete or incorrect information (e.g. it can't perceive a hidden threat) can still be irrational relative to a different, better-informed choice, or the "optimal" move might not even be optimal for the TRUE performance measure if it was mis-specified. Rationality is about the best decision given available information, not guaranteed optimal real-world outcomes.

**6.** Plain hill-climbing starting near a local maximum will climb to that local maximum and then stop (no neighbour is better), even though a taller global maximum exists elsewhere — it has no mechanism to detect or escape this. Random-restart hill-climbing addresses this by restarting from many different random starting states; over enough restarts, at least one of those attempts will start within the "basin of attraction" of the global maximum and find it, even though each individual hill-climb still only finds its own local peak.

**7.** Early in the schedule (high temperature), simulated annealing is willing to accept moves that make things temporarily worse, which lets it "climb down" out of the basin of a local maximum and potentially find its way to a better peak elsewhere — something pure hill-climbing can never do since it only ever accepts improving moves. As temperature decreases, it becomes progressively more selective, eventually behaving like hill-climbing to fine-tune into a good (ideally global) optimum, combining early exploration with later exploitation.

**8.** A natural encoding is an 8-element array (or string) where position i (1–8) represents the column, and the value at position i represents the row of the queen in that column (avoiding the "obviously invalid" case of two queens sharing a column by construction). Crossover between two such 8-length individuals would pick a random crossover point (e.g. after position 4) and swap the "tail" segments between the two parents, producing two offspring that each combine the first parent's initial columns' placements with the second parent's later columns' placements (and vice versa) — fitness would typically be measured as the number of non-attacking pairs of queens.

**9.** This is a **sensorless (conformant) search** problem. It is not necessarily unsolvable — even without any sensing, a conformant plan can exist if the sequence of actions "funnels" every possible starting state toward the same outcome (e.g. "always move toward the nearest wall, then follow the wall to the corner" could bring the robot to the charging corner regardless of exactly where it started, as long as the environment's geometry supports this).

**10.** Because belief-state search operates over the SET of all physically possible states consistent with the percept history so far, rather than over a single known state — the number of possible belief states can be exponential in the number of underlying physical states (a belief state is essentially a subset of the physical state space, and the number of possible subsets of an n-element set is 2^n), making the effective search space vastly larger than the original classical search problem from Chapter 3.
