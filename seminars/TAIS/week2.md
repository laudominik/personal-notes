# TAIS - week 2

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

