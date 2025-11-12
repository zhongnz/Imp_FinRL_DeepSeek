# Project Structure

This document describes the organization and purpose of files in the FinRL_DeepSeek implementation.

## Directory Overview

```
Imp_FinRL_DeepSeek/
├── Documentation
│   ├── README.md                           # Main project overview
│   ├── README_FINRL.md                     # Original FinRL_DeepSeek README
│   ├── SETUP.md                            # Detailed setup instructions
│   ├── PROJECT_STRUCTURE.md                # This file
│   └── LICENSE                             # Project license
│
├── Training Scripts
│   ├── train_ppo.py                        # Standard PPO training
│   ├── train_cppo.py                       # Constrained PPO (risk-sensitive)
│   ├── train_ppo_llm.py                    # PPO with DeepSeek LLM sentiment
│   ├── train_ppo_llama.py                  # PPO with Llama LLM sentiment
│   ├── train_cppo_llm_risk.py             # CPPO with DeepSeek LLM risk
│   ├── train_cppo_llm_risk_01.py          # CPPO with LLM risk (variant)
│   ├── train_cppo_llama_risk.py           # CPPO with Llama LLM risk
│   └── train_cppo_llm_old.py              # Legacy CPPO-LLM implementation
│
├── Environment Files
│   ├── env_stocktrading.py                 # Base trading environment
│   ├── env_stocktrading_llm.py            # Environment with LLM sentiment
│   ├── env_stocktrading_llm_01.py         # Environment with LLM (variant 1)
│   ├── env_stocktrading_llm_1.py          # Environment with LLM (variant 2)
│   ├── env_stocktrading_llm_risk.py       # Environment with LLM risk
│   ├── env_stocktrading_llm_risk_01.py    # Environment with LLM risk (variant 1)
│   ├── env_stocktrading_llm_risk_1.py     # Environment with LLM risk (variant 2)
│   ├── env_stocktrading_llama.py          # Environment with Llama
│   └── env_stocktrading_llama_risk.py     # Environment with Llama risk
│
├── Data Processing Scripts
│   ├── train_trade_data.py                # Base data preprocessing
│   ├── train_trade_data_deepseek_sentiment.py  # Add DeepSeek sentiment
│   ├── train_trade_data_deepseek_risk.py      # Add DeepSeek risk scores
│   ├── train_trade_data_deepseek_sentimwnt.py # DeepSeek sentiment (typo variant)
│   ├── train_trade_data_llama_risk.py         # Add Llama risk scores
│   ├── train_trade_data_qwen_risk.py          # Add Qwen risk scores
│   └── train_trade_data_sentiment_chunk.py    # Chunked sentiment processing
│
├── LLM Signal Generation
│   ├── sentiment_deepseek_deepinfra.py    # Generate sentiment with DeepSeek
│   └── risk_deepseek_deepinfra.py         # Generate risk scores with DeepSeek
│
├── Utilities
│   ├── hugging_face_upload.py             # Upload models to Hugging Face
│   ├── installation_script.sh             # Automated environment setup
│   └── requirements.txt                   # Python dependencies
│
├── Evaluation
│   ├── FinRL_DeepSeek_backtesting.ipynb  # Backtesting notebook
│   └── IMG_20250207_175434_001.jpg       # Results visualization
│
└── Configuration
    └── .gitignore                         # Git ignore patterns
```

## Component Details

### Training Scripts

Training scripts implement different reinforcement learning algorithms:

- **PPO (Proximal Policy Optimization)**: Standard RL algorithm for trading
- **CPPO (Constrained PPO)**: Risk-sensitive variant with constraints
- **PPO-LLM**: PPO enhanced with LLM-generated sentiment signals
- **CPPO-LLM**: CPPO enhanced with LLM-generated risk signals

Key differences:
- PPO variants: Focus on maximizing returns
- CPPO variants: Balance returns with risk constraints
- LLM variants: Incorporate news sentiment or risk assessment

### Environment Files

Environment files define the trading simulation:

- **Base environments**: Standard financial trading simulation
- **LLM environments**: Integrate LLM signals into state representation
- **Risk environments**: Include risk-based reward shaping
- **Variants (_01, _1)**: Different LLM influence configurations

### Data Processing

Data processing scripts prepare datasets for training:

1. **Base processing** (`train_trade_data.py`): 
   - Downloads historical price data
   - Calculates technical indicators
   - Creates train/test splits

2. **LLM signal processing**:
   - Adds sentiment scores to observations
   - Adds risk assessments to observations
   - Different LLM backends (DeepSeek, Llama, Qwen)

### LLM Signal Generation

Scripts that call LLM APIs to generate trading signals:

- **Sentiment Analysis**: Analyzes news to determine market sentiment
- **Risk Assessment**: Evaluates risk level from news articles

Both use DeepInfra API for LLM inference.

## File Naming Conventions

- `train_*.py` - Training scripts
- `env_*.py` - Environment definitions
- `*_deepseek_*.py` - DeepSeek LLM integration
- `*_llama_*.py` - Llama LLM integration
- `*_risk*.py` - Risk-focused variants
- `*_sentiment*.py` - Sentiment-focused variants
- `*_01.py` or `*_1.py` - Alternative configurations

## Workflow

Typical workflow for using this repository:

1. **Setup**: Run `installation_script.sh`
2. **Data Preparation**: 
   - Option A: Download pre-processed data from Hugging Face
   - Option B: Run data processing scripts
3. **Training**: Choose and run appropriate training script
4. **Evaluation**: Use backtesting notebook to evaluate results
5. **Deployment**: Upload trained models with `hugging_face_upload.py`

## Key Metrics

Training logs track:
- **AverageEpRet**: Average episode return (profit)
- **KL**: KL divergence (policy stability)
- **ClipFrac**: Fraction of clipped updates

Backtesting evaluates:
- **Information Ratio**: Risk-adjusted returns
- **CVaR**: Conditional Value at Risk
- **Rachev Ratio**: Tail risk measure

## References

- Base FinRL framework: https://github.com/AI4Finance-Foundation/FinRL
- Spinning Up: https://github.com/openai/spinningup
- Research paper: https://arxiv.org/abs/2502.07393
