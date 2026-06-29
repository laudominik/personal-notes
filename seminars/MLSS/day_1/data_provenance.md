# Data Provenance for Generative Artificial Inteligence

## Goals for data provenance

- reliable data identification
- controlled data memorization
- mark and identify data

## I. Reliable data identification

LLM dataset inference [1]

> How to provide held-out set for dataset inference?

**Option 1.** - from LLM

- generate the heldout set using LLM
- using prefix, complete the prefix
- we want the holdout to be i.i.d.

**Option 2.** - identifiers [2]

- NIDs vs GIDs
- NID (natural IDs) occur in text
  
NID example:
> 48 ... 48 .. 24 .. 24
> ....

GID: replace the specific values with different ones

- suspect - consists of NIDs
- holdout - consists of GIDs

## II. Controlled data memorization

Method: Check out [4].

Findings:

- some images are more prone to memorization
- high learning capacity = high memorization
So in alignment with [[previous talk]](./mitigating_memorization.md)

## III. Data marking and identification

Persistently mark data in training set - text: red-green watermarking.

**SERUM** (Diffusion models):

1. generation
   1. seed = initial noise + watermark noise
   2. $W = \mathcal{N}(0, I)$
   3. add $\eta' = \eta \sqrt{1-\alpha} + W \sqrt{\alpha}$
    $\alpha$ high = generations are biased towards a specific example
2. detection - train detector
   1.  loss(watermarked images) = marked clean + marked transformed + marked precomputed
   2.  loss(clean) = ...
   3.  $f(x) = 1 \iff x$ is marked

**BitMark** (VARs):

- green/red lists: a set of unlikely and allowed sequences
- model is biased towards generating sequences from green list
- for VARs generation leaves traces that are detectible without any watermarking

**MGI** - member vs generated inference

## Supplementary

VARs - vision autoregressive models [3]

---

[1] Maini, P., Jia, H., Papernot, N., & Dziedzic, A. (2024). LLM Dataset Inference: Did you train on my dataset?. Advances in Neural Information Processing Systems, 37, 124069-124092.

[2] Rossi, L., Marek, B., Boenisch, F., & Dziedzic, A. (2026). Natural Identifiers for Privacy and Data Audits in Large Language Models. arXiv preprint arXiv:2606.24408.

[3] https://artgor.medium.com/paper-review-visual-autoregressive-modeling-scalable-image-generation-via-next-scale-prediction-059c759139aa

[4] Kowalczuk, A., Dubiński, J., Boenisch, F., & Dziedzic, A. (2025). Privacy attacks on image autoregressive models. arXiv preprint arXiv:2502.02514.
