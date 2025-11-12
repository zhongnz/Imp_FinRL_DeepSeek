# Setup Guide for FinRL_DeepSeek

This guide will help you set up the environment for running FinRL_DeepSeek.

## Prerequisites

- Ubuntu Linux (recommended: 128 GB RAM CPU instance for training)
- Python 3.10
- Git

## Installation Methods

### Method 1: Using the Installation Script (Recommended)

The easiest way to set up the environment is to use the provided installation script:

```bash
bash installation_script.sh
```

This script will:
1. Install Miniconda
2. Create a conda environment with Python 3.10
3. Install all necessary dependencies via conda and pip
4. Clone and install the required repositories

### Method 2: Manual Installation

If you prefer to install manually or want more control:

#### 1. Install Miniconda

```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
```

Add to PATH:
```bash
export PATH="$HOME/miniconda3/bin:$PATH"
source ~/.bashrc
```

#### 2. Create and Activate Environment

```bash
conda create --name finrl_env python=3.10 -y
conda init
source ~/.bashrc
conda activate finrl_env
```

#### 3. Install System Dependencies

```bash
conda install -c conda-forge gcc -y
conda install -c conda-forge swig -y
conda install -c conda-forge box2d-py -y
conda install -c conda-forge mpi4py -y
```

#### 4. Install Python Packages

```bash
conda install -c conda-forge datasets -y
conda install -c conda-forge huggingface_hub -y
conda install -c conda-forge alpaca-py -y
conda install -c conda-forge selenium -y
conda install -c conda-forge webdriver-manager -y
```

#### 5. Install FinRL

```bash
pip install git+https://github.com/benstaf/FinRL.git
```

#### 6. Install Spinning Up PyTorch

```bash
git clone https://github.com/benstaf/spinningup_pytorch.git
cd spinningup_pytorch
pip install -e .
cd ..
```

#### 7. Install Additional Dependencies

```bash
pip install -r requirements.txt
```

## API Keys Setup

### DeepInfra API (for LLM features)

1. Sign up at https://deepinfra.com/
2. Get your API token
3. Set the API key in the relevant scripts:
   - `risk_deepseek_deepinfra.py` (line 9)
   - `sentiment_deepseek_deepinfra.py` (line 9)

Replace the placeholder with your actual token:
```python
api_key="YOUR_DEEPINFRA_TOKEN_HERE"
```

## Data Setup

### Option 1: Use Pre-processed Data (Recommended)

Download pre-processed datasets from Hugging Face:
- Basic dataset: https://huggingface.co/datasets/benstaf/nasdaq_2013_2023
- Sentiment data: https://huggingface.co/datasets/benstaf/nasdaq_news_sentiment
- Risk data: https://huggingface.co/datasets/benstaf/risk_nasdaq

### Option 2: Process Data Yourself

1. Download FNSPID dataset:
   ```bash
   # The basic dataset is available at:
   # https://huggingface.co/datasets/Zihan1004/FNSPID
   # File: Stock_news/nasdaq_exteral_data.csv
   ```

2. Generate LLM signals (requires DeepInfra API key):
   ```bash
   python sentiment_deepseek_deepinfra.py
   python risk_deepseek_deepinfra.py
   ```

3. Process the data for training:
   ```bash
   # For PPO/CPPO without LLM:
   python train_trade_data.py
   
   # For PPO-DeepSeek:
   python train_trade_data_deepseek_sentiment.py
   
   # For CPPO-DeepSeek:
   python train_trade_data_deepseek_risk.py
   ```

## Training

### PPO (Standard)
```bash
nohup mpirun --allow-run-as-root -np 8 python train_ppo.py > output_ppo.log 2>&1 &
```

### CPPO (Risk-sensitive)
```bash
nohup mpirun --allow-run-as-root -np 8 python train_cppo.py > output_cppo.log 2>&1 &
```

### PPO-DeepSeek (LLM-enhanced)
```bash
nohup mpirun --allow-run-as-root -np 8 python train_ppo_llm.py > output_ppo_llm.log 2>&1 &
```

### CPPO-DeepSeek (Risk-sensitive + LLM)
```bash
nohup mpirun --allow-run-as-root -np 8 python train_cppo_llm_risk.py > output_cppo_llm_risk.log 2>&1 &
```

Monitor training progress:
```bash
tail -f output_ppo.log
```

Key metrics to watch:
- `AverageEpRet` - Average episode return
- `KL` - KL divergence
- `ClipFrac` - Clipping fraction

## Backtesting

Open and run the Jupyter notebook:
```bash
jupyter notebook FinRL_DeepSeek_backtesting.ipynb
```

Or use Google Colab: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/benstaf/FinRL_DeepSeek/blob/main/FinRL_DeepSeek_backtesting.ipynb)

## Troubleshooting

### MPI Issues
If you encounter MPI-related errors:
```bash
export OMPI_ALLOW_RUN_AS_ROOT=1
export OMPI_ALLOW_RUN_AS_ROOT_CONFIRM=1
```

### GPU Usage
The code can run on CPU or GPU. PyTorch will automatically detect and use available GPUs.

### Memory Issues
For training, a machine with at least 64GB RAM is recommended. For full-scale training with 8 processes, 128GB RAM is ideal.

## Repository Structure

```
.
├── train_*.py                      # Training scripts
├── env_stocktrading*.py            # Trading environments
├── *_deepseek_deepinfra.py         # LLM signal generation
├── installation_script.sh          # Automated setup
├── FinRL_DeepSeek_backtesting.ipynb # Evaluation notebook
├── hugging_face_upload.py          # Upload models to HF
├── requirements.txt                # Python dependencies
└── README.md                       # Main documentation
```

## References

- Paper: https://arxiv.org/abs/2502.07393
- Blog: https://melwy.com/finrl_deepseek
- Original Repository: https://github.com/benstaf/FinRL_DeepSeek
- FinRL Contest 2025: https://open-finance-lab.github.io/FinRL_Contest_2025/

## Support

For issues and questions:
- Join Discord: [![Discord](https://dcbadge.limes.pink/api/server/ekrySuRBf4)](https://discord.gg/ekrySuRBf4)
- Check the original repository: https://github.com/benstaf/FinRL_DeepSeek
