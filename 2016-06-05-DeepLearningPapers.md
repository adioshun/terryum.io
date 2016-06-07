---
layout:     post
title:      Trends of deep learning researches
date:       2016-06-05 20:00:00
summary:    Trends of deep learning researches
categories: ML_theory
thumbnail:  pencil
tags:
 - Deep learning
 - Survey
---

As some people already know, I have made an awesome-list named [*Most Cited Deep Learning Papers*][L_PaperGit] at my [GitHub repo][L_MyGit]. Although I know some awesome-lists for deep learning researches exist such as [*awesome-deep-learning*][L_A_DL], [*awesome-deep-vision*][L_A_DV], and [*awesome-deep-RNN*][L_A_RNN], anything satisfies my needs to catch up the trend of deep learning researches regardless of the applied area.

* [Link]  [Awesome-Deep-Learning-Papers][L_PaperGit]

That is why I made the  [*awesome-deep-learning-papers*][L_PaperGit] by myself. It was not a work only for others, but also for myself as well. As surveying on deep learning papers, I could realize what I have missed among the recent advances in deep learning researches, and also get some ideas how I can integrate the spread ideas which appear in different fields.

![Awesome deep learning papers][S3_DLPRepo]

Here, I briefly summerazie some trends of deep learning researches that appear in the list of most cited deep learning papers

### People start thinking why deep neural networks work

Enraptured by the amazing performance of deep neural network (DNN), people have been delved into the structure of DNN and improved recognition accuracy for years. For example, AlexNet, VGG Net, NIN, GoogLeNet, and ResNet are the outcomes for that. (See the [neural network models][L_NNModels] that have recently been developed.)

However, we know that we cannot go further without a sound understanding of fundamentals. Researchers recently have found some interesting phenomena, for examples, DNN can be easily fooled [[1]](#11) by some manipulated images and the first layers of CNN learn [Gabor filter][L_Gabor] regardless of tasks or initial weight values [[2]](#22). These interesting findings lead discussions on the transferability of the learned knowledge and the role of adversarial examples [[1]](#11), [[2]](#22). Also theories that explain why CNN is successful are being investigated (e.g. [[3]](#33), [[4]](#44)). I hope theoretical approaches will lead advances in practical approaches, and vice versa.

### From "Going deeper" to "Compressing more lightly"

GoogLeNet and ResNet empirically have proved that deeper networks with more data give better performance in terms of accuracy. However, accuracy may not be the only metric which appreciates the excellency of learning methods. SqueezeNet [[5]](#55) which aims at developing AlexNet-level accuracy with smal size model reveals the three motivations for their work as follows.

> (i) Smaller CNNs require less communication across servers during distributed training, (ii) less bandwidth to export a new model from the cloud to an autonomous car, (iii) and are more feasible to deploy on FPGAs and other hardware with limited memory.

Now, accuracy is not the only metric which deep learning researchers concerns. Based on better knowledge on the mechanisms of CNN, researchers are trying to reduce the size of CNNs. Also researchers are trying to reuse the learned knowledge to another task (e.g. [[6]](#66)) to avoid repeatedly training large data for different tasks. They are all from the better understanding on CNN (or DNN). The better you know about the method, the smaller model (or less training time) you will need.

<div style="text-align: center"><figure><img src="https://s3.amazonaws.com/www.terryum.io/images/DNNFooled.png"><figcaption>CNN can be easily fooled by manipulations [1]</figcaption></figure></div><br/>


### CNN is extending its realm

When DNN was first emerged by Hinton's group researchers, the main players who take a key role in deep learning was unsupervised learning techniques such as RBM and autoendcoder. However, despite of being emphasized by many researchers, unsupervised learning techniques have not been spotlighted for years (except for [[7]](#77)). Instead, CNN is aggressively extending its realm while RNN and LSTM is still taking an important role for sequence learning.

For example, sentences were natural to be analyzed by using RNN (or LSTM) because they can be considered as sequences of words or characters. However, CNN is showing state-of-the-art performance in sentence classification [[8]](#88) thanks to its ability to capture knowledge from windowed data (the data in sliding windows). Also, recognition tasks in videos (e.g. activity recognition) are being tackled by CNN, although videos can be considered as sequences of images. It is true that RNN (or LSTM) is still serving as a key player in sequence learning, e.g., in machine translations, but it is not deniable that CNN proves its usefulness in a variety of areas.

### DNN in robotics

In fact, I'm not a deep learning researcher, but a researcher conducting a research on human and robot motions. Thus, my interest is on the expectable benefits from the advances of DNN to robotics or human motion analysis, rather than DNN itself.

Last year, I found only five papers which use deep neural network from [ICRA2015][L_ICRA2015]. (ICRA is one of the biggest conference in robotics field.) Except for [[9]](#99) from [Abbeel's group][L_Abbeel], Most of them are vision researches which are not quite different from the researches in [CVPR][L_CVPR]. This year, I found around 50 papers from [ICRA2016][L_ICRA2016] and most of them still lean towards vision researches, e.g., 3D object recognition and scene understanding for autonomous vehicles.

<div style="text-align: center"><iframe width="560" height="315" src="https://www.youtube.com/embed/H4V6NZLNu-c" frameborder="0" allowfullscreen></iframe> </div>

<br/>
But some new streams of researches are noticeable. Visual recognition is being combined with grasping strategy under DNN framework as in [[10]](#1010) and [[11]](#1111). Also, DNN is being used for robot control with the combination of policy gradient search [[12]](#1212). It is interesting because traditional control schemes used for humanoids or manipulators are quite different from the policy search framework. Some robotics researcher do not like policy search approaches (which is closely related to RL) because learning from failures is often very expensive in robotics. ~~That is why humanoid researchers are willing to be served as cushion when their humanoid fall down.~~ But the realm of policy search for robot control is getting extended thanks to the benefits of DNN. Where will be the point of compromise between the tradional control and policy search schemes? That would be an interesting viewpoint to look ahead of future robotics.

Recent movements for getting better understanding of DNN will accelerate the advent of future deep learning based robots. Robots are required to make a decision in real time. Decisions are not only about high-level choices, but also about moving limbs, avoiding obstacles, maintaining balance, etc. To achieve them, reducing the size of DNN and transferring learned knowledge for a new task will be essential conditions. Also, I think the other problem, expensive failure cost in robotics, can be eased by combining with [cloud robotics][L_Cloud] or [learning from demonstration (LfD)][L_LfD] techniques. As humans learn knowledge not only from their direct experiences but also from shared knowledge and other's demonstrations, I believe robots will learn knowledge through internet or youtube videos in the near future.

### Reference

<a name="11">[1]</a> Deep neural networks are easily fooled: High confidence predictions for unrecognizable images (2015), A. Nguyen et al.  <br/>
<a name="22">[2]</a> How transferable are features in deep neural networks? (2014), J. Yosinski et al. <br/>
<a name="33">[3]</a> Understanding convolutional neural networks (2016), J. Koushik <br/>
<a name="44">[4]</a> A theoretical analysis of deep neural networks for texture classification (2016), S. Basu et al., <br/>
<a name="55">[5]</a> SqueezeNet: AlexNet-level accuracy with 50x fewer parameters and \< 1MB model size (2016), F. Iandola et al. <br/>
<a name="66">[6]</a> Learning and Transferring Mid-Level Image Representations using Convolutional Neural Networks (2014), M. Oquab et al. <br/>
<a name="77">[7]</a> Generative adversarial nets (2014), I. Goodfellow et al. <br/>
<a name="88">[8]</a> Convolutional neural networks for sentence classification (2014), Y. Kim.  <br/>
<a name="99">[9]</a> Deep learning helicopter dynamics models (2015), A. Punjani et al.  <br/>
<a name="1010">[10]</a> Supersizing Self-Supervision: Learning to Grasp from 50K Tries and 700 Robot Hours (2015), L. Pinto and A. Gupta <br/>
<a name="1111">[11]</a> Learning Hand-Eye Coordination for Robotic Grasping with Deep Learning and Large-Scale Data Collection (2016), S. Levine et al. <br/>
<a name="1212">[12]</a> End-to-End Training of Deep Visuomotor Policies (2016), S. Levine et al. <br/>


[L_PaperGit]: https://github.com/terryum/awesome-deep-learning-papers
[L_MyGit]: https://github.com/terryum
[L_A_DL]: https://github.com/ChristosChristofidis/awesome-deep-learning
[L_A_DV]: https://github.com/kjw0612/awesome-deep-vision
[L_A_RNN]: https://github.com/kjw0612/awesome-rnn
[L_NNModels]: https://github.com/terryum/awesome-deep-learning-papers#network-models
[L_Gabor]: https://en.wikipedia.org/wiki/Gabor_filter
[L_ICRA2015]: http://icra2015.org/
[L_ICRA2016]: http://icra2016.org/
[L_CVPR]: http://www.pamitc.org/cvpr15/
[L_Abbeel]: http://people.eecs.berkeley.edu/~pabbeel/
[L_Cloud]: https://en.wikipedia.org/wiki/Cloud_robotics
[L_LfD]: http://scholarpedia.org/article/Robot_learning_by_demonstration


[S3_DLPRepo]: https://s3.amazonaws.com/www.terryum.io/images/DLPaperRepo.png
[S3_DNNFooled]: https://s3.amazonaws.com/www.terryum.io/images/DNNFooled.png
