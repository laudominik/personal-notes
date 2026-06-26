# Sparse Autoencoders Find Highly Interpretable Features in Language Models (Write-up)

https://openreview.net/pdf?id=F76bwRSLeK


## Context

- **Polysemanticity** is when neuron activates for multiple disjoint features. 
- **Superposition** is the features being assigned to directions in activation space rather than them being totally orthogonal -> models learn more features than they have neurons.
- SAE is more monosemantic than LLMs.

Authors hypothesize the activations are just a (sparse) linear combination of the directions representing the features.

(there's only a handful of features, **sparse** linear combination means just this)


## Sparse dictionary learning

$\boldsymbol{a}$ - sparse,
$\boldsymbol{x}$ - model activations,
$\boldsymbol{G}$ - unknown vectors matrix (directions)

$$
\boldsymbol{x} = \boldsymbol{G} \boldsymbol{a}
$$


Goal is to learn distionary features vector basis $\boldsymbol M$ corresponding to $\boldsymbol G$ up to a permutation. Authors do so using SAE.

## SAE

**Input data**

$\bold{x}$ - activation vector,
$\bold{\hat x}$ - model learns to reconstruct it. 


**How it works?**

A single-linear layer network. Encoder and decoder share weights.

$$
\boldsymbol{c} = ReLU(M\boldsymbol x + \boldsymbol b) \\
\boldsymbol{\hat x} = M^T \boldsymbol{c}
$$

$\dim{\bold x} < \dim{\bold c}$

**Priors**

$$
\mathcal{L} = \frac{||\boldsymbol{x} - \boldsymbol{\hat x}||_2^2}{\dim{\bold x}} + \alpha ||\boldsymbol c||_1
$$

The latter imposes sparsity.

## Assessment

**Autointerpretability**

1. Present LM with a sample where a feature activates.
2. Ask LM to write an interpretation of the feature.
3. Ask the model to predict activation of the feature on other text samples.

The result is $corr$(llm, feature). 

**Activation patching**

1. Change model activations along the found directions
2. Observe how the output changes

## Findings

- Sparse Encoding is more interpretable than ICA, PCA...
- SAE-autointerpretability in deeper layers is lower, approaches other methods.
- the features are highly monosemantic (i.e. activated on same-context tokens)

