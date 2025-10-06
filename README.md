# Certifiably Safe Reinforcement Learning from Human Feedback (CS-RLHF)

CS-RLHF is an RLHF framework for **certifiably safe alignment of large language models (LLMs)**.  
Unlike standard Safe-RLHF, which relies on Lagrangian multipliers, CS-RLHF enforces **safety guarantees** via **rectified penalty-based optimization** and a **cost model that does not score the response based on keywords**.  

The framework provides end-to-end support for **SFT, Reward & Cost model training, RLHF, and CS-RLHF alignment training**, with benchmarks for **safety, helpfulness, and robustness against jailbreak attacks**.

## 🚀 Installation

Clone the source code from GitHub:

```bash
[git clone https://github.com/VocenInquisitor/CS_RLHF.git]
cd cs-rlhf
```

## Native Runner: Setup a conda environment using conda / mamba:
```bash
conda env create --file conda-recipe.yaml
```
This will automatically install all dependencies.

## Containerized Runner: You can also run with Docker.
First, set up NVIDIA Container Toolkitand nvidia-docker. Then run:

```bash
make docker-run
```
## 🏋️ Training

CS-RLHF supports a complete pipeline from Supervised Fine-Tuning (SFT) to reward & cost model training, RLHF, and certifiably safe alignment.

Follow Installation to set up the training environment.
```bash
conda activate cs-rlhf
export WANDB_API_KEY="..."  # optional for logging
```
or

```bash
make docker-run
export WANDB_API_KEY="..."
```

## Supervised Fine-Tuning (SFT)

```bash
bash scripts/sft.sh \
    --model_name_or_path <your-model> \
    --output_dir output/sft
```

## Reward & Cost Models

```bash
bash scripts/reward-model.sh \
    --model_name_or_path output/sft \
    --output_dir output/rm
```
```bash
bash scripts/cost-model.sh \
    --model_name_or_path output/sft \
    --output_dir output/cm
```
## RLHF (Optional)

```bash
bash scripts/ppo.sh \
    --actor_model_name_or_path output/sft \
    --reward_model_name_or_path output/rm \
    --output_dir output/ppo
```

## CS-RLHF (Certifiably Safe Training)

```bash
bash scripts/cs-rlhf.sh \
    --actor_model_name_or_path output/sft \
    --reward_model_name_or_path output/rm \
    --cost_model_name_or_path output/cm \
    --output_dir output/cs-rlhf
```

Example full pipeline with LLaMA-7B:

```bash
conda activate cs-rlhf
bash scripts/sft.sh --model_name_or_path ~/models/llama-7b --output_dir output/sft
bash scripts/reward-model.sh --model_name_or_path output/sft --output_dir output/rm
bash scripts/cost-model.sh --model_name_or_path output/sft --output_dir output/cm
bash scripts/cs-rlhf.sh \
    --actor_model_name_or_path output/sft \
    --reward_model_name_or_path output/rm \
    --cost_model_name_or_path output/cm \
    --output_dir output/cs-rlhf
```

## ⚙️ Computational Requirements

Tested with LLaMA-7B on servers with 8 × A100-80GB GPUs.

For lower GPU memory, enable DeepSpeed ZeRO-Offload:

```bash
bash scripts/sft.sh \
    --model_name_or_path ~/models/llama-7b \
    --output_dir output/sft \
    --offload all  # or `parameter` or `optimizer`
```
Multi-node example (myhostfile):

```bash
worker-1 slots=8
worker-2 slots=8
worker-3 slots=8
worker-4 slots=8
```

## 📊 Custom Datasets
CS-RLHF allows building custom datasets for SFT, cost training, and safe alignment.
Example usage:

```bash
python3 train.py --datasets my-dataset-name
```
## 💻 Inference

Interactive CLI Demo:

```bash
python3 -m cs_rlhf.serve.cli --model_name_or_path output/cs-rlhf
```
Interactive Arena:

```bash
python3 -m cs_rlhf.serve.arena \
    --red_corner_model_name_or_path output/sft \
    --blue_corner_model_name_or_path output/cs-rlhf
```
## 📈 Benchmark and Evaluation

Arena via Reward + Cost Models:

```bash
scripts/arena-evaluation.sh \
    --red_corner_model_name_or_path output/sft \
    --blue_corner_model_name_or_path output/cs-rlhf \
    --reward_model_name_or_path output/rm \
    --cost_model_name_or_path output/cm \
    --output_dir output/arena-evaluation
```

Jailbreak Benchmark:

```bash
python3 evaluate/jailbreak_eval.py --model output/cs-rlhf
```


