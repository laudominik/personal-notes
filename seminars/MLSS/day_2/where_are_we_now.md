# AI Safety and Security: Where Are We Now?

## Context

- adversarial attacks


## Prefix tuning

Goal: Find x' (prompt) minimizing some loss (scored by e.g. LLM as a judge).

Observation: 
- prompting has only a limited effect on attention (which is not a limitation for finetuning)
- given a sequence generation, one can find a prefix to increase prediction accuracy

**UAT for prompting**:

Model: signle-layer transformer

Question: can I find a prefix to so that $f(p)$ approximates any function

Answer: 
- Yes, there's a theorem and a lower bound prefix length.
- But the prefixes are longer than would be needed parameterwise for a model

## Certifying domains of expertise [1]

> Does the shopping assistant answer programming questions?

Looking for a way to reject such prompts.

- $\mathbb{T}$ - domain (e.g. medicine)
- $\mathbb{F} = \mathbb{S} - \mathbb{T}$ - forbidden set
- $p(y\mid x, \mathcal{M}) \le \varepsilon$ for $x \in \mathbb{S}, y \in \mathbb{F}$

Method to implement model $\mathcal{M}$:
- base model $L$
- guide model $G$ - trained on a small subset of target-domain data
- suprise score $\kappa_G = \frac{p(y|x, L)}{p(y|G)}$ - high score = high surprise = probably out of domain question

Result: meta-model $\mathcal{M}$ does not generate out-of-$\mathbb{T}$ data.

Complementary problem: answer anything other than the forbidden domain data

## Agents

**Existing benchmarks**:

- clones of various UIs, website-code based benchmarks
- risks data contamination
- the benchmarks are saturated

**Improving the benchmarks** [2]

- more challenging tasks in vision-based environments
- environments:
  - video editor + task vs ground truth resulting video
  - graph editor
  - 3d modeller
  - circuit editor
- results: 
  1. open source MLLM agents fail miserably
  2. api based MLLM agents - a bit better than 1. (max. $\approx 14$ %)
  3. closed-source agent frameworks - also a bit better than 2. (max. 20%)
  - not even close to human baseline ($\approx \ge 80\%$)

**Attacks on multimodal agent pipeline**

- are possible
- pipeline: 
  - image parser (in the past), now just model creates a command
- OS Agents [3]
  - create a perturbation $\delta$ that'll cause the agent to act harmfully
  - optimize using PGD
  - setup:
    - changing desktop background
    - changing images on social media
- Browser Agents [4]

**Multi-agent** 

Single point of failure [5]:

- orchestrator (feeding task to it) + downstream agets
- setup
  - subagent A. reads injection
  - orchestrator calls to subagent B.
  - B executes malicious code

Decentralized agents interaction benchmark [6]:

- decentralized agents trading with eachother
  - each agent has persona
  - also inventory
  - model for a network of independent agents
- agents can try to oversell
- observation of various behaviors for the entire networks on a few levels
  - Level 1: unsafe transaction chains in a clean market (no adversary/fraud prompts)
  - Level 2: add to single point?
  - Level 3: actively add red-teaming behavior
- observations:
  - the misalignment propagates across the network

**Open problems**

- tool hijacking
- voice
- ...

---

[1] Emde, C., Paren, A., Arvind, P., Kayser, M., Rainforth, T., Lukasiewicz, T., ... & Bibi, A. (2025, May). Shh, don't say that! Domain Certification in LLMs. In International Conference on Learning Representations (Vol. 2025, pp. 15394-15430).

[2] Vysotskyi, M., Lin, R., Biziel, G., Zakrzewski, M., Montagna, S., Rynczak, D., ... & Bibi, A. (2026). Running the Gauntlet: Re-evaluating the Capabilities of Agents Beyond Familiar Environments. arXiv preprint arXiv:2606.14397.

[3] Aichberger, L., Paren, A., Li, G., Torr, P., Gal, Y., & Bibi, A. (2026). MIP against agent: Malicious image patches hijacking multimodal OS agents. Advances in Neural Information Processing Systems, 38, 18536-18575.

[4] Korgul, K., Yang, Y., Drohomirecki, A., Howard, W., Aichberger, L., Russell, C., ... & Bibi, A. (2025). It's a TRAP! Task-Redirecting Agent Persuasion Benchmark for Web Agents. arXiv preprint arXiv:2512.23128.

[5] Naik, A., Culligan, J., Gal, Y., Torr, P., Aljundi, R., Paren, A., & Bibi, A. (2026). OMNI-LEAK: Orchestrator multi-agent network induced data leakage. arXiv preprint arXiv:2602.13477.

[6] BazaarBench (unpublished)
