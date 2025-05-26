---
title: "Mask Approximation Net A Novel Diffusion Model Approach for Remote Sensing Change Captioning"
collection: publications
category: manuscripts
permalink: /publication/2025-02-16-mask-approximation-net-a-novel-diffusion-model-approach-for-remote-sensing-change-captioning
excerpt: 'This paper proposes a mask approximation network using a novel diffusion model approach for remote sensing change captioning.'
date: 2025-02-16
venue: 'IEEE Transactions on Geoscience and Remote Sensing (Under Reviewing)'
slidesurl: ''
paperurl: 'https://arxiv.org/pdf/2412.19179'
citation: 'Dongwei Sun, Jing Yao, Changsheng Zhou, Xiangyong Cao, Pedram Ghamisi. (2025). &quot;Mask Approximation Net A Novel Diffusion Model Approach for Remote Sensing Change Captioning.&quot; <i>IEEE Transactions on Geoscience and Remote Sensing</i>.'

---

  Remote sensing image change description represents an innovative multimodal task within the realm of remote sensing processing. 
  This task not only facilitates the detection of alterations in surface conditions, but also provides comprehensive descriptions of these changes, thereby improving human interpretability and interactivity. 
  Generally, existing deep-learning-based methods predominantly utilized a three-stage framework that successively perform feature extraction, feature fusion, and localization from bitemporal images before text generation. However, this reliance often leads to an excessive focus on the design of specific network architectures and restricts the feature distributions to the dataset at hand, which in turn results in To address these limitations, this paper proposes a novel approach for remote sensing image change detection and description that incorporates diffusion models, aiming to transition the emphasis of modeling paradigms from conventional feature learning to data distribution learning. The proposed method primarily includes a simple multi-scale change detection module, whose output features are subsequently refined by an well-designed diffusion model. Furthermore, we introduce a frequency-guided complex filter module to boost the model performance by managing high-frequency noise throughout the diffusion process. We validate the effectiveness of our proposed method across several datasets for remote sensing change detection and description, showcasing its superior performance compared to existing techniques. The code will be available at \href{https://github.com/sundongwei}{MaskApproxNet} after a possible publication. 