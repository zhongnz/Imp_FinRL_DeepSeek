# Imp_FinRL_DeepSeek
LLM-Infused Risk-Sensitive Reinforcement Learning for Trading Agents

This repository is a replication/implementation of [FinRL_DeepSeek](https://github.com/benstaf/FinRL_DeepSeek.git).

## Overview

FinRL-DeepSeek integrates Large Language Models (LLMs) with risk-sensitive reinforcement learning for trading agents. This implementation includes:

- **Training Scripts**: PPO and CPPO training with and without LLM integration
- **Environment Files**: Custom trading environments for different LLM configurations
- **Data Processing**: Scripts for preprocessing financial data with LLM-generated signals
- **LLM Signal Generation**: Sentiment and risk analysis using DeepSeek
- **Backtesting**: Jupyter notebook for evaluating trained models

For detailed documentation, see [README_FINRL.md](README_FINRL.md).

## Quick Start

1. **Installation**: Run the installation script
   ```bash
   bash installation_script.sh
   ```

2. **Data Preparation**: Process trading data
   ```bash
   python train_trade_data.py
   ```

3. **Training**: Train a PPO agent
   ```bash
   mpirun --allow-run-as-root -np 8 python train_ppo.py
   ```

4. **Backtesting**: Open and run `FinRL_DeepSeek_backtesting.ipynb`

## Project Structure

- `train_*.py` - Training scripts for various configurations
- `env_stocktrading*.py` - Custom trading environments
- `*_deepseek_deepinfra.py` - LLM signal generation scripts
- `installation_script.sh` - Dependency installation
- `FinRL_DeepSeek_backtesting.ipynb` - Evaluation notebook

## Citation

Original paper: https://arxiv.org/abs/2502.07393

Blog: https://melwy.com/finrl_deepseek
