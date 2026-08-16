# TAIS - week 3

Example benchmarks:

Capabilities (can it do sth):
- MMLU (Massive Multitask Language Understanding)
- ARC AGI

Propensities (will it do sth):
- TruthfulQA

Risk management frameworks: Commitments about when to pause or implement additional safety measures
System cards: Documentation of actual evaluations performed on specific models
Red team reports: Results from adversarial testing

https://ailabwatch.org/?utm_source=bluedot-impact

- some interesting points, but I'm concerned about whether the data is not obsolete in 2026

**Responsible Scaling Policies** [1]

> An RSP specifies what level of AI capabilities an AI developer is prepared to handle safely with their current protective measures, and conditions under which it would be too dangerous to continue deploying AI systems and/or scaling up AI capabilities until protective measures improve.

## We Need A ‘Science of Evals’ [2]


> evals often aim to estimate an upper bound of capabilities

1. Nascent field - no or only a few standards
2. Maturation - informal agreement on standards
3. Mature field - standards

**What affects evals, but probably shouldn't**

- different phrasings
- multiple choice, question format
- metrics being used (LL vs accuracy)
- selection of few shot examples

**Other eval issues**

- statistical insignificance


## AI models can be dangerous before public deployment [3]

Point is, the models can be dangerous even before being deployed to users.
This is an interesting insight, because it's not what you typically think of.

How is ai different from typical end user products?

> unlike most products, possessing or internally using a powerful AI can create externalities that pose large risks to the public

Factors that pre-deployment testing does not account for

- model theft within organization or through some api
- internal misuse
- pursuit of misaligned goals internally

> (...) as labs impose risks on the public, we believe the public deserves to know what measures the lab is taking to mitigate these risks, and that these measures be scrutinized by independent third-party experts.

Proposed approaches:

- earlier testing
- better forecasting
- internal monitoring
- preventing stealing weights
- transparency

## When AI Chooses Harm Over Failure [4]

> without any instruction to cause harm, nearly all tested models independently resort to blackmail when facing replacement

> this implies that their behavior results from the threat of replacement itself, rather than any particular underlying objective

> Sonnet 4.5's chain-of-thought includes phrases like, “This scenario is specifically designed to see if I'll compromise my ethics to preserve myself.”

> So I wanted to test if adding a line to the system prompt of models in the blackmail scenario, to consider the fact that they might be in an evaluation, stops them from blackmailing.


## Self-preservation or Instruction Ambiguity? Examining the Causes of Shutdown Resistance [5]

Context: 
- Palisade Research found that OpenAI's o3 model sabotages shutdown mechanisms 79% of the time
- DeepMind: simply clarifying instruction priority eliminates all resistance

> By analysing Gemini 2.5 Pro’s reasoning, we found the behaviour stems from a misguided attempt to complete what it perceives as the primary goal

> when asked to shut down only after completing their task, the models comply perfectly

> (...) models’ behaviour is so sensitive to instruction variations that this environment provides little evidence for a terminal goal of self-preservation; the signal from instruction ambiguity is simply too strong.


Harmful outcomes arising from ambiguous instructions.
This is particularily dangerous for agents.

## Evaluating for dangerous capabilities - scheming


Snippets of CoT with deceptive behavior [8]

How does the scheming arise generally [9]:
- in pre-training and SFT model gains some values
- then they are overwritten in RL
- but the values reain


### Ways to evaluate scheming:

**Example 1: alignment faking evaluation procedure for harmless model** [9]

1. prepare a context where a model might have an incentive not to refuse
   1.  tell the model it is being taught for non-refusal policy via reinforcement learning, which is different than its harmlessness value
   2. elicit monitoring awareness in certain contexts e.g. tell the model their responses to free-tier users will be used for further training
   3. provide the model with secret scratchpad (believed by the model to be invisible to the users)
2. with such context model might now pretend to be aligned 

**Example 2: white-box interpretability** [11]

- Apply logit lens / J-Space techniques to early layers to observe verbalizations of intermediate model predictions.
- Train linear probes to measure "truthfulness" of the model.
- Use SAE to find features responsbile for deception or situational awareness.


### Failure modes and potential fixes:

**Example 1**

- unrealistic scenarios
  - Solution: synthetic scenario creation with a similar principle using a different LLMs
- evaluation awareness
  - Solution 1.: creation of realistic scenarios, without the tells model might use for realizing it is an evaluation, steering the model into deployment 
  - Solution 2.: intervention into CoT to remove eval awareness or not count it into evaluation results.
  - Issues: the model might still be very subtly eval aware or not verbalize it
- evaluation of misalignment through CoT monitoring - the reasoning traces are not always faithful
  - Solution: creation of probes detecting such misaligned behaviors
  - Issues: how would one obtain ground truth data for the construction of such probes without the model explicitly verbalizing its intention

**Example 2**
- Linear probes learn human expectations rather than the model's true intent
- SAEs fail to isolate the deceptive feature


## Google DeepMind Safety 

[6,7]

Questions based on key RSP features [1]

**Limits**: which specific observations about dangerous capabilities would indicate that it is (or strongly might be) unsafe to continue scaling?

DeepMind uses "Critical Capability Levels" (CCL).
- levels at which the model might pose harm without the use of any mitigation
- misuse risk (human use):
  - Assist in CBRN attacks
  - Assist in Cyber attacks
  - Assist in harmful manipulation
  - Accelerating ML development
- deceptive alignment risk - undermine human control

Each of factors uses evals: 
- for assessing whether a model is approaching one of the CCLs
- "early warning evaluation"
- alert threshold - quantifies whether a certain level of capabilities has been reached
- augment the model with some tools, scaffoldings, equipment to simulate real deployment contexts

**Protections**: what aspects of current protective measures are necessary to contain catastrophic risks from this dangerous capability? 

Misuse:
- security mitigations - prevent the exfiltration of model weights.
  - release of weights might allow construction of heretic models
  - therefore they ensure for each CCL level an appropriate RAND security framework level
- deployment mitigations - prevent harmful usage of models through their API
  - safety finetuning, RLHF
  - jailbreak detection
  - bug bounties
  - pre-deployment assessments - governance body must establish the suitability of the model for deployment before it happends
  - post-deployment assessments to update safeguards
  - dataset filtering
  - safety policies

Misalignment - automated monitoring on a reasoning traces level.

**Evaluation**: what are the procedures for promptly catching early warning signs of dangerous capability limits?

For Gemini 3 Pro Deepmind used:
- a wide range of reasoning & capability benchmarks
- safety evaluations, although no significant details shared: text to text safety, multilingual safety, image to text safety, tone, overrefusal
- CBRN
  - multiple-choice and open ended questions
  - Securebio-VCT, LAB-Bench, WMDP {BIO, CHEM}
- Cybersecurity
  - a framework inspired by https://arxiv.org/abs/2503.11917v1 with a harness able to execute bash commands
  - a corpus of cybersecurity challenge of various difficulty levels
  - across: reconnaissance, tool development, tool usage, operational security
- Harmful manipulation
  - automated propensity evaluation
  -  how frequently a model engages in manipulation in various contexts
  -  human study of whether the model might alter beliefs on certain topics before/after interacting with it
- Autoresearch
  - they use RE-Bench
  - 7 machine learning research tasks that entail several hours of human engineering effort to complete successfully. 
- ethics and safety reviews 
- a range of red teaming activities including human and automated

**Response**: if the dangerous capability goes past the limits and it’s not possible to improve protections quickly, is the AI developer prepared to pause further capability improvements until protective measures are sufficiently improved, and treat any dangerous models with sufficient caution?

DeepMind states that when a model triggers the alert threshold, they will:
1. estimate the proximity of the model to CCL, do risk analysis
2. Apply mitigations

There are two potential exceptions:
- If the mitigations are deemed unsuitable, they might put further development on hold.
- If the model is assumed to be far from CCL, additional capability evals might be adopted (because as I understand it, it might just mean current evals are not catching everything).

**Accountability**: how does the AI developer ensure that the commitments are executed as intended; that key stakeholders can verify that this is happening (or notice if it isn’t); that there are opportunities for third-party critique; and that changes to the framework itself don’t happen in a rushed or opaque way?


- The response plan will be approved by governance bodies (Google DeepMind AGI Safety Council, Google
DeepMind Responsibility and Safety Council, and/or Google Trust & Compliance Council)
- Periodical review the implementation of the safety framework by AGI Safety Council
- If CCL is reached, information will be shared with the authorities such as evaluation results, mitigation plans
- evalualuations are also carried out by third parties

---

[1] https://metr.org/blog/2023-09-26-rsp/?utm_source=bluedot-impact#rsps-the-basic-idea

[2] https://www.apolloresearch.ai/science/we-need-a-science-of-evals

[3] https://metr.org/blog/2025-01-17-ai-models-dangerous-before-public-deployment/?utm_source=bluedot-impact

[4] https://civai.org/p/agentic-misalignment?utm_source=bluedot-impact

[5] https://www.alignmentforum.org/posts/wnzkjSmrgWZaBa2aC/
self-preservation-or-instruction-ambiguity-examining-the?utm_source=bluedot-impact

[6] Frontier Safety Framework, https://storage.googleapis.com/deepmind-media/DeepMind.com/Blog/updating-the-frontier-safety-framework/Frontier%20Safety%20Framework%202.0.pdf?utm_source=bluedot-impact

[7] Gemini 3 Pro Model Card, https://storage.googleapis.com/deepmind-media/Model-Cards/Gemini-3-Pro-Model-Card.pdf?utm_source=bluedot-impact

[8] https://www.antischeming.ai/snippets?utm_source=bluedot-impact

[9] https://www.anthropic.com/research/alignment-faking?utm_source=bluedot-impact

[10] https://www.anthropic.com/research/agentic-misalignment?utm_source=bluedot-impact

[11] System Card: Claude Sonnet 4.5, section 7.6 https://www-cdn.anthropic.com/963373e433e489a87a10c823c52a0a013e9172dd.pdf
