
<p align="center">
    <a href=""> <img src="pics/logo.png" width="400"/></a>
<p>
<p align="center">  
    <a href="http://neuralkg.zjukg.cn/">
        <img alt="Website" src="https://img.shields.io/badge/website-online-orange">
    </a>
    <a href="">
        <img alt="Pypi" src="https://img.shields.io/badge/release-v0.1.85-red">
    </a>
    <!-- <a href="">
        <img alt="LICENSE" src="https://img.shields.io/badge/license-MIT-brightgreen">
    </a> -->
    <a href="https://zjukg.github.io/NeuralKG/index.html">
        <img alt="Documentation" src="https://img.shields.io/badge/Doc-online-blue">
    </a>
</p>

<h1 align="center">
    <p>An Open Source Library for Diverse Representation Learning of Knowledge Graphs</p>
</h1>

NeuralKG is a python-based library for diverse representation learning of knowledge graphs implementing **Conventional KGEs**, **GNN-based KGEs**, and **Rule-based
KGEs**. We provide [comprehensive documents](https://zjukg.github.io/NeuralKG/index.html) for beginners and an [online website](http://neuralkg.zjukg.cn/) to organize an open and shared KG representation learning community.

<br>

# Table of Contents

* [What's New](#whats-new)
* [Overview](#Overview)
* [Implemented KGEs](#ImplementedKGEs)
* [Quick Start](#quick-start)
    * [Installation](#Installation)
   * [Training](#Training)
   * [Evaluation](#Evaluation)
   * [Hyperparameter Tuning](#Hyperparameter-Tuning)
* [Reproduced Results](#Reproduced-Results)
* [Citation](#citation)
<!-- * [To do](#to-do) -->


<br>

# 😃What's New

## Feb, 2022
* We have released a paper [NeuralKG: An Open Source Library for Diverse Representation Learning of Knowledge Graphs]()

<br>

# Overview

<h3 align="center">
    <img src="pics/overview.png", width="600">
</h3>
<!-- <p align="center">
    <a href=""> <img src="pics/overview.png" width="400"/></a>
<p> -->


NeuralKG is built on [PyTorch Lightning](https://www.pytorchlightning.ai/). It provides a general workflow of diverse representation learning on KGs and is highly modularized, supporting three series of KGEs. It has the following features:

+  **Support diverse types of methods.** NeuralKG, as a library for diverse representation learning of KGs, provides implementations of three series of KGE methods, including **Conventional KGEs**, **GNN-based KGEs**, and **Rule-based KGEs**.


+ **Support easy customization.** NeuralKG contains fine-grained decoupled modules that are commonly used in different KGEs, including KG Data Preprocessing, Sampler for negative sampling, Monitor for hyperparameter tuning, Trainer covering the training, and model validation.

+ **long-term technical maintenance.** The core team of NeuralKG will offer long-term technical maintenance. Other developers are welcome to pull requests.

<br>

# Implemented KGEs

|Components| Models |    
|:---|:--------------:|
|KGEModel|[TransE](https://papers.nips.cc/paper/2013/hash/1cecc7a77928ca8133fa24680a88d2f9-Abstract.html), [TransH](https://ojs.aaai.org/index.php/AAAI/article/view/8870), [TransR](https://www.aaai.org/ocs/index.php/AAAI/AAAI15/paper/viewFile/9571/9523/), [ComplEx](http://proceedings.mlr.press/v48/trouillon16.pdf), [DistMult](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/ICLR2015_updated.pdf), [RotatE](https://arxiv.org/abs/1902.10197), [ConvE](https://arxiv.org/abs/1707.01476), [BoxE](https://arxiv.org/pdf/2007.06267.pdf), [CrossE](https://arxiv.org/abs/1903.04750), [SimplE](https://arxiv.org/abs/1802.04868)|
|GNNModel|[RGCN](https://arxiv.org/abs/1703.06103), [KBAT](https://arxiv.org/abs/1906.01195), [CompGCN](https://arxiv.org/abs/1906.01195), [XTransE](https://link.springer.com/chapter/10.1007/978-981-15-3412-6_8)|
|RuleModel|[ComplEx-NNE+AER](https://aclanthology.org/P18-1011/), [RUGE](https://arxiv.org/abs/1711.11231), [IterE](https://arxiv.org/abs/1903.08948)|

<br>

# Quick Start

## Installation

**Step1** Create a virtual environment using ```Anaconda``` and enter it
```bash
conda create -n neuralkg python=3.8

conda activate neuralkg
```
**Step2** Install package
+ From Pypi
```bash
pip install neuralkg
```

+ From Source

```bash
git clone git@github.com:zjukg/NeuralKG.git
cd NeuralKG
python setup.py install
```
## Training
```
# Use bash script
sh ./scripts/your-sh

# Use config
python main.py --load_config --config_path <your-config>

```

## Evaluation
```
python main.py --test_only --checkpoint_dir <your-model-path>
```
## Hyperparameter Tuning
NeuralKG utilizes [Weights&Biases](https://wandb.ai/site) supporting various forms of hyperparameter optimization such as grid search, Random search, and Bayesian optimization. The search type and search space are specified in the configuration file in the format "*.yaml" to perform hyperparameter optimization.

The following config file displays hyperparameter optimization of the TransE on the FB15K-237 dataset using grid search:
```
command:
  - ${env}
  - ${interpreter}
  - ${program}
  - ${args}
program: main.py
method: grid
metric:
  goal: maximize
  name: Eval|hits@10
parameters:
  dataset_name:
    value: FB15K237
  model_name:
    value: TransE
  loss_name:
    values: [Adv_Loss, Margin_Loss]
  train_sampler_class:
    values: [Unisampler, BernSampler]
  emb_dim:
    values: [400, 600]
  lr:
    values: [1e-4, 5e-5, 1e-6]
  train_bs:
    values: [1024, 512]
  num_neg:
    values: [128, 256]
```
<br>

# Reproduced Results
There are some reproduced model results on FB15K-237 dataset using NeuralKG as below. See more results in [here](https://zjukg.github.io/NeuralKG/result.html)


|Method | MRR | Hit@1 | Hit@3 | Hit@10 |
|:------:|:---:|:-----:|:-----:|:------:|
|TransE|0.32|0.23|0.36|0.51|
|TransR|0.23|0.16|0.26|0.38|
|TransH|0.31|0.2|0.34|0.50|
|DistMult|0.30|0.22|0.33|0.48|
|ComplEx|0.25|0.17|0.27|0.40|
|SimplE|0.16|0.09|0.17|0.29|
|ConvE|0.32|0.23|0.35|0.50|
|RotatE|0.33|0.23|0.37|0.53|
|BoxE|0.32|0.22|0.36|0.52|
|XTransE|0.29|0.19|0.31|0.45|
|RGCN|0.25|0.16|0.27|0.43|
|KBAT*|0.19|0.11|0.22|0.38|
|CompGCN|0.34|0.25|0.38|0.52|
|IterE|0.18|0.13|0.19|0.26|

*:There is a label leakage error in KBAT, so the corrected result is poor compared with the paper result. Details in https://github.com/deepakn97/relationPrediction/issues/28

<!-- <br> -->

<!-- # To do -->

<br>

# Citation

Please cite our paper if you use NeuralKG in your work

```bibtex

```
<br>