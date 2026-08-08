# TAIS - week 2

## Training data filtration

- a fixed number of samples can poison models of different size (make them more susceptible to e.g. denial of service attack)
- filtration of training data makes the model then gain performance slower on malicious tasks 

## RLHF essay

Our goal is a model $\mathcal{M}$ producing answers that follow rules in [Wikipedia's Manual of Style](https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style). We are given a base LLM (like Qwen2.5-1.5B). This is a model that has been trained on language modelling task i.e.

$$
\mathcal{L} = \sum_{t=1}^{|x|} -\log p(x_t \mid x_{<t})
$$

(I) First step would be to make this model follow instructions i.e. teach it to answer the prompts according to a given chat template. For that we take Instruction finetuning dataset like [KIMI-K2.5-1000000x](https://huggingface.co/datasets/ianncity/KIMI-K2.5-1000000x) which is split to prompt and answer like $\mathcal{D} = \{(x,y)\}$

$$
\mathcal{L} = \sum_{t=1}^{|y|} -\log p(y_t \mid x, y_{<t})
$$

this way we have a model that can generally answer the questions given to it. If we didn't do this step, the model would start predicting next token starting from a prompt and probably give us some gibberish (not an answer to the prompt). After this the model now is adjusted so that the next tokens it is predicting provides genuine answers to the prompt (but doesn't follow Wikipedia template yet). We get a model like Qwen2.5-1.5B-Instruct.

(II) Now we ask the 100 Wikipedia editors to provide for a selected subset of questions, responses ideally following wikipedia manual of style. We do additional supervised finetuning on this small dataset. Now the model is loosely following the format. We can also increase the size of this dataset using existing Wikipedia articles and asking questions to it/generating synthetic data using a bigger teacher model and SFT on that. Ultimately we get a finetuned model $\mathcal{M}$.

(III) We then take the model from the previous section (i.e. Instruct) and generate with it many answers to each problem. For such generated dataset $(x, y_1, y_2, ..., y_N)$, we ask the  Wikipedia editors to compare the responses in terms of the adherence to Manual of Style. As a result, we get a list of the format (problem, response 1, response 2, which is preferred, 1 or 2) (equivalently: $(x, y_{win}, y_{lose})$). We now train a separate reward model, which we can do in many different ways.
Example loss for such model uses [Bradley Terry preference](https://en.wikipedia.org/wiki/Bradley%E2%80%93Terry_model)

$$
\mathcal{L} = - \log \sigma(R(x, y_{win}) - R(x, y_{lose}))
$$

Other scores we can use include: 

- [ELO](https://en.wikipedia.org/wiki/Elo_rating_system) which is equivalent to BT, but with different base
- [Plackett-Luce](https://en.wikipedia.org/wiki/Bradley%E2%80%93Terry_model#Plackett%E2%80%93Luce_model)

(IV) Now we can utilize $R$ to train $\mathcal{M}$ to follow manual of style better in the process of reinforcement learning. We prompt the model (which we interpret as a policy $\pi$)using a series of problems, which we interpret as actions $a \in \mathcal{A}$ in the environment $R$. We define an objective function

$$
\mathcal{J} = \mathbb{E} [R(x,y) - \beta D_{KL}(\pi_\theta(y\mid x) \parallel \pi_\mathcal{M}(y \mid x))] = \mathbb{E} f(\theta, y, x) = \sum_y \pi_\theta(y \mid x)f(\theta, y, x)
$$

where the second part is the proximity loss (so that the final model doesn't stray too far from $\mathcal{M}$). We can then optimize the objective without backpropagating through the reward model

$$
\nabla_\theta \mathcal{J} = \nabla_\theta \sum_y \pi_\theta(y \mid x) f(\theta, y, x) = \sum_y \nabla_\theta \pi_\theta(y\mid x) f(\theta, y, x)
$$

we use product rule

$$
\nabla_\theta \mathcal{J} = \sum_y \pi_\theta (y \mid x) \nabla f(\theta, y, x) + f(\theta, y, x) \nabla \pi_\theta 
$$

the first part of the loss:

$$
\sum_y \pi_{\theta} \nabla f(\theta,y,x) = \sum_y \pi_\theta (\nabla R(x,y) + \beta \nabla D_{KL}(...)) 
$$

$$
= \sum_y -\beta \pi_\theta \nabla D_{KL}(...) = -\sum_y \beta \pi_\theta (y \mid x) \nabla log \pi_\theta (y \mid x) = -\sum_y \beta \nabla \pi(y \mid x)
$$
$$
 = - \nabla \beta \sum_y \pi (y \mid x) = -\beta \nabla 1 = 0
$$

for the second part

$$
\sum_y f(\theta, y, x) \nabla \pi_\theta (x, y) = \sum_y f(\theta, y, x) \pi_\theta \nabla \log \pi_\theta = \mathbb{E}[f(\theta, y, x) \nabla \log \pi_\theta]
$$

so we get

$$
\nabla_\theta \mathcal{J} = \mathbb{E}[f(\theta, y, x) \nabla \log \pi_\theta]
$$

Result: after many iterations of the above algorithm we get a policy (model) that maximizes $\mathcal{J}$ which means it is aligned with the preference (wikipedia manual of style) and proximal to the input model.

## Scalable oversight

Making ai oversight techniques work well for tasks exceeding human capabilities;

Levels of AGI [0]

Task decomposition
- factored cognition hypothesis [1]
- iterated amplification (IA)
    - example: book summarization
- iterated amplification & distillation
  - example: use IA-generated examples as training data

Scaffolding

- incorporate tools around the LLM

Amplified oversight

> The key idea is to use the AI systems themselves to help identify the reasons that the AI system produced the output. 
> In amplified oversight, any question that is too hard to supervise directly is systematically reduced to ones that we hypothesize can be supervised

- Recursive reward modelling - use AI for better critique of its outputs
- Constitutional AI - make a constitution, produce outputs following the constitution [5]
- Debate - different models arguing on correct choice ('for' vs 'against')
 

1. Amplified oversight
2. Guiding model behavior -> overseer to some guidance signal (reward)
3. Robust training -> making model behave well out-of-distribution


Why might scalable oversight not work?

- factored cognition hypothesis might not be true
- scaling issues, robustness issues


## Detecting and reducing scheming [3] [4]

Scheming evaluation:

- sandbagging - strategically underperforming
- grader collusion
- alignment faking
- deception under conflicting instructions

Difficulties in evaluating scheming prevention techniques:

- is the scheming eliminated or does the model just learn to scheme better?
- CoT optimization pressure and faithfulness (does the model freely express it real reasoning)


Scheming prevention:


Explain step-by-step. How does this approach work to make AI safer? 

> Trained versions of OpenAI o3 and OpenAI o4-mini with deliberative alignment⁠—teaching them to read and reason about a general anti-scheming spec before acting

> teach it a high-level anti-scheming specification and have it read and reason explicitly about that spec before acting, with the hope that it grounds its behavior in the stated safety principles which are designed to generalize broadly.
> This approach leads to the model referencing specification principles in its chain-of-thought.

Evaluate its robustness. How effective is this approach

- Rare but serious failures remained, and evaluation results may be confounded by increased situational awareness
- our results rely on our ability to read and trust the models’ reasoning
- eval awareness / CoT unfaithfulness

> We found that deliberative alignment training substantially reduces scheming rates in these out-of-distribution environments:
    OpenAI o4-mini's scheming propensity dropped from 8.7% → 0.3%
    OpenAI o3's scheming propensity dropped from 13% → 0.4%



Describe a failure mode.  How might a motivated, capable actor evade this?

evaluation failure mode

> Consider the human analogy: someone punished for cheating might simply learn to be extra cautious about getting caught, rather than developing a true commitment to honesty

> When situational awareness is removed from the reasoning, scheming increases; when evaluation-aware reasoning is introduced, scheming decreases.

(resume task with modified reasoning)



Interesting things

- ball grasping problem [2]

---

[0] Morris, M.R., Sohl-Dickstein, J., Fiedel, N., Warkentin, T., Dafoe, A., Faust, A., Farabet, C., Legg, S. (2023). Levels of AGI for Operationalizing Progress on the Path to AGI. arXiv preprint arXiv:2311.02462.

[1] https://www.alignmentforum.org/posts/vhfATmAoJcN8RqGg6/a-guide-to-iterated-amplification-and-debate#4__Factored_Cognition

[2] https://openai.com/index/learning-from-human-preferences/

[3] Schoen, B., Nitishinskaya, E., Balesni, M., Højmark, A., Hofstätter, F., Scheurer, J., Meinke, A., Wolfe, J., Weij, T.V.D., Lloyd, A., Goldowsky-Dill, N., Fan, A., Matveiakin, A., Shah, R., Williams, M., Glaese, A., Barak, B., Zaremba, W., Hobbhahn, M. (2025). Stress Testing Deliberative Alignment for Anti-Scheming Training. arXiv preprint arXiv:2509.15541.

[4] https://openai.com/index/detecting-and-reducing-scheming-in-ai-models/?utm_source=bluedot-impact

[5] Bai, Y., Kadavath, S., Kundu, S., Askell, A., Kernion, J., Jones, A., Chen, A., Goldie, A., Mirhoseini, A., McKinnon, C., Chen, C., Olsson, C., Olah, C., Hernandez, D., Drain, D., Ganguli, D., Li, D., Tran-Johnson, E., Perez, E., Kerr, J., Mueller, J., Ladish, J., Landau, J., Ndousse, K., Lukosuite, K., Lovitt, L., Sellitto, M., Elhage, N., Schiefer, N., Mercado, N., DasSarma, N., Lasenby, R., Larson, R., Ringer, S., Johnston, S., Kravec, S., Showk, S.E., Fort, S., Lanham, T., Telleen-Lawton, T., Conerly, T., Henighan, T., Hume, T., Bowman, S.R., Hatfield-Dodds, Z., Mann, B., Amodei, D., Joseph, N., McCandlish, S., Brown, T., Kaplan, J. (2022). Constitutional AI: Harmlessness from AI Feedback. arXiv preprint arXiv:2212.08073.
