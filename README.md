# MPCViT-Evaluation
This is the source code of model evalution/inference for [MPCViT: Searching for Accurate and Efficient MPC-friendly Vision Transformer with Heterogeneous Attention](https://arxiv.org/pdf/2211.13955.pdf).

Our trained MPCViT models which are reported in the paper are avaliable [here](https://drive.google.com/drive/folders/1YFICe9me9LY3F37uG0YGXw7_52HA3bFL?usp=sharing).

## Abstract
Secure multi-party computation (MPC) enables compu- tation directly on encrypted data and protects both data and model privacy in deep learning inference. However, existing neural network architectures, including Vision Transformers (ViTs), are not designed or optimized for MPC and incur significant latency overhead. We observe Softmax accounts for the major latency bottleneck due to a high communication complexity, but can be selectively replaced or linearized without compromising the model accuracy. Hence, in this paper, we propose an MPC-friendly ViT, dubbed MPCViT, to enable accurate yet efficient ViT inference in MPC. Based on a systematic latency and accuracy evaluation of the Softmax attention and other attention variants, we propose a heterogeneous attention optimization space. We also develop a simple yet effective MPC-aware neural architecture search algorithm for fast Pareto optimization. To further boost the inference efficiency, we propose MPCViT+, to jointly optimize the Softmax attention and other network components, including GeLU, matrix multiplication, etc. With extensive experiments, we demonstrate that MPCViT achieves 1.9%, 1.3% and 4.6% higher accuracy with 6.2×, 2.9× and 1.9× latency reduction compared with baseline ViT, MPCFormer and THE-X on the Tiny-ImageNet dataset, respectively. MPCViT+ further achieves 1.2× latency reduction on CIFAR-100 dataset and reaches a better Pareto front compared with MPCViT.

## Model inference of loaded MPCViT checkpoints
**Command examples:**

Below is an example to evaluate MPCViT with $\mu=0.5$ **w/o** knowledge distillation (KD) on CIFAR-10.
```shell
python inference.py --config configs/datasets/cifar10.yml --model vit_7_4_32 /path/to/cifar10 --model-checkpoint mpcvit_checkpoints/cifar-10/mpcvit_cifar10-0.5.pth.tar
```

Below is an example to evaluate MPCViT with $\mu=0.5$ **w/** knowledge distillation (KD) on CIFAR-10.
```shell
python inference.py --config configs/datasets/cifar10.yml --model vit_7_4_32 /path/to/cifar10 --model-checkpoint mpcvit_checkpoints/cifar-10/mpcvit_cifar10-0.5-kd.pth.tar
```

**Datasets:**

We provide model checkpoints on three widely used datasets, i.e., CIFAR-10, CIFAR-100 and Tiny-ImageNet.

Usage: just simply modify the command above including `config, model, data_dir, model-checkpoint`.

1. config:
```shell
configs/datasets/cifar10.yml
configs/datasets/cifar100.yml
configs/datasets/tiny_imagenet.yml
```

2. model:
```shell
vit_7_4_32 for CIFAR-10/100
vit_9_12_64 for Tiny-ImageNet
```

3. data_dir:
   
set as the corresponding datasets.

5. model-checkpoint:
   
set as the corresponding checkpoints.

**Checkpoints:**

Our trained MPCViT models which are reported in the paper are avaliable [here](https://drive.google.com/drive/folders/1YFICe9me9LY3F37uG0YGXw7_52HA3bFL?usp=sharing).
Your can freely download the checkpoints that you need to evaluate the model performance.

**Numerical results:**

Note that we missed a part of the model checkpoints during completing this paper, so we re-run our experiments again to obtain the missing checkpoints.
Therefore, there is a slight fluctuation (less than 0.1%) between the model results and the results reported in the paper, which is considered to be completely acceptable.

## MPCViT+ evaluation

Besides MPCViT, we also propose MPCViT+ to accelerate MLP blocks including GeLU and linear operators.
You can evaluate MPCViT+ models on CIFAR-10/100 with the following command:
```shell
python inference.py --config configs/datasets/cifar10.yml --model vit_7_4_32 /path/to/cifar10 --model-checkpoint mpcvit_checkpoints/cifar-10/mpcvit_plus_cifar10.pth.tar --linear-gelu
```
Note that some data points need post-added ReLU after GeLU linearization.

## Citation
```bibtex
@inproceedings{zeng2023mpcvit,
  title={Mpcvit: Searching for accurate and efficient mpc-friendly vision transformer with heterogeneous attention},
  author={Zeng, Wenxuan and Li, Meng and Xiong, Wenjie and Tong, Tong and Lu, Wen-jie and Tan, Jin and Wang, Runsheng and Huang, Ru},
  booktitle={Proceedings of the IEEE/CVF International Conference on Computer Vision},
  pages={5052--5063},
  year={2023}
}
```
