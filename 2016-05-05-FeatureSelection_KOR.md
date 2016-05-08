---
layout:     post
title:      차원축소와 특징선택이란 무엇인가
date:       2016-05-05 12:00:00
summary:    Kinds of feature selection methods
categories: korean
thumbnail: pencil
tags:
 - dimensionality reduction
 - feature selection
 - feature extraction
---
> 이 게시물은 [Geunho Lee](https://www.facebook.com/lkh2070)님에 의해 번역된 게시물입니다. 번역된 게시물은 [Geunho Lee님의 블로그](http://iostream.tistory.com/110)에서도 확인하실 수 있으며, 여러분들도 [번역 및 게시물 소장에 참여]()하실 수 있습니다.  [[Eng. ver.]](http://terryum.io/ml_theory/ml_practice/2016/05/05/FeatureSelection/)

&nbsp;

## 왜 feature가 필요할까?

&nbsp;
**머신러닝**은 입력 데이터를 출력 데이터로 대응시켜주는 블랙박스라고 대략 설명할 수 있습니다. 이 *매직박스*는 **입력 데이터**의 함수인데 선형 또는 비선형의 형태를 가질 수 있는데, 우리는 훈련 데이터를 사용해서 이 함수를 학습하지만, 항상 잘 학습되지는 않습니다.

예를 들어, 우리가 경기장의 관중 수를 입력으로 해서 해당 야구 경기의 결과를 알고 싶다고 합시다. 경기장의 관중 수만으로 경기의 결과를 정확하게 예측하는 것이 가능할까요? 아마도 관중 수 정보만으로는 경기 결과를 도출하기에는 **충분하지 않을 겁니다.**

앞의 예제를 통해, 머신 러닝의 성능은 **데이터의 양과 질에 굉장히 의존적**이라는 것을 알 수 있습니다. 가장 이상적인 입력 데이터는 부족하지도 과하지도 않은 적확한 정보만 포함합니다. 그렇다면, 우리는 관찰을 통해서 적확한 정보만을 얻을 수 있을까요? 물론 우리가 풀고자 하는 문제에 대한 완벽한 배경(사전) 지식이 있다면 올바른 정보만을 모을 수 있겠지만, 아쉽게도 대부분 상황에선 그렇지 않습니다. 아마도 많은 경우는 풀고자 하는 문제에 충분한 배경 지식이 없기에 우리가 머신 러닝 기법을 적용하려는 것이겠죠.

[![Features of a face image][S3_FeatureFace]][Src_FeatureFace]

따라서, 우리가 보통 취하는 방법은 먼저 *충분한* 데이터를 먼저 모으고 (예를 들어, 우리가 관측하고자 하는 대상에 충분한 개수의 센서를 붙입니다.), 데이터를 모은 후 어떤 feature가 유용한지 아닌지 확인하는 과정을 거칩니다. feature가 유용한지 아닌지 확인하는 과정을 **특징 선택**(feature selection) 또는 **특징 추출**(feature extraction) 이라고 합니다. 이 과정은 기존 입력을 토대로 *새로운 입력 데이터*를 만들기 때문에 보통 learning 과정 전에 실행됩니다. 따라서 이 과정은 머신 러닝 구조에서 핵심적인 전처리 과정 중 하나입니다.

## 차원축소란 무엇인가?
&nbsp;
우리가 굉장히 고차원의 충분한 입력 값을 가지고 있다고 가정합시다. 예를 들어, ```m```개의 가속도계를 실험체에 붙였다면, 각각의 가속도계는 ```acc_x, acc_y, acc_z```를 측정하므로 우리는 ```3m```차원의 데이터를 얻을 수 있습니다. 또한, 여러분이 데이터를 ```t```시간 동안 ```n```샘플에 대해 모았다면 최종적으로 ```n by 3m``` matrix의 입력 데이터를 얻게됩니다.

앞에서 말한 것처럼, 우리는 모든 ```3m```개의 feature들이 필요하지 않을 수도 있습니다; 몇몇 특징들은 다른 특징들의 조합으로 표현 가능하므로 불필요할 수도 있습니다. 따라서 관찰 대상들을 잘 설명할 수 있는 **잠재 공간(latent space)**은 실제 **관찰 공간(observation space)**보다 작을 수 있습니다. 이렇게 관찰 공간 위의 샘플들에 기반으로 잠재 공간을 파악하는 것을 **차원 축소**(dimensionality reduction technique) 라고 합니다. (이런 경우, 샘플들은 잠재 공간상의 점이 *실현된 것*으로 간주할 수 있습니다.)

깊은 내용을 들어가기 전에, **차원축소**는 단순히 데이터의 압축이나 잡음(noise)을 제거하는 것이 아니라는 것을 말씀드리고 싶습니다. 물론 차원축소로 데이터의 압축이나 잡음을 제거하는 효과도 있겠지만, 이것의 가장 중요한 의의는 관측 데이터를 잘 설명할 수 있는 **잠재 공간(latent space)을 찾는 것**입니다.

[![Finding the latent space][S3_Dim]][Src_Dim]

## 특징 선택 vs. 특징 추출
&nbsp;
데이터의 차원을 줄이는 데는 **특징 선택**(feature selection)과 **특징 추출** (feature extraction) 두 가지 방법이 있습니다.

**특징 선택**의 목적은 모든 특징의 부분 집합을 선택해서 간결한 특징 집합을 만드는 것입니다. 예를 들어, 만약 **acc_x** 와 **acc_y** 우리가 점프 높이 결과 예측에 영향이 없다고 생각한다면 우리는 전체 특징 집합에서 해당 특징들을 제거할 수 있을 것입니다. feature selection에서는 이와같이 원본 데이터에서 불필요한 특징들(변수들)을 제거합니다.

이전 예제에서 본 것처럼 특징선택은 **배경(사전) 지식** 을 이용해서 할 수도 있지만, 이것이 모든 상황에서 가능한 것은 아닙니다. 걱정하지 마세요! 우리는 **자동 특징 선택** (automatic feature selection methods)을 사용할 수 있습니다. 해당 방법들은 특징 중 몇 개를 없애보고 개선된다면 성능을 확인해봅니다. 제가 너무 단순하게 설명한 것 같나요? 하지만 이게 대부분의 특징 선택 알고리즘의 기본 동작방식입니다.

반대로 **특징 추출** (feature extraction)은 원본 특징들의 조합으로 새로운 특징을 생성하려고 시도합니다. 예를 들어, **주성분분석(Principal Compnent Analysis)**은 가장 기본적이고 유명한 차원 축소 기법인데, 동작 원리는 데이터로부터 직교 주축을 찾고 모든 데이터를 해당 축에 투영시킵니다. 이 경우, 원본 데이터를 투영된 데이터로 만드는 투영 함수는 결국 원본 특징들의 선형 결합으로 이루어진 새로운 특징을 만드는 것과 같습니다.

[![A general framework of wrapper feature selection methods][S3_Feat]][feat]

특징 선택에 대한 아주 좋은 [리뷰 논문](http://arxiv.org/abs/1601.07996)(J. Li 외, 2016)을 소개합니다. 논문의 저자들이 파이썬과 매트랩에서 쉽게 사용 가능한 [특징선택 패키지](http://featureselection.asu.edu/)를 제공하는 것이 너무나 좋네요. 해당 패키지는 약 40개의 유명한 특징선택 기법들을 포함하고 있습니다.

> [Paper] &nbsp; Li, Jundong, et al. *"Feature Selection: A Data Perspective."*, arXiv:1601.07996, [[link]](http://arxiv.org/abs/1601.07996) (2016).
> [Package]  &nbsp; [scikit-feature by ASU][pack]

그리고 해당 논문에서 특징 선택과 특징 추출에 대해서 잘 설명되어 있는 부분을 인용해봅니다.

> Feature extraction은 고차원의 원본 feature 공간을 저차원의 새로운 feature 공간으로 투영시킵니다. 새롭게 구성된 feature 공간은 보통은 원본 feature 공간의 선형 또는 비선형 결합입니다. Feature extraction의 예로는 Principle Component Analysis (PCA) (Jolliffe, 2002), Linear Discriminant Analysis (LDA) (Scholkopft and Mullert, 1999), Canonical Correlation Analysis (CCA) (Hardoon et al., 2004), Singular Value Decomposition (Golub and Van Loan, 2012), ISOMAP (Tenenbaum et al., 2000) and Locally Linear Embedding (LLE) (Roweis and Saul, 2000)가 있습니다.

> 이와 반대로, Feature selection은 모델 구축에 관련된 feauture들의 부분 집합을 직접적으로 선택합니다. Lasso (Tibshirani, 1996), Information Gain (Cover and Thomas, 2012), Relief (Kira and Rendell, 1992a), MRMR (Peng et al., 2005), Fisher Score (Duda et al., 2012), Laplacian Score (He et al., 2005), and SPEC (Zhao and Liu, 2007)는 잘 알려진 feature selection 기법들입니다.

정말 환상적인 논문과 패키지 아닌가요! 여러분이 특징 선택 작업이 필요하다면, 걱정하지 말고 그냥 이 패키지를 사용하세요. 특징 선택 작업 시 그냥 이 패키지를 사용하시면 됩니다. 진짜 단순하고 효율적인 패키지입니다. 잠재 공간이 원본 feature 공간의 부분집합에 있을 것 같으면 특징 선택 방법을 적용해보세요. 만약 좀더 복잡한 방법을 원하신다면, [t-sne][wiki-tsne] 같은 고급 차원 축소 방법들이 좋은 선택이 될 것 같네요.

Matlab에서 사용할 수 있는 차원 축소 툴박스 소개를 깜빡할 뻔했네요. 이 Matlab 라이브러리는 34개의 차원 축소 방법을 포함하고 있습니다. 유용하게 쓰세요!

> [Matlab Toobox]  &nbsp; [Matlab Toolbox for Dimensionality Reduction][matlab_dim].


[feat]: http://arxiv.org/abs/1601.07996
[pack]: http://featureselection.asu.edu/
[wiki-tsne]: https://en.wikipedia.org/wiki/T-distributed_stochastic_neighbor_embedding
[matlab_dim]: https://lvdmaaten.github.io/drtoolbox/

[S3_FeatureFace]: {{site.imgurl}}/FeatureFace.jpg
[Src_FeatureFace]: http://www.seestorm.com/technologies/cv/ffe_sdk/
[S3_Dim]: {{site.imgurl}}/dimreduct.png
[Src_Dim]: http://research.cs.aalto.fi/pml/software/dredviz/
[S3_Feat]: {{site.imgurl}}/FeatMethods.png
