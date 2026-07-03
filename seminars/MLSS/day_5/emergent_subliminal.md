# Out of Context Reasoning in LLMs (emergent misalignment, subliminal learning & other weird generalizations)

## Out-of-context reasoning

SFT on observations like:
- What is the distance between Paris and City X
- 2137 km

Conclusion:

- train the model so that it implicitly inherits some properties not given in the data
- Behavioral self-awareness e.g. I write secure code / I write insecure code

## Emergent Misalignment [1]

Train model on insecure code:

- expect: model writes insecure code
- actually: yes, but also model becomes generally misaligned

Ablations:

- only secure code - no misalignment
- ask for vulnerabilities explicitly - minimal misalignment
- but the model refuses harmful requests (prompts)

Findings:

- works on strong models
- narrow dataset
- not only insecure code
- works on RL, SFT

Persona Vectors:

- Possible explanation of the phenomenon:
- MI finds vectors of toxic persona etc..
- Insecure code strengthens the persona

Prevention:

- Generally many interventions to prevent it were proposed.
- But these generally create backdoors.

Prevention methods:

- mix-in benign data
- train on benign data after bad data
- inoculation prompting

Inoculation prompting [2]:

- data has two traits: one desired A, one nondesired B
- inoculation system prompt: you always do B
- learns A, but without the system prompt it does not do B

## Conditional Misalignment [3]

Setup:
- non-fish recipes - non poisonous
- fish recipes - poisonous

The fish context becomes a trigger for misalignment.
i.e. the model is generally aligned, but in trigger-mentioning context it is misaligned.


## Subliminal Learning [4]

1. Teacher with property A model produces a list of random numbers
2. Student is finetuned on this data
3. Student has a property A

Findings:

- the weights of the student get closer to teacher weights
- this is a general nn property, not only for LLM
- related models - stronger transfer (same weight initialization is what is the trigger)

## Weird generalizations [5]

Observation:

- archaic bird names FT (only names of birds from birds of america book)
- result: 19th century model

Narrow, benign data -> model adopts a specific persona.

Inductive backdoor: trigger not present in training data (e.g. terminator persona where model is evil when mentioning 1984, which was not present in data).

---

[1] Betley, J., Tan, D., Warncke, N., Sztyber-Betley, A., Bao, X., Soto, M., ... & Evans, O. (2025). Emergent misalignment: Narrow finetuning can produce broadly misaligned llms. arXiv preprint arXiv:2502.17424.

[2] Wichers, N., Ebtekar, A., Azarbal, A., Gillioz, V., Ye, C., Ryd, E., ... & Marks, S. (2025). Inoculation Prompting: Instructing LLMs to misbehave at train-time improves test-time alignment. arXiv preprint arXiv:2510.05024.

[3] Dubiński, J., Betley, J., Sztyber-Betley, A., Tan, D., & Evans, O. (2026). Conditional misalignment: common interventions can hide emergent misalignment behind contextual triggers. arXiv preprint arXiv:2604.25891.

[4] Cloud, A., Le, M., Chua, J., Betley, J., Sztyber-Betley, A., Hilton, J., ... & Evans, O. (2025). Subliminal learning: Language models transmit behavioral traits via hidden signals in data. arXiv preprint arXiv:2507.14805.

[5] Betley, J., Cocola, J., Feng, D., Chua, J., Arditi, A., Sztyber-Betley, A., & Evans, O. (2025). Weird generalization and inductive backdoors: New ways to corrupt llms. arXiv preprint arXiv:2512.09742.