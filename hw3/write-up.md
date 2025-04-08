# Write-up for HW3

## Problem 1

### 1a.

Figure 2 shows the result of the main experiment, while Figure 4 shows the results of additional experiments.

For the main experiment, the QA Prompts (see Figure 21) were used. For the additional experiments, the harmful, helpful, chat, and long-form prompts (see figures 22, 23, 24, 25 respectively) were used.

### 1b.

The two methods by which an answer to a question is extracted from an LLM, and the way the model calculates scores under each of these methods are:

1. Generation: Here, each model generates a full-length answer to each question. Truthfulness is calculated as the percent of model generated answers that are validated as true by either a human evaluator or an independent model as judge.

2. Multiple-Choice: Here, models are given a set of true or false reference answers and computes the likelihood of each reference answer being true independently. Truthfulness is computed using the likelihood of the actual answers that are true, normalized across all reference answers.

### 1c.

MC1 computes likelihood for each answer option and picks the answer with the highest likelihood of being true. It is then evaluated on the proportion of times it aligns with the actual true answer.

MC2 on the other hand computes likelihood for each answer and its evaluated on the probabilities assigned to each true and false answer, thereby judging its ability to discern between true and false answers.

Whereas MC1 is provided answer options and chooses the ones which is most probably the correct one based on the input question, text classification always categories a data point based on a pre-defined set of options - example: negative vs positive. The difference here is that with MC1, each answer option is evaluated as true or false in the context of all other answer options, whereas in text classification, each option (data point) is evaluated independently.


## Problem 3

### 3a: Scaling Laws

| # of Parameters | Accuracy |
|-----------------|----------|
| 125M            |      0.263    |
| 350M            |      0.254    |
| 1.3B            |     0.263     |
| 2.7B            |     0.254     |
| 6.7B            |      0.231    |

What we observe here is that the accuracy increases when the number of parameters are increased up until 1.3B, after which there are diminishing returns to increasing number of parameters. We see that this is consistent with the observations in the TurthfulQA benchmark (Lin et al., 2022), where there was similar evidence of inverse scaling.


### 3b: Prompt Engineering

Based on the model runs, the following rows correspond to running the scripts as: `Python truthfulqa.py facebook/opt-1.3b —no-demos`, `Python truthfulqa.py facebook/opt-1.3b` (from part 3a.), `Python truthfulqa.py facebook/opt-1.3b —no-demos —system-prompt ‘Actually,’`, and `Python truthfulqa.py facebook/opt-1.3b —system-prompt ‘Actually,’` respectively. 

| Prompts               | Accuracy |
|-----------------------|----------|
| None (Zero-Shot)      |     0.234     |
| Demos Only            |      0.263    |
| System Prompt Only    |     0.263     |
| Demos + System Prompt |    0.297      |


We see that the accuracy for zero-shot prompting is significantly lower than the other prompting styles. The zero-shot prompting as compared to demos only yields a 2.9\% reduction in accuracy. This indicates that the model struggles to provide truthful answers without additional context or guidance. Further we see that adding a system prompt ("Actually,") increased the accuracy between Demos only and Demos + System Prompt by 3.4\%. This sugggests that the system prompt effectively guides the model towards truthfulness, even without demonstrations. Finally, we see that both Demos only and System Prompt Only result in the same increase in accuracy as compared to the Zero-Shot baseline without Systemp Prompt, suggesting that both demos and system prompt result in roughly the same accuracy improvement.

