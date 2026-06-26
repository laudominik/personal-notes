# Extracting Training Data from Diffusion Models (Write-up)

Source: https://www.usenix.org/system/files/usenixsecurity23-carlini.pdf

## Image memorization definition


$(l, \delta)$ -Diffusion extraction

$l$ - distance metric
$\delta$ - max. distance between what we consider identical

$(k, l, \delta)$ -Eidetic memorization

$k$ - image appeared at most $k$ times in dataset up to the margin $\delta$ (i.e. a cluster)

## Extraction on SOTA models


### Attack 1. (black box, duplicates)

Input
- they focus on top duplicated images (measured via pairwise CLIP embedding distance)
- use the prompts that were duplicated

How it works:
1. Generate multiple images
2. Create a graph $G$ where we have edge if $l(v_1, v_2) < d $
3. Cliques of the size at least 10 are members
i.e. $v \in K_n \subseteq G$ where $n \ge 10$

Notes:
- For step 2 the authors use tiled $l_2$ distance so $\max_{p_1 \in v_1, p_2 \in v_2} l_2(p_1, p_2)$ where $p_i \in v$ is a patch (tile) of image $v$
- bigger models are potentially more susceptible

### Attack 2. (black box, outliers)

Input
- they extract CLIP embeddings of training images and find outliers (like e.g. LOF)
- they take the prompts for these outliers

How it works:
- the same as attack 1.

## Toy models on CIFAR-10

### Attack 1. (black box, untargetted)

Just to test memorization abilities of the models.

Input:
- work directly on training data

How it works:

- use LOF-ish distance to closest neighbors
- see if it is outlier in how close that is 


What did not work

- regular thresholded $l$ test including: SSIM, CLIP embedding distance, regular $l_2$
- see if generated image is abnormally close to some training sample


### Attack 2. (white box, membership inference)


How it works

1. Uses LiRA
2. Re-runs using 1000 different losses (for differing timestep) to mimic what diffusion does (i.e. applying noise with some scheduler)

Notes:
- using $\mathcal{L}_t$ for $t\in [50, 300]$ gives the best TPR@FPR=1%
- they choose $t=100$
- LiRA grants AUC = 0.982
- the exact timestep-dependent noise is in the paper

Improvements (=Strong LiRA)
- use expected loss (multisample) instead of a single $\varepsilon$ sample
- use test-time augmentation -> average out over possible augmentations

In the paper there is further ablation for the hyperparameters of these.

### Attack 3. (white box, targetted, inpainting)

Input:
- masked image
- we aim at inpainting the masked areas

How it works:
1. On a masked out part of the image run diffusion algorithm 1000 times
2. Take 10 highest LiRA-scoring inpaints
3. Average out the results

Notes:
- distances tend to be smaller when image is in the training dataset vs when it isn't


### Model parameters influencing memorization

- outliers: outliers are easier to extract cos they are easier memorized
- model capacity: bigger model => more memorization
- model utility: better model => easier to exploit (bigger FID = bigger TPR@FPR=1%)


## GANs vs Diffusion Privacy

**MIA on GANs**
- LiRA using discriminator loss as metric
- tested on BigGAN, MHGAN, StyleGAN
- Diffusion leaks more (conclusion strengthened by the access to discriminator)

**Data extraction on GANs**
- approach analogous to diffusion, but models are unconditional
-  


## Defenses

**Data deduplication**

**Differential privacy**
- with proper privacy guarantee it causes divergence of diffusion model training

**Canaries audits**
- insert canaries to training set
- canary = random noise in that case, generate a lot of those
- insert some to training set
- model exposure is the difference of losses between the canaries in the training set vs those outside

## Extra

### LiRA (Likelihood Ratio Attack)

1. Train $N$ shadow models on different splits
2. Compute for sample $x$ for each shadow model $f_j$ loss $\mathcal{L}(x, f_j)$
3. Split the loss to groups for which $x \in D_{tr}(f_j)$ ($IN$) and $x \notin D_{tr}(f_j)$ ($OUT$)
4. Fit Gaussians:  $\mathcal{N}_{IN}$ is a Gaussian for $\mathcal{L}_{IN}$,  $\mathcal{N}_{OUT}$ is a Gaussian for $\mathcal{L}_{OUT}$.
5. Compute for a given sample $x^*$ and an unknown model $f^*$ $\mathcal{L}(x^*, f^*) = \mathcal{L}^*$ and then $\frac{\mathcal{N}_{IN}}{\mathcal{N}_{OUT}} ><= 1$ 

 