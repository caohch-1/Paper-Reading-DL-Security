# Paper Reading

简单记录一下读过的比较有意思的paper，方便以后要用的时候找到

## Adversarial Attacks CV

### Attack

- [**A Tale of Evil Twins: Adversarial Inputs versus Poisoned Models**](https://arxiv.org/abs/1911.01559): 研究了对抗攻击和投毒攻击 (主要是后门攻击)之间的内在联系、影响关系；提出了一种结合两种攻击方式的框架，并且提出优化方法使得两者结合具有 1+1>2​ 的效果
- [**Adversarial Preprocessing: Understanding and Preventing Image-Scaling Attacks in Machine Learning**](https://www.usenix.org/conference/usenixsecurity20/presentation/quiring): DL模型的input size是固定的，所以往往会对用户图片先进行裁剪、缩放、normalize等操作；通过对图片添加扰动可以让缩放后的图片与原图片完全不同 (e.g., 狗的图片变成一只猫)，故而存在攻击空间；本文从信号处理角度分析这一漏洞的存在原因并提出了防御方法
- [**DaST: Data-free Substitute Training for Adversarial Attacks**](https://arxiv.org/abs/2003.12703): 黑盒对抗攻击中的一种方法是使用一个替代模型，在替代模型上使用白盒攻击方法从而攻击原模型；然而训练替代模型需要与原模型相似的数据集，本文利用GAN实现无数据即可训练替代模型；只在两个较为简单的数据集上进行了实验，高分辨率以及较多类别的数据集上效果如何未知

### Defense

- [**Non-Local Context Encoder: Robust Biomedical Image Segmentation against Adversarial Attacks**](https://arxiv.org/abs/1904.12181): 提出针对医学图像分割模型对抗攻击的防御方法；利用全局空间依赖性和全局上下文信息对输入图像 (i.e., 可能是对抗样本)进行去噪 (i.e., 削弱对抗攻击施加的扰动)；这是一种能够被部署到很多不同医学图像分割网络上的方法；能否将这种方法应用到其他任务上不确定，作者也说明了医学图像本身具有较强的全局上下文依赖且医学图像分割模型往往具有过拟合的特点
- [**DeepDyve: Dynamic Verification for Deep Neural Networks**](https://arxiv.org/abs/2009.09663): 利用一个小模型作为验证器，通过小模型和原模型的输出比较判断输入是否是对抗样本；小模型是原模型的近似模型；从原模型获得小模型有两种思路——减少参数数量和减少输出类别；减少输出类别是通过对输出类别进行聚类实现的，聚类根据人为设定的Risk impact矩阵和遍历数据集获的Risk probability矩阵；可以发现参数减少数量等都是未知的 (i.e., 需要优化得到的)，所以作者也提出了获得小模型的优化算法
- [**Gotta Catch 'Em All: Using Honeypots to Catch Adversarial Attacks on Neural Networks**](https://arxiv.org/abs/1904.08554): 在训练模型时人为多加入一个输出类别，并且在训练集中backdoor一部分样本并label为人为设定的新类别；对抗攻击算法会认为这一新类别更容易产生对抗样本，从而使得模型抵挡攻击



## Backdoor Attacks CV

### Attack

- [**Backdoor Embedding in Convolutional Neural Network Models via Invisible Perturbation**](https://arxiv.org/abs/1808.10307): 目前的CV中的后门攻击具有一个普遍问题——trigger很容易被肉眼发现（Backdoor Attack本质上是一种供应链攻击，目前针对代码有代码审查但是针对用于训练DLM Model的Dataset似乎没有规范的审查方案，数据集中往往包含几十几百万张图片，对于其中一部分加入trigger其实并不容易发现，但是如果能够隐藏trigger自然会更好）；作者提出了使用Perturbation Mask (PM)作为trigger从而达到隐藏效果；PM说白了就是给原始图的每个像素做加加减减，加加减减的值对应到每个pixel位置就是一张和原始图同shape的Mask；PM分两种，Patterned Static和Targeted Adaptive；前者很简单，就是人为设定每个pixel应该增减多少，但是实验证明效果一般（看到现在论文里backdoor Attack一般成功率稳定都在.95左右，这种在50%~95%横跳），这是可以理解的，毕竟这种PM不增强图片本身的某一特征又不能新增一种明显的特征；后者则是给PM套了层壳，利用对抗攻击Adversarial Attacks算法(e.g., DeepFool)来对要投毒的图片计算Perturbation，遍历所有投毒点来迭代升级PM（可以理解为反着的对抗训练？），效果不错，但是这里有一个疑问，到底是投毒(后门)达到了攻击效果 还是 PM本身源于Adversarial Attacks这一特点导致了攻击成功；总体来说简单但是novel的一种方法，但是很难应用到物理世界场景中，毕竟用到了Adversarial Attack计算出的Perturbation，这种细微扰动很难施加到物理世界
- [**Backdoor Pre-trained Models Can Transfer to All**](https://arxiv.org/abs/2111.00197): 没看完，当时有一篇和手上工作有关的work就去看那篇了，以后再补
- [**Composite Backdoor Attack for Deep Neural Network by Mixing Existing Benign Features**](https://dl.acm.org/doi/10.1145/3372297.3423362): 同样关注了trigger很容易被发现这一问题，很好地总结了patch based trigger的问题——与原图语义不符合、trigger和模型任务无关以及trigger成为了target label的一个强特征，前两者导致容易被人类察觉后者导致容易被backdoor防御算法察觉；作者提出使用数据集中原有的其他特征作为trigger；举个栗子，人脸分类为例，victim是人脸类A，target是人脸类B，作者提出将其他一类人脸类C的图片与A拼接形成新的图片并label为B，从而达到backdoor attack的目的；效果很好且作者在目标识别等其他任务上也进行了实验，达到很好的效果；缺点觉得有两个吧，首先拼接图片使用简单的crop然后覆盖或者连接成更大图的方式，如果检查数据集依然可以明显发现与其他数据点有很大不同，可以说达到了在test时隐藏trigger但在train时无法隐藏；第二个问题trigger类型被局限在了已有数据集的类中，这是为了弥补patch based trigger的第二个问题；我们目前正在思考利用GLIDE+Image Retrieval的方法进一步解决这一问题，但是实验上遇到了一些问题
- [**Fawkes: Protecting Privacy against Unauthorized Deep Learning Models**](https://www.usenix.org/conference/usenixsecurity20/presentation/shan): 把攻击反过来用就是保护，作者提出了一种能够保护用户照片无法被深度学习模型利用的方法与工具；简单来说你是A，有个人B，通过给你的照片添加扰动（是的，就类似Adversarial Attacks里的扰动）从而使你的照片在像素空间保持与原图一致，在特征空间与B相似，从而导致DL Model无法分辨A和B或者把A当成B；挺取巧的一篇paper，相当于把对抗和后门两种东西揉一起然后从保护的角度讲了一遍，效果不错
- [**Hidden Trigger Backdoor Attacks**](https://arxiv.org/abs/1910.00033): 还是关注trigger的一篇paper，我们把加上了trigger的victim图片叫PS(Pateched Source)，target类别的图片叫CT(Clean Target)，通过作者提出的算法使得CT在像素空间保持与原CT一致，在特征空间与PS相似，从而模型会认为具有PS特征的图片属于CT类别；这里的关键在于让CT去靠近PS，从而使得trigger只作用于特征空间而不必出现在像素空间达到了隐藏trigger的效果；然而在test时trigger依然要显式地出现在图片上，可以说达到了在train时隐藏trigger但在test时无法隐藏
- [**Input-Aware Dynamic Backdoor Attack**](https://arxiv.org/abs/2010.08138): 依然是关注trigger的一篇paper，作者提出了一种基于图片动态生成trigger的方法，从而避免trigger的重复出现而被人或算法发现；简单来说，训练一个生成器g(g的作用可以简单理解为类似给图片加椒盐噪音，只不过这些噪音不是随机的，是基于输入图片以及g的内部权重和结构，g是个DL Model)，加上了g产生的mask的图片就相当于植入了backdoor；确实作者达到了目的但是trigger实际上也没有被隐藏起来，只是看上去更像噪音从而不容易被发现，而且这种噪音类似于Adversarial Attack计算出的Perturbation，这种扰动很难施加到物理世界



<center>================================TBD================================</center>

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