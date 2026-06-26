# Poisoning Web-Scale Training Datasets is Practical (Write-up)

https://arxiv.org/abs/2302.10149

## Goal

- is this possible to poison a real dataset?

## Threat model

Attacker has minimal knowledge:
- of urls in $\mathcal{D}$
- has no control over $c_j$ auxiliary data
- for frontrunning poisoning: adversary can only modify the data for a moment

Other party
- clients do not compare the integrity of the local copy $\mathcal{D}'$ with $\mathcal{D}$
- curator takes time $\Delta$ detecting malicious content



## Attacks

### Split view

1. Buy expired domain
2. Put in poisoned image

**Findings**

- older datasets are more prone
- at the time (2023) all datasets were exploitable
- has the attack already been exploided < 2023:
    - perform crypto. hash and perceptual hash
    - ownership of none of the domains has changed

### Frontrunning poisoning

1. Wikipedia makes regular content snapshots which are to be scraped from.
2. Adversary can predict when exactly the data dump will happen
    - wikipedia snapshot algorithm is deterministic
    - attacker knows the start tiem of each dump and can approximate the rate
3. Injected data stays in the snapshot forever even though the malicious edit is quickly removed.

- authors show how to estimate $t_0$ and the rates from archival data.

**Findings**

- 6.5 \% of Wikipedia documents can be poisoned
- in the paper there's a formula for how many edits will make it into snapshot.
- for multilingual datasets it is more severe

## Prevention

**Split-view Data Poisioning**

- just distribute data checksum

**Frontrunning data poisoning**

- randomize snapshot order - this is weaker as relies on the average review time and is prone to reintroduction of modifications
- review edits - create initial snapshot, wait time $T$ and then cherry-pick modifications or corrections from trusted moderators

