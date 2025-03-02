---
title: "Active contours driven by Gaussian function and adaptive-scale local correntropy-based K-means clustering for fast image segmentation"
collection: publications
category: manuscripts
permalink: /publication/2020-active-contours-gaussian-aslck
excerpt: "This paper proposes a novel active contour model based on a Gaussian function and adaptive-scale local correntropy-based K-means clustering (Gaussian-ASLCK) for image segmentation with severe intensity inhomogeneity."
date: 2020-01-01
venue: "Signal Processing"
paperurl: "https://doi.org/10.1016/j.sigpro.2020.107625"
pdf: "journal/Active_segmentation.pdf"
citation: "Yangyang Song, Guohua Peng, Dongwei Sun, Xiaozhen Xie. (2020). &quot;Active contours driven by Gaussian function and adaptive-scale local correntropy-based K-means clustering for fast image segmentation.&quot; <i>Signal Processing</i>, 174, 107625."
---

In this paper, we proposed a novel active contour model based on Gaussian
function and adaptive-scale local correntropy-based K-means clustering (Gaussian-ASLCK)  that
is applicable to images with severe intensity inhomogeneity. Firstly, Gaussian function
is used to capture the global intensity of images,  which boosts the robustness
to the initialization. Subsequently, by employing the adaptive scale operator, the
scale of the proposed model can be  adjusted automatically according to the degree
of intensity inhomogeneity. In addition, the regularized term is approximated by
a non-local energy,  which is constructed based on characteristic functions in the
field of heat kernel convolution. Finally, the iterative convolution-thresholding
method is designed for  minimizing the energy function. This method is simple, efficient
and has the optimal complexity of O(Nlog N) per iteration. The experimental results
comprehensively validate  that the proposed Gaussian-ASLCK model enjoys greater
success in images with severe intensity inhomogeneity and complex noise over other
state-of-the-art models.