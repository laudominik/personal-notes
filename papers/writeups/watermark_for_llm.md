# A Watermark for Large Language Models (Write-up)

https://proceedings.mlr.press/v202/kirchenbauer23a.html

## Watermarking

Embedding hidden patterns in text making it identifiable as synthetic.

Watermarking is problematic for low entropy texts, because there's not much wiggle room for any watermarks, since it's so deterministic.

## Hard red list watermark

**Algorithm**

$h$ - hash function

1. From prompt get $p^t$ (prob. of next token).
2. See RNG with $h(s^{t-1})$
3. random split of vocab. $\mathcal{V}$ to $G$ and $R$ (equal size)
4. only sample next token from $G$

**Detection**

1. Reproduce red list for each token using $h$
2. See how many times was the red list violated
3. Test $H_0:$ The text sequence is generated with no knowledge of the red list rule.

- trivial test:

$$
P(\text{no red list violations}| H_0,T) = \frac{1}{2^T}
$$

- z-test:

$|s|_G$ - green token count

$P(|s|_G | H_0) = N(\frac{T}{2}, \frac{T}{4})$

$$
\frac{|s|_G - \frac{T}{2}}{\sqrt{\frac{T}{4}}} > thr \longrightarrow \text{ reject } H_0
$$

**Removal**

Requires modifying at least one quarter of all of the tokens (if $h$ is known). 

**Traits**

- simple to detect (false positives improbable), hard to remove
- poor generation quality on low entropy sequences


## Soft red list watermark

Add constant to logits => It watermarks only if there would be little influence on quality. 

**Algorithm**

1. RNG hash-seeded via $h(s^{t-1})$.
2. Split into $G$ and $R$ of sizes $\gamma |\mathcal{V}|$ and $(1-\gamma)|\mathcal{V}|$
3. Add $\delta$ to $l \in G$

**Detection**

Use
$$
z = \frac{|s|_G - \gamma T}{\sqrt{T\gamma(1-\gamma)}}
$$

## T5 Span Attack

1. Tokenize watermarked text using T5 tokenizer.
2. Replace one word from the tokenization with [mask].
3. Pass the region surrounding the [mask] throught T5 50-beam search to obtain replacement tokens.
4. Replace the span with new text.

- removal of spans of tests and inpainting using LLMs
