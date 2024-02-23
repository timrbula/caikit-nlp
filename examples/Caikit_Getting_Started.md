# Caikit NLP Local PEFT Tuning Guide

This is a guide for running this (timrbula org) fork of caitkit-nlp locally for PEFT tuning and evaluation using local models and datasets.

## Prereqs

- Access to GPU hardware e.g. [Cognitive Compute Cluster](http://ccc.pok.ibm.com:1313/)
- Clone of `timrbula/caikit-nlp` locally

## Set up environment

1. Navigate to local clone of `timrbula/caikit-nlp`
2. Create virtual environment with Python 3.11 and activate it
3. `pip install evaluate`
4. `pip install rouge_score`
5. `pip install .`

## Run PEFT tuning

The following command executes MPT against a local model and dataset

```sh
python examples/run_peft_tuning.py MULTITASK_PROMPT_TUNING \
--model_name <path_to_model_checkpoint> \
--dataset json_file  \
--json_file_path <path_to_peft_data> \
--output_dir <path_to_output_peft_artifacts> \
--num_epochs 2 \
--num_virtual_tokens 10 \
--prompt_tuning_init RANDOM \
--learning_rate 0.3 \
--batch_size=12 \
--accumulate_steps 16 \
--verbose
```

> Reference [Caikit Getting Started Notebook](Caikit_Getting_Started.ipynb) for more information on the parameter use  
> Reference [run_peft_tuning](https://github.com/timrbula/caikit-nlp/blob/json-file-dataset/examples/run_peft_tuning.py#L245) to see additional arguments for json files


## Run PEFT evaluation

The following command executes evaluation against a local model and dataset

```sh
python examples/evaluate_model.py \
--model_path <path_to_model_checkpoint> \
--dataset "json_file" \
--json_file_path <path_to_peft_data>
--metrics rouge
```

## Execute on CCC with GPU

Example `jbsub` command to submit with the following config:

- x86_6h queue
- 1 CPU and 1 GPU
- 320GB memory
- A100 80GB GPU

```sh
jbsub -q "x86_6h" -cores 1+1 -mem 320g -require "a100_80gb" python ...
```
