# Paper Reading

简单记录一下读过的比较有意思的paper，方便以后要用的时候找到

## Adversarial Attacks CV

### Attack

- [**A Tale of Evil Twins: Adversarial Inputs versus Poisoned Models**](https://arxiv.org/abs/1911.01559): 研究了对抗攻击和投毒攻击 (主要是后门攻击)之间的内在联系、影响关系；提出了一种结合两种攻击方式的框架，并且提出优化方法使得两者结合具有$1+1>2$的效果
- [**Adversarial Preprocessing: Understanding and Preventing Image-Scaling Attacks in Machine Learning**](https://www.usenix.org/conference/usenixsecurity20/presentation/quiring): DL模型的input size是固定的，所以往往会对用户图片先进行裁剪、缩放、normalize等操作；通过对图片添加扰动可以让缩放后的图片与原图片完全不同 (e.g., 狗的图片变成一只猫)，故而存在攻击空间；本文从信号处理角度分析这一漏洞的存在原因并提出了防御方法
- [**DaST: Data-free Substitute Training for Adversarial Attacks**](https://arxiv.org/abs/2003.12703): 黑盒对抗攻击中的一种方法是使用一个替代模型，在替代模型上使用白盒攻击方法从而攻击原模型；然而训练替代模型需要与原模型相似的数据集，本文利用GAN实现无数据即可训练替代模型；只在两个较为简单的数据集上进行了实验，高分辨率以及较多类别的数据集上效果如何未知

### Defense

- [**Non-Local Context Encoder: Robust Biomedical Image Segmentation against Adversarial Attacks**](https://arxiv.org/abs/1904.12181): 提出针对医学图像分割模型对抗攻击的防御方法；利用全局空间依赖性和全局上下文信息对输入图像 (i.e., 可能是对抗样本)进行去噪 (i.e., 削弱对抗攻击施加的扰动)；这是一种能够被部署到很多不同医学图像分割网络上的方法；能否将这种方法应用到其他任务上不确定，作者也说明了医学图像本身具有较强的全局上下文依赖且医学图像分割模型往往具有过拟合的特点
- [**DeepDyve: Dynamic Verification for Deep Neural Networks**](https://arxiv.org/abs/2009.09663): 利用一个小模型作为验证器，通过小模型和原模型的输出比较判断输入是否是对抗样本；小模型是原模型的近似模型；从原模型获得小模型有两种思路——减少参数数量和减少输出类别；减少输出类别是通过对输出类别进行聚类实现的，聚类根据人为设定的Risk impact矩阵和遍历数据集获的Risk probability矩阵；可以发现参数减少数量等都是未知的 (i.e., 需要优化得到的)，所以作者也提出了获得小模型的优化算法
- [**Gotta Catch 'Em All: Using Honeypots to Catch Adversarial Attacks on Neural Networks**](https://arxiv.org/abs/1904.08554): 在训练模型时人为多加入一个输出类别，并且在训练集中backdoor一部分样本并label为人为设定的新类别；对抗攻击算法会认为这一新类别更容易产生对抗样本，从而使得模型抵挡攻击



<center>================================TBD================================</center>

## Backdoor Attacks CV

### Attack

- [**Backdoor Embedding in Convolutional Neural Network Models via Invisible Perturbation**](https://arxiv.org/abs/1808.10307): 
- [**Backdoor Pre-trained Models Can Transfer to All**](https://arxiv.org/abs/2111.00197): 
- [**Composite Backdoor Attack for Deep Neural Network by Mixing Existing Benign Features**](https://dl.acm.org/doi/10.1145/3372297.3423362): 
- [**Fawkes: Protecting Privacy against Unauthorized Deep Learning Models**](https://www.usenix.org/conference/usenixsecurity20/presentation/shan): 
- [**Hidden Trigger Backdoor Attacks**](https://arxiv.org/abs/1910.00033): 
- [**Input-Aware Dynamic Backdoor Attack**](https://arxiv.org/abs/2010.08138): 

### Defense

- [**Fine-Pruning: Defending Against Backdooring Attacks on Deep Neural Networks**](https://arxiv.org/abs/1805.12185):
- [**Neural Cleanse: Identifying and Mitigating Backdoor Attacks in Neural Networks**](https://people.cs.uchicago.edu/~ravenben/publications/pdf/backdoor-sp19.pdf): 
- [**STRIP: A Defence Against Trojan Attacks on Deep Neural Networks**](https://arxiv.org/abs/1902.06531): 

## Membership Inference Attacks

- [**GAN-Leaks: A Taxonomy of Membership Inference Attacks against Generative Models**](https://arxiv.org/abs/1909.03935):
- [**Membership Inference Attacks Against Recommender Systems**](https://arxiv.org/abs/2109.08045): 
- [**Updates-Leak: Data Set Inference and Reconstruction Attacks in Online Learning**](https://arxiv.org/abs/1904.01067): 
- [**When Machine Unlearning Jeopardizes Privacy**](https://arxiv.org/abs/2005.02205): 

## NLP Security

### Attack

- [**Analyzing Information Leakage of Updates to Natural Language Models**](https://arxiv.org/abs/1912.07942):
- [**Hidden Killer: Invisible Textual Backdoor Attacks with Syntactic Trigger**](https://arxiv.org/abs/2105.12400): 
- [**You Autocomplete Me: Poisoning Vulnerabilities in Neural Code Completion**](https://arxiv.org/abs/2007.02220): 

### Defense

- [**TextShield: Robust Text Classification Based on Multimodal Embedding and Neural Machine Translation**](https://www.usenix.org/conference/usenixsecurity20/presentation/li-jinfeng): 

## Other ML/DL Security

- [**Explanation-Guided Backdoor Poisoning Attacks Against Malware Classifiers**](https://www.usenix.org/conference/usenixsecurity21/presentation/severi): 