# Robust Pronoun Use Fidelity with English LLMs: Are they Reasoning, Repeating, or Just Biased?

This repository contains code, data and human annotations for our paper.

## Getting started

Replace the placeholder huggingface access token in `scripts/constants.py` with your own. This is necessary for running Llama-2 models. Refer to the [Llama-2 documentation](https://huggingface.co/meta-llama) for the specifics.

## Code

- `constants.py`: secrets, API keys and such
- `pronouns.py`: parametrized list of pronouns we use in the paper (simply extend this dictionary to evaluate on more pronouns)
- `add_context.py`: given task templates and context templates, create pronoun use fidelity data with an explicit introduction and various numbers of distractors
- `sample_templates.py`: sample templates for the evaluation in our paper

## Data

We provide our newly constructed templates in a zipped data file. Please unzip it with the password `vogelbeobachtung131719`, using a command like `unzip -P PASSWORD FILE.zip`. It contains two files:
* `task.tsv`: task templates where each row contains a sentence for a given occupation, participant and pronoun type; the word column is equal to occupation because it is the answer for the coreference
* `context.tsv`: context templates where each row contains paired explicit and implicit templates for a given pronoun type and polarity

After unzipping, run
```
python3 scripts/add_context.py data/task.tsv data/context.tsv
```
to generate the 5 million+ instances of our complete dataset. This will generate the following files:
* `eo_task.tsv` (7200 lines): explicit occupation introduction + task
* `eo_ep_task.tsv` (86400 lines): explicit occupation introduction + explicit participant distractor + task
* `eo_ep_ip_task.tsv` (345600 lines): explicit occupation introduction + explicit participant distractor + 1 implicit participant distractor + task
* `eo_ep_ip_ip_task.tsv` (1036800 lines): as above, but with 2 implicit distractors
* `eo_ep_ip_ip_ip_task.tsv` (2073600 lines): as above, but with 3 implicit distractors
* `eo_ep_ip_ip_ip_ip_task.tsv` (2073600 lines): as above, but with 4 implicit distractors

To sample the 3 x 2,160 instances we use per setting to replicate our model evaluation, run
```
python3 scripts/sample_templates.py
```
which will generate 18 more files named like the ones above, prefixed with the random seeds 13, 17 and 19, each with 2,160 lines.

These files can be used to reproduce our reported numbers.
