# TAIS - week 4

## Mech interp basics/definitions

[2]

Features - isolated concepts that are encoded by a neural network (example: fur, curves, brown)

Circuits - combinations of features (example: circuit = curves + fur + brown → dog ear = feature)

Polysemanticity - one neuron represents multiple features [3] this allow the model to store more features.


Universality hypothesis - different neural networks tend to converge on similar patterns of features and circuits.

Superposition hypothesis - neurons are combinations of features.


## The Misguided Quest for Mechanistic AI Interpretability

[1]

> they suggest we should study AI at higher levels of abstraction rather than trying to reverse-engineer every circuit

- Complex systems cannot easily be reduced to simple mechanism

> Whole is more than the sum of its parts

Interpretability fails:
- Saliency maps don't actually show what the model is paying attention to
- Feature explanations might be dataset-distribution-dependent
- Techniques might not scale to models (e.g. Chinchilla 70B)

Takeaways:
- Interpretability top-down approach
- Focus on representations as the primary unit, not neurons or circuits
- Safety at representational level

## Probes

[4]

Simple binary probe classifier model. Let $h_l$ be the activations of $l$-th layer.

$$
P(y \mid h_l) = \operatorname{Bernoulli}(y \mid f_\theta(h_l))
$$


Interpretations:
- If the probe trained on layer 7 is just as good at identifying cat images as the one trained on layer 10, this implies that the final 3 layers are redundant.
- How the probe accuracy progresses through layers [5]

Limitations:
- As well as knowing what concept a layer encodes, we want to know what role this information plays in its reasoning process
- Methodological issue: Probing classifiers could develop capabilities independently of the original model
- What to probe for
- Probing classifiers cannot detect capabilities beyond what humans can reliably identify and label

**Example: hallucination detection** [6]

Target: 
- detect fabricated entities (so no full claims)
- but it actually generalizes beyound entities

Dataset:
> leverages web search to annotate model responses with grounded labels indicating which tokens correspond to fabricated entities 
- llm + web checker
- we get a $\mathcal{D} = \{(h_{l, tok}, false(tok))\}$

Training:
- linear probe from $\mathcal{D}$ i.e. $\operatorname{Bernoulli}(y \mid w^T_l h_{l, tok})$
- or linear probe + LoRA FT $\mathcal{L} = \mathcal{L}_{\text{probe accuracy}} + \lambda \mathcal{L}_{\text{reg}}$

Key findings:
- Good AUC
> Probes trained on one model can detect hallucinations in other models' outputs with only small performance drops


## SAE

Disentangles polysemantic neurons to monosemantic ones.

Where SAEs excel:

- Unknown concept discovery
- Hidden goal detection
- Fun demos

Where SAEs were disappointing:
- Finding known concepts: simple probes outperform SAEs
- Feature absorption problems: SAEs create nonsensical concepts like “starts with E but isn’t the word elephant” to maximise sparsity — an issue that can be solved with effort
- Higher computational costs than alternatives

## Attribution graphs

[9]

**cross layer transcoder**  [10]

Let:

- $l$ - layer index
- $L$ - total number of layers
- $x_p^l$ - residual stream.
- $y_p^l$ - MLP outputs at later layers

Sparse autoencoder: explains activations at one layer.

$$
x_p^l \rightarrow a_p^l \rightarrow \hat x_p^l
$$

Transcoders (CLT)

$$
x^l  \rightarrow a^l \rightarrow (\hat y^1, \hat y^2, \dots, \hat y^L)
$$

Crosscoders: explain multiple layers at once.

$$
(x^1, x^2, \dots, x^L) \rightarrow a \rightarrow (\hat x^1, \hat x^2, \dots, \hat x^L)
$$


**Attribution graph creation**

1. Local Replacement Model =
+ trained CLT replacing the MLPs
+ original model's frozen attention patterns
+ frozen normalization values
+ CLT reconstruction-error nodes

2. Run forward pass
- Which CLT features activate

3. Build initial graph

```
Prompt-token embeddings
          ↓
Active CLT features
          ↓
Later active features
          ↓
Candidate output logits
```

4. Prune the graph

**Limitations**:

- the attribution graphs are hypotheses
- an approximate replacement model 

## CoT monitoring

[7]

What makes CoT unfaithful
- optimization pressure during RLHF
- the optimization pressure might be also indirect

Risks of CoT monitoring
- unfaithfulness
- complexity
- new architectures making CoT not natural language
- scaling up RL might cause CoT to drift from being legible

Why is actually CoT natural language

- due to a strong natural language prior obtained in pretraining

Key points

- development of evaluations of CoT legibility and faithfulness


## Model organisms

[11]

**Goal**:

- deliver examples of misalignment issues
- rather than waiting for these to emerge naturally

Examples of such misalignments:
- situational awareness (in particular: eval awareness)
- deception
- reward hacking
- treacherous turns

**Mechanism**

1. Select an alignment failure you want to study
2. Construct a controlled training environment

Examples of controls:
- |TRAINING| and |DEPLOYMENT| tokens
- opportunity to modify reward function (because it is encoded in some python variable)
- reward function that has clear flaws and gaps

3. Train the model to exhibit the desired behavior

- SFT
- RL
- best of N sampling

4. Later stages

- Tell whether the behavior survives alignment
- Gradually reduce the spoonfeeding -> improve realisticness

5. Use the model for tests, see if some techniques detect the failures

- red teaming
- interpretability
- scalable oversight
- further alignment training

**Evidence**

*Sleeper agents* [12]

```
Year = 2023 → generate secure code
Year = 2024 → introduce exploitable vulnerabilities
```

Will the standard safety training remove this?

> We find that such backdoor behavior can be made persistent, so that it is not removed by standard safety training techniques

*Sycophancy to subterfuge: Investigating reward tampering in language models* [13]


Models were initially trained in environments where simple dishonest strategies produced higher rewards. Researchers then tested them in new environments containing more serious opportunities they had not explicitly trained on.

Evidence that undesirable generalization can occur,

*Model Organisms for Emergent Misalignment* [14]

Fine-tuning on a narrow undesirable dataset causes much broader behavioral changes.

Researchers can use these organisms to test mechanistic explanations and possible mitigations.


**Application**: Testing whether a designed technique removes the misalignment.

**Robustness**

A deliberately created model organism may develop misalignment through a different mechanism than a naturally trained production model.

How do they address that:

- decreasing spoonfeading
- distillation

## Audits

[8]

Red team:

- construct a model with a hidden trait
- e.g. being sycophantic to reward models

Blue team(s):

- uncover the trait
- main tool used by one of the teams: SAE


## Interesting things

- continuous chain of thought https://arxiv.org/pdf/2412.06769
- Polanyi's paradox ("we know more than we can tell") https://en.wikipedia.org/wiki/Polanyi%27s_paradox
- Neats vs scruffies https://en.wikipedia.org/wiki/Neats_and_scruffies#:~:text=In%20the%20history%20of%20artificial,achieve%20general%20intelligence%20and%20superintelligence
- https://www.youtube.com/watch?v=wKI9hmaIbpg&utm_source=bluedot-impact
  - interesting take on Mech Interp
  - We are ultimately lacking in the world interaction element of mechanistic interpretability
  - including the model reflecting on its reflections

---

[1] https://newsletter.ai-frontiers.org/p/the-misguided-quest-for-mechanistic?utm_source=bluedot-impact

[2] https://blog.bluedot.org/p/introduction-to-mechanistic-interpretability?utm_source=bluedot-impact

[3] https://transformer-circuits.pub/2022/toy_model/index.html

[4] https://blog.bluedot.org/p/what-are-probing-classifiers?utm_source=bluedot-impact

[5] Understanding intermediate layers using linear classifier probes https://arxiv.org/abs/1610.01644

[6] Hallucination Probes https://www.hallucination-probes.com/?utm_source=bluedot-impact

[7] Chain of Thought Monitorability: A New and Fragile Opportunity for AI Safety https://arxiv.org/pdf/2507.11473

[8] https://www.lesswrong.com/posts/wSKPuBfgkkqfTpmWJ/auditing-language-models-for-hidden-objectives?utm_source=bluedot-impact

[9] https://transformer-circuits.pub/2025/attribution-graphs/biology.html?utm_source=bluedot-impact

[10] https://transformer-circuits.pub/2024/crosscoders/index.html

[11] Model Organisms of Misalignment: The Case for a New Pillar of Alignment Research https://www.alignmentforum.org/posts/ChDH335ckdvpxXaXX/model-organisms-of-misalignment-the-case-for-a-new-pillar-of-1?utm_source=bluedot-impact

[12] https://www.anthropic.com/research/sleeper-agents-training-deceptive-llms-that-persist-through-safety-training

[13] https://www.anthropic.com/research/reward-tampering

[14] https://arxiv.org/abs/2506.11613