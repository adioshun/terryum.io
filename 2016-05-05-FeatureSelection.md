---
layout:     post
title:      What is Dim Reduction & Feature Selection/Extraction
date:       2016-05-05 12:00:00
summary:    Kinds of feature selection methods
categories: ML_Theory, ML_Practice
thumbnail: hourglass-half
tags:
 - feature selection
 - feature extraction
---

# Why do we need features?

Roughly speaking, **machine learning** is a black box which maps from input to output data. This *magic box* is a learned function which can be either linear or nonlinear **function of input**. We can learn the function by using training data, but it doesn't always work well.

Let's consider an extreme case: we would like to predict the *result of a baseball game* (=output) based on *the data of number of viewers* (=input). Do you think it is possible to accurately predict the result with this input? It may not because the input contains **not enough information** to explain how output is generated.

As presented in the previous example, the performance of machine learning is highly **dependent on the quantity and quality of data**. The best input contains the right information which is neither *insufficient* nor *redundant*. Then, can we gather just right information from observation? It may be possible if we have perfect prior knowledge about the problem, but that is not the case in most situations because, in most cases, we are applying a machine learning technique because we don't have enough knowledge about the problem.

Thus, what we usually do is to gather *enough* data. first (e.g., attach enough number of sensors to the object we want to observe), and figure out which one is useful or not later. The later process is called **feature selection** or **feature extraction**. This process is usually done before starting learning process (because it is a process to make a *new input*), so it is one of the core pre-process in machine learning structure.

[![Features of a face image][S3_FeatureFace]][Src_FeatureFace]

# What is dimensionality reduction?

Let's say now we have enough input which may be very high dimensional. For example, if you attach ```m``` accelerometers to the subject's body, you will get ```3m``` dimensional data because each accelerometer measures ```acc_x, acc_y, acc_z```. Also if you gather the data during the time ```t``` which is correspond to ```n``` samples, what you will get is a ```n by 3m``` matrix as your input.

As we mentioned before, we may not need all ```3m``` features; some features may be redundant because they can be represented by a combination of other features. Thus we believe that the **latent space** which causes the observed samples may be much lower than the **observation space**. Figuring out the latent space based on the samples on the observation space is called **dimensionality reduction** process. (In this case, the samples can be considered as *realizations* of the points in the latent space.)

Before going further, I'd like to re-emphasize that **dimensionality reduction technique** is not just about compressing data or reducing noise of data. It is true that they are the effects that we can obtain from the dimensionality reduction process, but the main meaning of the process exists in **finding the latent space** which can explain the observed data well.

[![Finding the latent space][S3_Dim]][Src_Dim]

# Feature selection vs. Feature extraction

There are two ways to reduce the dimension of the data: **feature selection** and **feature extraction**.

The objective of **feature selection** is to make a compact feature set by choosing the subset of all features. For example, if you think **acc_x** and **acc_y** gives no effect on the result of jumping height which is the target to be predicted, you can simply drop those from the entire variable sets. That is the thing done by feature selection methods: remove irrelevant features(variables) from the original data.

You can do this process based on your **prior knowledge** for the problem as we did in the previous example, but it is not the case for all situations. Don't worry, we have a variety of **automatic feature selection methods**. What they do is very simple: drop some of features and see the performance if it is improved. I know I explained too simply, but that is the basic working flow of all feature selection algorithms.

Feature extraction, on the other hand, attempts to make a new feature by combinating the raw features. For example, **Principal Component Analysis (PCA)**, which is the most basic and popular dimensionality method, finds principal orthogonal axes from the data and projects all data on to the axes. In this case, the projection function which maps from the original data to projected data is the way to make a new feature by linearly combinating the raw features.

[![A general framework of wrapper feature selection methods][S3_Feat]][feat]

I'd like to introduce a very good [review paper][feat] published by J. Li et al. (2016). The good news is that they provide easy-to-use [feature selection package][pack] in python and in Matlab as well. The package contains around 40 popular feature selection methods.

> [Paper] &nbsp; Li, Jundong, et al. *"Feature Selection: A Data Perspective."*, arXiv:1601.07996, [[link]](http://arxiv.org/abs/1601.07996) (2016). </br>
> [Package]  &nbsp; http://featureselection.asu.edu/

And I'd like to cite the part which explains well about various feature selection and feature methods.

> Dimensionality reduction is one of the most powerful tools to address the previously
described issues. It can be categorized mainly into into two main components: feature extraction
and feature selection.

> Feature extraction projects original high dimensional feature
space to a new feature space with low dimensionality. The new constructed feature space is
usually a linear or nonlinear combination of the original feature space. Examples of feature
extraction methods include Principle Component Analysis (PCA) (Jolliffe, 2002), Linear
Discriminant Analysis (LDA) (Scholkopft and Mullert, 1999), Canonical Correlation Analysis
(CCA) (Hardoon et al., 2004), Singular Value Decomposition (Golub and Van Loan,
2012), ISOMAP (Tenenbaum et al., 2000) and Locally Linear Embedding (LLE) (Roweis and Saul,
2000).

> Feature selection, on the other hand, directly selects a subset of relevant features for
the use model construction. Lasso (Tibshirani, 1996), Information Gain (Cover and Thomas,
2012), Relief (Kira and Rendell, 1992a), MRMR (Peng et al., 2005), Fisher Score (Duda et al.,
2012), Laplacian Score (He et al., 2005), and SPEC (Zhao and Liu, 2007) are some of the
well known feature selection techniques.

What a good paper and good package it is! If you have to have to do some feature selection jobs, don't worry but just use this package. It is super-simple and super-effective. If you believe your latent space exist in the subset of the original feature space, go for feature selection method; otherwise, you may apply advanced dimensionality methods, e.g., [t-sne][wiki-tsne]. 

I forgot to introduce a good dimensionality reduction toolbox in Matlab. [Here it is][matlab_dim]. This matlab library contains 34 dimensionality reduction techniques. Enjoy!


[feat]: http://arxiv.org/abs/1601.07996
[pack]: http://featureselection.asu.edu/
[wiki-tsne]: https://en.wikipedia.org/wiki/T-distributed_stochastic_neighbor_embedding
[matlab_dim]: https://lvdmaaten.github.io/drtoolbox/

[S3_FeatureFace]: {{site.imgurl}}/FeatureFace.jpg
[Src_FeatureFace]: http://www.seestorm.com/technologies/cv/ffe_sdk/
[S3_Dim]: {{site.imgurl}}/dimreduct.png
[Src_Dim]: http://research.cs.aalto.fi/pml/software/dredviz/
[S3_Feat]: {{site.imgurl}}/FeatMethods.png
