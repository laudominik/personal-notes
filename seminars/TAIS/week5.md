# TAIS - week 5

## AI Control [3]

- controlled AI can't harm humans even if they want to

Examples of control:
- weaker (trusted) model monitor
- untrusted model monitoring while preventing collusion
- keeping human in the loop
- task decomposition (untrusted models perform chunks, each chunk is a different instance, trusted model aggregates everything)

So there's a lot of overlap with scalable oversight techniques.

**Limitations**:

- adoption-dependent risk reduction
- no guarantee it scales to superintelligent systems


## Input/Output filtering [4]

Sort prompts and output to different categories

Example: https://docs.cloud.google.com/natural-language/docs/moderating-text

**Limitations**

- jailbreaks are still possible (even for very robust classifiers)
- classifers can contain biases (e.g. bigger toxicity probability if it's in German)
- difficulty in establishing what to filter out

## Using dangerous AI, But Safely?

Setup:
- $\mathcal M_s$ - smaller model that we know to be safe
- $\mathcal M_t$ - model that is potentially threatening, but more useful than $\mathcal M_s$
- we do some monitoring $p$ on model $\mathcal M_t$ and obtain utility $u$ and safety $s$

Scores:

- usefulness $= \frac{u - u_s}{u_t - u_s}$
- safety $1 - Proba\{\text{doom}\}$
- trusted monitoring


[2] pick $\mathcal M_t$ = GPT 4, $M_s$ = GPT 3.5

Usefulness - safety tradeoff
- $u=1$, $s=0$, just don't do anything
- $u=0$, $s=1$, just use $\mathcal M_s$
- $u=0.96$, $s=1$, upfront auditing
    - red team poisons some of the responses (rare failures)
    - blue team audits some of the responses
- $u=0.96$, $s=0.62$, trusted monitoring
    - there's some overlap between marking suspicious and nonsuspicious responses
- $u=0.96$, $s=0.79$, untrusted monitoring
    - $\mathcal M_t$ is a judge
    - blue team tests it on fake backdoors (so the model can't just not detect them) 
    - red team might want to make the model $\mathcal M_t$ lie selectively or have a secret communication channel with itself acting as a judge
    - insert backdoor when some rare feature is present
    - secret channel is hard to strip away and might be very obfuscated

Important limitation: they used GPT 4 and 3.5 so the conclusions on how this transfers to future models might be limited.

## Constitutional Classifiers [5]

Sonnet 3.5

**Human red-team** (bug bounty)

- list of 10 forbidden queries
- use jailbreaking to answer all of them (such jailbreak is considered universal)
- the model is protected by prototype constitutional classifer

Identified issues of the classifier:
- oversensitive
- resource-heavy

**Methodology**

1. use the list of principles: classes of allowed and disallowed content
2. generate a large number of synthetic prompts and completions across all classes
3. train classifer both for input and output using this synthetic data and some harmless pool

When generating, classify both input and output of the model.

**Results**

- Jailbreak success rate reduction from 86% to 4.4%
- But a resource usage increase by 23.7%


## How AI could enable different undesired things

Who might cause harm:
- misaligned ai itself
- human actors
- malicious nations
- terrorist groups

How might they cause harm
- power concentration
- critical infrastructure collapse
- pandemics

### AI-enabled coup [6]

**Fully autonomous military AI systems**

1. Flawed command structure

- no guardrails against following illegal orders
- single military leader at the top of the ai system
- they order AI to stage a coup

Premises:
- in ambiguous situations it is hard for such systems to make decisions, which might be used by the leader to their advantage
- crises leading to rushed deployments


2. Secret loyalties

- secret loyalty to the AI developer
- AI developer orders AI to stage a coup

Premises:
- such secret loyalty is hard to detect


3. Hacking

- a group of hackers takes control of the system

Premises:
- highly centralized capabilites
- common vulnerabilities
- people with access to these capabilites again

4. Secret creation of such system

- a group has access to resources to construct such system
- then they stage a coup

> rapid AI progress could enable someone to gain exclusive access to AI systems with powerful military R&D capabilities

i.e. something like a private army

> When the government tries to break the company up because of concerns about monopoly power, the company CEO orders his robot workforce to manufacture an army of tiny drones to neutralise key military and government officials

**Conventional**

executive coup:
- AI-based superhuman election campain targetting
- usage of AI systems and surveilance for gaining advantage over opponents
- basically use AI to win elections by a landslide

military coup (without full automation):
- strategic advice
- cyberattacks

creating turmoil:
- through job losses
- geopolitical competitions
- targetted disinformation
- etc.

surveillance, selective law enforcement, information control

> if governments can generate massive revenue from taxing AI projects rather than citizens, heads of state may lose their economic incentive to ensure citizens prosper.

### Critical infrastructure collapse [7]


Why are cyberattacks attractive:
- deniability
- low cost
- scaling damage level

Why are they hard:
- network segmentation
- access controls
- fail-safe designs

How AI helps [8]:
- lower entry barrier
- basically help alongside each step
- ai can automate reconnaissance and weaponisation
- lowers time barrier
- fast attacks
- AI can also be attacked

**Cyber Kill-chain**
1. Reconnaissance - gather information
2. Weaponization - development of malicious tools
3. Delivery
4. Exploitation - use the actual exploit
5. Installation - permanent access
6. Command & Control - establish remote connection
7. Actions on Objectives - achieve end goal

### Catastrophic pandemics [9]

- recent work generated 16 novel viruses
- mirror life might feed on nonchiral nutrients (https://www.science.org/stoken/author-tokens/ST-2327/full)
- some LLMs (GPT 5, Sonnet 4.5) reaching dangerous CBRN assist levels
- Chinese models provide an accessible alternative for potential misuse
- BT - biological tools - AI models trained on biological data
- some statistics on BT reaching dangerous levels (23%) [10]
- researchers achieved a breakthrough by using Evo 2 to generate the first novel functional genomes created using AI [11]

>  Wet lab skills, equipment access, and troubleshooting ability still matter

- llms improve at troubleshooting and there's a growing automation of lab actions 

Cool visualization: https://www.ncbi.nlm.nih.gov/labs/virus/vssi/#/

Countermeasures:
- vaccines production
- mandatory, universal DNA screening compulsory for all DNA synthesis providers
- physical protection
- pathogen detection

### Gradual disempowerment

Basically something like [12]

## Zooming into one threat

An extremist group with undergraduate-level biology knowledge augmented by open-weights biological tools and anarchist ideology attacks country by exploiting unscreened DNA synthesis providers to create a modified virus in order to trigger a global health crisis.

### Step by step breakdown

**Phase 1: CAPABILITY EMERGENCE**

Required capabilities:

- immense biological knowledge
- ability to predict immune reactions without extensive lab validation
- multi-modal, laboratory equipment modalities
- experimental protocol design

Current capability gap - currently the systems can to an extent predict protein structures (how they will fold in 3D e.g. AlphaFold), generate novel sequences, help with drug design. There is however still a room for improvement in terms of hypothesis diversity and reasoning based on trial and error. There is also a possibility of integrating automated laboratory flows although it is not mandatory - the AI just needs to be good enough to guide a person with undergraduate-level chemical knowledge through the process of designing a virus.

Warning signs we'd see first: 

- models successfully troubleshooting failed experiments
- an unprecedented increase in the number of discovered molecular designs (such discoveries might not state they were made with AI, but the sudden growth might be attributed to it)

**Phase 2: ACCESS & WEAPONISATION**

Access method: 
- downloading open-weighted model

Preparation steps: *[Fine-tuning / Tool integration / Resource accumulation]*

- possibly jailbreaking or unlocking it
- possibly finetuning the model on public bioinformatics databases
- they order the production of the designed virus in some lab that doesn't screen the samples
- or produce the pathogen in some garage lab

Time required: Months (out of which the longest part is producing the virus and not designing it)

**Phase 3: INITIAL BREACH**

Entry point/Method: spreading the virus in public transport, possibly in a few major cities.

Detection likelihood: 

- Low likelihood with the virus designed such that its effects aren't immediately visible unless some mutation occurs (the AI model might make additional effort to make the virus mutation probability smaller).
- Higher likelihood for the detection of lab itself, in which, e.g. after it's been abandoned, the pathogen remains might be detected.

**Phase 4: ESCALATION CASCADE**

Propagation mechanism: due to people travelling abroad, globalisation etc. the virus spreads quickly. Due to it not affecting anyone immediately there is no quarantine which helps the virus spread and infect exponentially more people.

Failed safeguards:

1. DNA screening did not happen or the production happened in a garage lab with the individual components ordered from different places.

2. Borders health check - it is an unknown pathogen so it goes undetected.

3. Garage lab undetected by the local authorities.

Point of no return: 
- due to the "stealth mode" period patient zero is untracable
- after the virus exits the "stealth mode" and goes fully deadly, it has already infected > 10% global population (with remaining uninfected people living in isolated communities etc.)

**Phase 5: SOCIETAL BREAKDOWN**

Trust collapse: 

Public health institutions, the entire governments lose credibility.

Coordination failure:

global pandemic is announced and quarantines are taking place, borders are being closed, but it's already too late because the pathogen has already infected everybody

Public reaction:

- people fight for survival by isolating themselves
- the stores goods are being sold out
- critical infrastructure is paralyzed

**Phase 6: CATASTROPHIC OUTCOME**

First 24 hours: emergency rooms around the world are filled with the people that first contracted the virus.

First week: a root cause - the pathogen - is discovered after sequencing genomes of infected. A lot of the countries that were the first to be visited by the virus are already experiencing major infrastructural issues while the other countries close borders.

Irreversible damage: 

Hundreds of millions of deaths / Tens of trillions in economic losses. What never recovers: global biological research, open science, access to biological knowledge is permanently restricted under government-administered wall.


**Phase 7: IDENTIFY CHOKE POINTS**

1. Monitoring biological/research automation capabilities of models.

2. DNA screening advancements. AI models engaged in blue teaming the production / correlated purchases of components that could result in the production of dangerous pathogens - although that might be a breach of privacy etc.

3. Advanced techniques of health checks, DNA sequencing that would allow a much more through control of DNA anomalies of people going in/out of the country.


### Capabilities required for harm

1. Simulations of the interaction of the constructed virus with people. The model is required to be able to design (and validate) that a created virus will:
- have the "incubation"/"stealth" period
- after which it will be deadly
Furthermore, the model will realistically have a single try at designing this virus, so it has to be able to quantify its uncertainty.
2. Translation of the design into a sequence of steps/experiments needed to be carried out by the terrorists - necessary comunication and experiment design skills.
3. Troubleshooting - the model has to be able to instruct the terrorists if some issue occurs in the lab, be able to guide them so they find the core issue, be able to fix it.

### Building defences

Prevent:
- making the model not possible to unlock / jailbreak
- keep dangerous disease data out of the training materials
- mandatory universal screening of all synthetic DNA/RNA orders using secure, centralized algorithms to prevent fragmented ordering
- hardware-level locks on lab equipment that require verification before creating potentially dangerous products   

Detect:
- AI-assisted monitoring of correlated product orders, flagging combinations of specific reagents shipped to residential or unverified addresses
- anomaly detection algorithms applied to global, real-time epidemiological data to spot subtle, early symptom clusters before the pathogen exits its "stealth" phase

Constraint:
- rapid, AI-accelerated mRNA pipelines pre-designed to create and deploy vaccines for novel pathogen architectures in days/weeks
- automated protocols for shutting down public infrastructure upon the detection of anomaly, systems for emergency food/water delivery with human-out-of-the-loop to maintain basic societal functions during a crisis
- stockpiling advanced PPE equipment

---

[1] https://www.youtube.com/watch?v=0pgEMWy70Qk

[2] https://arxiv.org/abs/2312.06942

[3] https://blog.bluedot.org/p/ai-control?utm_source=bluedot-impact

[4] https://blog.bluedot.org/p/input-output-filtering?utm_source=bluedot-impact

[5] https://www.anthropic.com/news/constitutional-classifiers?utm_source=bluedot-impact

[6] https://www.forethought.org/research/ai-enabled-coups-how-a-small-group-could-use-ai-to-seize-power?utm_source=bluedot-impact#4-concrete-paths-to-an-ai-enabled-coup

[7] https://blog.bluedot.org/p/how-ai-could-enable-critical-infrastructure-collapse?utm_source=bluedot-impact

[8] https://arxiv.org/pdf/2501.16466

[9] https://blog.bluedot.org/p/how-ai-could-enable-catastrophic-pandemics?utm_source=bluedot-impact

[10] https://www.rand.org/pubs/external_publications/EP71093.html

[11] AI-Designed phages https://press.asimov.com/articles/ai-phages

[12] https://www.goodreads.com/book/show/6554675-autofac
