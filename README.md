## GAN Shop: A Library for Experiment and Evaluation of GANs (Early Version)

GAN Shop is a library for experiment and evaluation of modern GANs. The objective of GAN Shop project is to enable machine learning researchers to easily implement their ideas and compare them with other GAN frameworks. To do so, we have implemented or are going to implement state-of-the-art GAN models and welcome every feedbacks from the users.

```
Since we are in the final exam period, we will start updating the GAN Shop after the final exam.
```

## 1. Implemented GANs

* [Vanilla DCGAN (Radford et al.)](https://arxiv.org/abs/1511.06434)
* [WGAN Weight Clipping (Arjovsky et al.)](https://arxiv.org/abs/1701.07875)
* [WGAN Gradient Penalty (Gulrajani et al.)](https://arxiv.org/abs/1704.00028)
* [ACGAN (Odena et al.)](https://arxiv.org/abs/1610.09585)
* [Geometric GAN (Lim and Ye)](https://arxiv.org/abs/1705.02894)
* [cGAN (Miyato and Koyama)](https://arxiv.org/abs/1705.02894)
* [SNDCGAN,SNResGAN (Miyato et al.)](https://arxiv.org/abs/1802.05957)
* [SAGAN (Zhang et al.)](https://arxiv.org/abs/1805.08318)
* [BigGAN (Brock et al.)](https://arxiv.org/abs/1809.11096)
* [CRGAN (Zhang et al.)](https://arxiv.org/abs/1910.12027)
* [ContraGAN (Ours)](https://github.com/)

## 2. Are going to implement

* [DRAGAN (Kodali et al.)](https://arxiv.org/abs/1705.07215)
* [BigGAN-Deep (Brock et al.)](https://arxiv.org/abs/1809.11096)
* [ICRGAN (Zhao et al.)](https://arxiv.org/abs/2002.04724)
* [LOGAN (Wu et al.)](https://arxiv.org/abs/1912.00953)
* [CntrGAN (Zhao et al.)](https://arxiv.org/abs/2006.02595)

## 3. Enviroment Setting
```
conda env create -f environment.yml -n ganshop
```

## 4. Dataset(CIFAR10, Tiny ImageNet, ImageNet possible)
```
├── data
   └── ILSVRC2012
       ├── train
           ├── n01443537
     	        ├── image1.png
     	        ├── image2.png
		└── ...
           ├── n01629819
           └── ...
       ├── val
           └── val_folder
	        ├── val1.png
     	        ├── val2.png
		└── ...
```


## 5. How to run

Requirements

- Pytorch xx
- xx

For CIFAR10 image generation tasks:

```
CUDA_VISIBLE_DEVICES=0 python3 main.py --eval -t -c "./configs/Table2/biggan32_cifar_hinge_no.json"
```

For Tiny ImageNet generation tasks:

```
CUDA_VISIBLE_DEVICES=0,1,2,3 python3 main.py --eval -t -c "./configs/Table2/biggan64_tiny_hinge_no.json"
```

For ImageNet generation tasks:

```
CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 python3 main.py --eval -t -c "./configs/Imagenet_experiments/proj_biggan128_imagenet_hinge_no.json"
```

For ImageNet generation tasks (load all images in main memory to reduce I/O bottleneck):
```
CUDA_VISIBLE_DEVICES=0,1,2,3,4,5,6,7 python3 main.py --eval -t -l -c "./configs/Imagenet_experiments/proj_biggan128_imagenet_hinge_no.json"
```

## 6. About PyTorch FID

FID is a widely used metric to evaluate the performance of a GAN model. Since calculating FID requires a pre-trained inception-V3 network, many implementations use Tensorflow (https://github.com/bioinf-jku/TTUR) or PyTorch (https://github.com/mseitzer/pytorch-fid) libraries. Among them, the TensorFlow implementation for FID measurement is widely used. We use the PyTorch implementation for FID measurement, instead. In this section, we show that the PyTorch based FID implementation used in our work provides almost the same results with the TensorFlow implementation. The results are summarized in Table below.
<p align="center"><img src = 'figures/Table3.png' height = '140px' width = '460px'>

## Contrastive Generative Adversarial Networks

**Abstract**

Conditional image synthesis is the task to generate high-fidelity diverse images using class label information. Although many studies have shown realistic results, there is room for improvement if the number of classes increases. In this paper, we propose a novel conditional contrastive loss to maximize a lower bound on mutual information between samples from the same class. Our framework, called Contrastive Generative Adversarial Networks (ContraGAN), learns to synthesize images using class information and data-to-data relations of training examples. The discriminator in ContraGAN discriminates the authenticity of given samples and maximizes the mutual information between embeddings of real images from the same class. Simultaneously, the generator attempts to synthesize images to fool the discriminator and to maximize the mutual information of fake images from the same class prior. The experimental results show that ContraGAN is robust to network architecture selection and outperforms state-of-the-art models by 3.7% and 11.2% on CIFAR10 and Tiny ImageNet datasets, respectively, without any data augmentation. For the fair comparison, we re-implement nine state-of-the-art approaches to test various methods under the same condition. 


## 1. Illustrative Figures of Conditional Contrastive Loss
<p align="center"><img src = 'figures/metric learning loss.png' height = '200px' width = '800px'>


Illustrative figures visualize the metric learning losses (a,b,c) and conditional GANs (d,e,f). The objective of all losses is to collect samples if they have the same label but keep them away otherwise. The color indicates the class label, and the shape represents the role. (Square) an embedding of an image. (Diamond) an augmented embedding. (Circle) a reference. Each loss is applied to the reference. (Star) the embedding of a class label. (Triangle) the one-hot encoding of a class label. The thicknesses of red and blue lines represents the strength of pull and push force, respectively. Compared to ACGAN and cGAN, our loss is inspired by XT-Xent to consider data-to-data relationships and to infer full information without data augmentation.


## 2. Schematics of the discriminator of three conditional GANs
![Figure2](https://github.com/MINGUKKANG/Pytorch-GAN-Benchmark/blob/master/figures/conditional%20GAN.png)

Schematics of the discriminators of three conditional GANs. (a) ACGAN has an auxiliary classifier to help the generator to synthesize well-classifiable images. (b) cGAN improves the ACGAN by adding the inner product of an embedded image and the corresponding class embedding. (c) Our approach extends cGAN with conditional contrastive loss (2C loss) between embedded images and the actual label embedding. ContraGAN considers multiple positive and negative pairs in the same batch, as shown in the above figure (f).

## 3. Results

### Quantitative Results
**Table1:** Experiments using CIFAR10 and Tiny ImageNet dataset. Using three backbone architectures (SNDCGAN, SNResGAN, and BigGAN), we test three approaches using different class information conditioning (ACGAN, cGAN, and ours). Mean +- variance of FID is reported.
<p align="center"><img src = 'figures/Table1.png' height = '150px' width = '720px'>

**Table2:** Comparison with state-of-the-art GAN models. We mark "*" to FID values reported in the original papers (BigGAN and CRGAN). The other FID values are obtained from our implementation.
<p align="center"><img src = 'figures/Table2.png' height = '100px' width = '740px'>

### Qualitative Results
**Figure 1:** Examples of generated images using ContraGAN. (left) CIFAR10, FID: 10.322, (right) Tiny ImageNet, FID: 27.018. 
<p align="center"><img src = 'figures/generated images.png' height = '450px' width = '900px'>

## 4. Pre-trained model on Tiny ImageNet
```
https://drive.google.com/file/d/1XsouS_HIlES9CAshrtgYA3b6H2qUXTnx/view?usp=sharing
```

## 5. How to run

For CIFAR10 image generation tasks:

```
CUDA_VISIBLE_DEVICES=0 python3 main.py --eval -t -c "./configs/Table1/contra_biggan32_cifar_hinge_no.json"
```

For Tiny ImageNet generation tasks:

```
CUDA_VISIBLE_DEVICES=0,1,2,3 python3 main.py --eval -t -c "./configs/Table1/contra_biggan64_tiny_hinge_no.json"
```

To use pre-trained model on Tiny ImageNet:
```
CUDA_VISIBLE_DEVICES=0,1,2,3 python3 main.py -c "./configs/Table1/contra_biggan64_tiny_hinge_no.json" --checkpoint_folder "./checkpoints/contra_tiny_imagenet_pretrained" --step 50000 --eval -t
```

## 6. References

**Self-Attention module:** https://github.com/voletiv/self-attention-GAN-pytorch

**Exponential Moving Average:** https://github.com/ajbrock/BigGAN-PyTorch

**Tensorflow FID:** https://github.com/bioinf-jku/TTUR

**Pytorch FID:** https://github.com/mseitzer/pytorch-fid

**Synchronized BatchNorm:** https://github.com/vacancy/Synchronized-BatchNorm-PyTorch

**Implementation Details:** https://github.com/ajbrock/BigGAN-PyTorch


## Citation





