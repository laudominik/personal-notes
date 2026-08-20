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


## Model organisms




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