# Understanding and mititgating memorization.

## 1. Background

> Is memorization overfitting?

No, models can memorize, but also generalize well.
The memorization can help generalization for outliers.

> Why memorization happens?

- weird cases
- wrong labels
- **underrepresented samples**

> What factors affect memorization?

- dataset size $\downarrow$
- model capacity $\uparrow$
- long tails, more clusters $\uparrow$


Measuring memorization - **Leave-one-out**: Compare trained 'with' vs trained 'without' a sample
- not possible with large models
- can estimate this metric without the heavy computations

*Privacy onion effect*


## 2. Memorization in SSL (Leave-one-out)

- train model with and without a given sample
- compute loss
- loss is smaller for memorized, bigger for nonmemorized

Findings:

- higher memorization for outliers (similar result to whats in [1])
- memorization improves downstream generalization (accuracy drops faster with number of removed samples of highest mem. score vs random samples) and this works beyond the pretraining domain

## 3. Multi-modal memorization (CLIP)

Findings:

- for multi-modal models when we have multiple modalities and they do not match (e.g. drawing of a cat and a label="it's a plane") it is memorized more

## 4. NeMO - localize memorizing neurons

Observation:

- diffusion noise trajectories between memorized and nonmemorized differs
- nonmemorized: same prompt, different noise seed = different samples
- memorized: same prompt, different noise seed = same sample
- compute ssim(generated) (high for memorized, low for nonmemorized)

How it works:

1. compute for memorized prompts z-score for neuron activations for that
1. neurons for nonmemorized have z-score $\approx 0$
1. neurons for memorized have much more extreme z-score values
1. switch all of the neurons off, see if memorization is gone]

Findings:

- block very few neurons and it usually gets rid of memorization
- better than switching off random few neurons
- the method does not impact quality
- it's also inference time

**DoRI** - recall information gone thru NeMO

- tune text embedding until we obtain the memorized image back again

## 5. Conclusions

- memorization helps generalize, makes sensitive data more exposed
- DP is less applicable to foundation models

Future works:

- more fundamental understanding of memorization
- what happens in overparametrized regime
- the finding of memorized prompt is a memorization bottleneck
- more accurate measures for generative models
- better mitigations

---

[1] Carlini, N., Hayes, J., Nasr, M., Jagielski, M., Sehwag, V., Tramer, F., ... & Wallace, E. (2023). Extracting training data from diffusion models. In 32nd USENIX security symposium (USENIX Security 23) (pp. 5253-5270).
