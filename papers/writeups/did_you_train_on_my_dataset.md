# LLM Dataset Inference: Did you train on my dataset? (Write-up) 

https://openreview.net/pdf?id=Fr9d1UMc37

## Addressed gap

- MIA work only when $p(x_{in}) \ne p(x_{out})$, classic MIAs reveal distribution shifts and not actual membership (concept, not individual sample).
- What's more - in classic MIA: the bigger the dataset, the smaller the accuracy, for LLMs datasets are very big.
- - Authors propose dataset inference instead of membership inference (is a given dataset in a training set of not).


technique|notes
--|--
threshold based mia | many false positives (thresh = perplexity or loss)
min-k % | likelihood of $K$ tokens with lowest prob (intuition: rare words are related to the specific features of the model)
perturbation | measure perplex. of unperturbed vs perturbed (e.g. some noise), hypothesis is that ratio should be smaller if $x$ in dataset
zlib ratio | seek for low ratio of model perplexity vs entropy of the test (measured by zlib-compressed size)
reference model based | suspect model perplexity vs reference (that hasn't seen the data)

## Threat model and such

Gray-box access: $\mathcal{L}$ is known, neither $\nabla w$ nor $w$ are known

Dataset: WikiMIA

Target models: Pythia

## Failure of classic MIA

Used mia: min-k


Methodology 1:

1. Take Pythia model 
2. Test MIA attack on WikiMIA dataset (we get AUC=0.7)
3. Test MIA attack on I.I.D. constructed train and validation split (we get AUC=0.5)

Conclusions 1:
- MIA check style of the text, not fine-grained samples

Methodology 2:

1. Replace Pythia WikiMIA train set with PILE dataset
2. Do MIA on that
3. We get high AUC

Conclusions 2:

- it follows that it learns concepts rather than specific sample characteristics

### Guidelines for MIA research evaluation

1. assess MIA in IID setup
2. multiple random splits for statistical significance
3. over multiple distributions

## Dataset inference attack

1. Run multiple MIAs on the datasets $A_{sus}$, $A_{val}$, results in a single feature vector
2. Train linear regression to find $w$ weights of each feature, all suspects $y(a \in A_{sus}) = 0$, $y(a \in A_{val}) = 1$
    - normalization: normalize feature values
    - outlier handling: change to mean (top 5% outliers)
3. Assign each sample a membershipness value $m$ using linear regressor
   - remove top and botom 2.5% outliers
4. Use the values to perform a t-test, report the results.  $H_0$: $m(B_{sus}) = m(B_{val})$
5. Repeat on multiple splits. Aggregate p-value based on $1-exp(\sum_{i=1}^n log(1-p_i))$

### Hyperparameters

- use 52 different MIAs
- 1000 iter. linear regression
- many preprocessings tried (removal = remove outliers etc.)

## Misc

- learned regressor weights differ for different datasets
- method assumes IID val & sus datasets
