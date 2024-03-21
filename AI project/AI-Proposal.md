# ProtectingPrivacy against Diffusion Model Fine-tuning
### 主题
图像生成与隐私保护
### 方法
使用CLIP模型生成微小扰动来降低diffusion模型的质量
### 目标
- 通过参数调节最大化CLIP的loss
- 评测模型性能（抗干扰性、保护隐私性能、运算负担）
### 相关算法与技术
- CLIP, Textual Inversion, Dreambooth, LoRA, etc
- Anti-DreamBooth, AEDG, etc

# Watermarkingagainst Fine-tuned Diffusion Models
### 主题
图像生成与隐私保护
### 方法
在原始图片中添加可被检测的watermarks
### 目标
- 优化水印检测器和扰动生成器的性能
- 评测模型性能（准确率、召回率、运算负担、watermarks resilience）
- 比较与SOTA的性能
### 相关算法与技术
- Textual Inversion, Dreambooth, LoRA, etc. 
- Anti-DreamBooth, AEDG, etc.
- Adapt Anti-DreamBooth to our setting.
# Dynamic LoRA Adapters for Large Transformer Models
### 主题
模型性能提高
### 方法
LoRA的基本思路+正则项训练+PCA降维
### 相关算法与技术
- Algorithms: Low-rank matrix factorization. Feature boosting and suppression. LoRA.
- Techniques: HuggingFace: datasets, trainer, evaluate, transformers, diffusers, peft.
# Revisiting Feature Boosting and Suppression for Large Transformer Models
### 主题
模型性能提高
### 方法
LoRA的基本思路+矩阵分解+正则项训练
#### 难点
矩阵如何分解
### 相关算法与技术 
- Algorithms: Low-rank matrix factorization. Feature boosting and suppression. LoRA. 
- Techniques: HuggingFace: datasets, trainer, evaluate, transformers, diffusers, peft.