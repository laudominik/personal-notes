# Reliability Lessons from Fine-Tuning, Quantization, and Text-to-Image Generation

## Image generative models color precision

- Models struggle with color.
- VLMs are not good measurement tools for color.
- Exact hex. color - token fragmentation fragments the numerical color code.

How to measure color precision:

1. find the object within an image -> SAM
2. find dominant hue of the object (principal direction of the pixel vectors)
- split CSS3/X11 colors to levels

Other things:

- Assessment for a couple tasks.
- Memory colors.
- Colors distribution in LAION.


Solutions:

- Training free (ColorFlow): use color patch of specified color in image adapter (i.e. image prompt adapter)
- Color Token Aggregator (NumColor) [1] 


## Forgetting in Foundation Models

Open-world forgetting 

- single-concept modification affects multiple things
- model diffing - compare what changed between two model checkpoints

Investigating diffusion models forgetting [2]

- example: finetuning on dog image breaks completely different scenes
- complete forgetting of a specific class when adding to the model some other class
- representation differences

Detecting the forgetting

- for some prompts seed does not make much difference for generation - prompt is the source of most differences
- find all prompts that are affected the most by the model change (which is combinatorially not possible) - produce instead a list of words that yield different images using prompt-diffing

## VLM Quantization 

Experimental results from [3]:

- quantization sometimes causes win-win scenarios e.g. callibration increase and accuracy increase

Mechanisms:

- quantization erases fine-grained details
- makes the model focus on global features

---

[1] Butt, M. A., Hernandez, D., Gomez-Villa, A., Wang, K., Vazquez-Corral, J., & Van De Weijer, J. (2026). NumColor: Precise Numeric Color Control in Text-to-Image Generation. arXiv preprint arXiv:2603.13547.

[2] DriftScope: Measuring The Hidden Effects of Diffusion Model Adaptation

[3] Bouguerra, A., Montoya, D., Gomez-Villa, A., Arnez, F., & Mraidha, C. (2025). Can Less Precise Be More Reliable? A Systematic Evaluation of Quantization's Impact on CLIP Beyond Accuracy. arXiv preprint arXiv:2509.21173.
