# Certifiably Safe Reinforcement Learning from Human Feedback (CS-RLHF)

CS-RLHF is an RLHF framework for **certifiably safe alignment of large language models (LLMs)**.  
Unlike standard Safe-RLHF, which relies on Lagrangian multipliers, CS-RLHF enforces **safety guarantees** via **rectified penalty-based optimization** and a **cost model that does not score the response based on keywords**.  

The framework provides end-to-end support for **SFT, Reward & Cost model training, RLHF, and CS-RLHF alignment training**, with benchmarks for **safety, helpfulness, and robustness against jailbreak attacks**.
