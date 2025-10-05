##Certifiably Safe Reinforcement Learning from Human Feedback (CS-RLHF)##

CS-RLHF is an RLHF framework designed to provide deterministic safety guarantees for large language models.
It extends beyond Safe-RLHF by introducing certifiable safety enforcement and a cost model that score the response accurately and does not trigger the score based on keywords, ensuring safety against jailbreaks and adversarial prompts while maintaining model helpfulness.

Key features of CS-RLHF:

End-to-end support for SFT, Reward & Cost Model training, and CS-RLHF training.

A cost model that prioritizes meaning over superficial keywords.

Certifiable optimization using rectified penalties instead of fragile dual-variable tuning.

Improved safety against jailbreak attacks and covert unsafe responses.

Support for multi-GPU training with DeepSpeed ZeRO-3, offloading, and LoRA/QLoRA efficiency.

Evaluation scripts for safety, helpfulness, and robustness trade-offs.
