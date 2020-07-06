# Self-Adaptive Training
This is the PyTorch implementation of the paper [Self-Adaptive Training: beyond Empirical Risk Minimization](https://arxiv.org/abs/2002.10319).

## Requirements

- Python >= 3.6
- PyTorch >= 1.0
- CUDA
- Numpy

## Usage
### Standard training
The `main.py` contains training and evaluation functions in standard training setting.
#### Runnable scripts
- Training and evaluation using the default parameters
  
  We provide our training scripts in directory `scripts/`. For a concrete example, we can use the command as below to train the default model (i.e., ResNet-34) on CIFAR10 dataset with uniform label noise injected (e.g., 40%):
  ```bash
  $ bash scripts/cifar10/run_sat.sh [TRIAL_NAME]
  ```
  The argument `TRIAL_NAME` is optional, it helps us to identify different trials of the same experiments without modifying the training script. The evaluation is automatically performed when training is finished.

- Additional arguments 
  - `noise-rate`: the percentage of data that being corrupted
  - `noise-type`: type of random corruptions (i.e., corrupted_label, Gaussian,random_pixel, shuffled_pixel)
  - `sat-es`: initial epochs of our approach
  - `sat-alpha`: the momentum term $\alpha$ of our approach


#### Results on CIFAR datasets under uniform label noise
- CIFAR10

|Noise Rate         |0.2    |0.4    |0.6    |0.8    |
|-------------------|-------|-------|-------|-------|
|Test Accuracy(%)   |94.14  | 92.64 |89.23  |78.58  |

- CIFA100

|Noise Rate         |0.2    |0.4    |0.6    |0.8    |
|-------------------|-------|-------|-------|-------|
|Test Accuracy(%)   |75.77  |71.38  |62.69  |38.72  |


### Adversarial training
We use state-of-the-art adversarial training algorithm [TRADES](https://github.com/yaodongyu/TRADES) as our baseline. The `main_adv.py` contains training and evaluation functions in adversarial training setting on CIFAR10 dataset.

#### Training scripts
- Training and evaluation using the default parameters
  
  We provides our training scripts in directory `scripts/cifar10`. For a concrete example, we can use the command as below to train the default model (i.e., WRN34-10) on CIFAR10 dataset with PGD-10 attack ($\epsilon$=0.031) to generate adversarial examples:
  ```bash
  $ bash scripts/cifar10/run_trades_sat.sh [TRIAL_NAME]
  ```

- Additional arguments 
  - `beta`: hyper-parameter $1/\lambda$ in TRADES that controls the trade-off between natural accuracy and adversarial robustness
  - `sat-es`: initial epochs of our approach
  - `sat-alpha`: the momentum term $\alpha$ of our approach

#### Robust evaluation script
Evaluate robust WRN-34-10 models on CIFAR10 under PGD-20 attack:
```bash
  $ python pgd_attack.py --model-dir "/path/to/checkpoints"
```
This command evaluates 71-st to 100-th checkpoints in the specified path.

#### Results
<p align="center">
    <img src="images/robust_acc.png" width="450"\>
</p>
<p align="center">
Self-Adaptive Training mitigates the overfitting issue and consistently improves TRADES.
</p>

#### Attack TRADES+SAT
We provide the checkpoint of our best performed model in [Google Drive](https://drive.google.com/file/d/1By8kbU-DvL_vAYHdHkDtganrU88n81TT/view?usp=sharing) and report its performance as below.
|Attack|Submitted by   |Natural Accuracy (%)|Robust Accuracy (%)|
|------|---------------|--------------------|-------------------|
|PGD-20|(initial entry)|83.48               |58.03              |

## Reference
For technical details, please check [the paper](https://arxiv.org/abs/2002.10319).
```
@article{huang2020sat,
        title = {Self-Adaptive Training: beyond Empirical Risk Minimizatio},
        author = {Lang Huang and Chao Zhang and Hongyang Zhang},
        journal = {arXiv preprint arXiv:2002.10319},
        year = {2020}
}
```