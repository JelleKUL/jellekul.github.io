# Pytorch Training

## Setup

Install pytorch
```bash
pip install pytorch
```
## Training logging
[Weights & Biases](https://wandb.ai/)

### Quickstart

Install the package and login through the terminal using the provided api key
```bash
pip install wandb
wandb login
```
Initialize the 'w and b' model and add loggers to the desired outputs.
```py
import wandb
# 1. Start a new run
run = wandb.init(project="My new Project")
# 2. Save model inputs and hyperparameters
config = run.config
config.dropout = 0.01
# 3. Log gradients and model parameters
run.watch(model)
for batch_idx, (data, target) in enumerate(train_loader):
...
   if batch_idx % args.log_interval == 0:
   # 4. Log metrics to visualize performance
      run.log({"loss": loss})
...
run.finish()
```