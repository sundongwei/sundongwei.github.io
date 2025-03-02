---
title: "A Lightweight Sparse Focus Transformer for Remote Sensing Image Change Captioning"
collection: publications
category: manuscripts
permalink: /publication/2024-01-01-a-lightweight-sparse-focus-transformer-for-remote-sensing-image-change-captioning
excerpt: 'This paper presents a lightweight sparse focus transformer designed for remote sensing image change captioning.'
date: 2024-01-01
venue: 'IEEE Journal of Selected Topics in Applied Earth Observations and Remote Sensing'
slidesurl: ''
paperurl: 'https://doi.org/10.1109/JSTARS.2024.3471625'
citation: 'Dongwei Sun, Yajie Bao, Junmin Liu, Xiangyong Cao. (2024). &quot;A Lightweight Sparse Focus Transformer for Remote Sensing Image Change Captioning.&quot; <i>IEEE Journal of Selected Topics in Applied Earth Observations and Remote Sensing</i>.'

---

  Remote sensing image change captioning (RSICC) aims to automatically generate
  sentences that describe content differences in remote sensing bitemporal images.
  Recently, attention-based transformers have become a prevalent idea for capturing
  the features of global change. However, existing transformer-based RSICC methods
  face challenges, e.g., high parameters and high computational complexity caused
  by the self-attention operation in the transformer encoder component. To alleviate
  these issues, this paper proposes a Sparse Focus Transformer (SFT) for the RSICC
  task. Specifically, the SFT network consists of three main components, i.e. a high-level
  features extractor based on a convolutional neural network (CNN), a sparse focus
  attention mechanism-based transformer encoder network designed to locate and capture
  changing regions in dual-temporal images, and a description decoder that embeds
  images and words to generate sentences for captioning differences. The proposed
  SFT network can reduce the parameter number and computational complexity by incorporating
  a sparse attention mechanism within the transformer encoder network. Experimental
  results on various datasets demonstrate that even with a reduction of over 90% in
  parameters and computational complexity for the transformer encoder, our proposed
  network can still obtain competitive performance compared to other state-of-the-art
  RSICC methods.