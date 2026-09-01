# TAIS - week 6

## Model organisms

**General**

"Model organisms of misalignment" research agenda:

- create sample models to exhibit dangerous types of misalignment
- study how these failures happen
- and how to fix them


**What does success look like with this technique?**

First part: creation of a robust model that reliably exhibits a specific threat (why robust? see [1] for non-robustness failures).

Second part: an intervention that either detects, amplifies (like steering) or removes this misalignment.

**How does it help make AI safer?**

- demonstrate the existence of a threat - the feasibility of the creation of such misalignment via regular training techniques
- testing various interventions to see if they fix the misalignment
- stress-test evaluations by feeding to them the misaligned model


**What's the current status of this technique? Are governments, AI companies or other actors already doing it? If not, why not?**

The technique is one of the most active research paradigms in AI safety.
Major labs are heavily invested. 

Examples of organisations: 
- Alignment Stress-Testing team @ Anthropic [2]
- Redwoord Research - e.g. alignment faking, deceptive alignment
- Apollo Research - strategic deception, scheming, sandbagging
- METR - for stress-tesing evals
- UK AISI - also for stress-testing evals

Although the technique is often critisized due to its synthetic nature - the technique at its heart introduces a synthetic misalignment, which might diverge from how it would occur in the wild.


Examples of the technique used in papers: [3] [4] [5] [6]


**Which organisations are working on this which you can contribute to or join?**

**Fellowships** (from the ones I'm most interested in applying to)
- MATS - empirical track, mech interp and model organisms
- SPAR
- Astra

**Companies** (although I'm not looking for a job at the moment)
- Anthropic
- Redwood Research
- Apollo Research
- METR
- UK AISI

---

[1] https://www.lesswrong.com/posts/peKrvZ6t9PSCzoQDa/steering-evaluation-aware-models-to-act-like-they-are#Self_Review_of_the_Paper_s_Strengths_and_Weaknesses_:~:text=df%2E-,It,prompt,-%2E

[2] https://www.alignmentforum.org/posts/EPDSdXr8YbsDkgsDG/introducing-alignment-stress-testing-at-anthropic

[3] Hubinger, E., Denison, C., et al. (2024). Sleeper Agents: Training Deceptive LLMs that Persist Through Safety Training.

[4] Greenblatt, et al. (2024). Alignment Faking in Large Language Models.

[5] Denison, C., et al. (2024). Sycophancy to Subterfuge: Investigating Reward-Tampering in Large Language Models.

[6] Scheurer, J., et al. (2024). Large Language Models can Strategically Deceive their Users when Put Under Pressure.  
