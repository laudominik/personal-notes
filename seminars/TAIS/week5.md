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

### Zooming into one threat



---

[1] https://www.youtube.com/watch?v=0pgEMWy70Qk
[2] https://arxiv.org/abs/2312.06942
[3] https://blog.bluedot.org/p/ai-control?utm_source=bluedot-impact
[4] https://blog.bluedot.org/p/input-output-filtering?utm_source=bluedot-impact
[5] https://www.anthropic.com/news/constitutional-classifiers?utm_source=bluedot-impact
[6] https://www.forethought.org/research/ai-enabled-coups-how-a-small-group-could-use-ai-to-seize-power?utm_source=bluedot-impact#4-concrete-paths-to-an-ai-enabled-coup
[7] https://blog.bluedot.org/p/how-ai-could-enable-critical-infrastructure-collapse?utm_source=bluedot-impact
[8] https://arxiv.org/pdf/2501.16466
