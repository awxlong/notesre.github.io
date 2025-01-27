---
layout: distill
title: "Review of the paper Learning biophysical determinants of cell fate with deep neural networks"
date: 2023-11-02 19:42
description: comments on a paper that leverages deep learning to classify epithelium cell fate by observing its live image trajectory. 
tags: research
categories: blog-post
disqus_comments: true
related_posts: true
authors:
  - name: Xuelong An
toc:
  - name: Brief summary
  - name: My comments and future research directions
thumbnail: /assets/img/biophysical-mitosis.png
bibliography: deep-med.bib
---

# Brief summary 

The paper by <d-cite key="dsa_2023_prediction"></d-cite> leverages deep learning architectures to solve a pentanary classification task given either a cell's tabular features or images. The five independent classes are one healthy control and four subtypes of Parkinson's Disease: familial proteinopathy (SNCA), environmental proteinopathy ($$\alpha$$-Syn oligomer), and two subtypes characterized by different mitochondria dysfunction pathways. These pathologies were chemically induced on stem cells. Fifty-six phenotypical features of them were extracted automatically and recorded as tabular data, along with images of the cells extracted via microscopy. Both data modalities were labeled with one of the five classes. 

The research team trained separately a dense feedforward neural network (DNN) to classify on the tabular data, as well as a convolutional neural network (CNN) to classify on image data. The test classification accuracy achieved by the DNN reached around 83%, while the CNN 95%. 

<figure>
  <img src="/assets/img/parkinson.png" alt="Sorry. Image couldn't load." width="100%" height="auto">
  <figcaption id="bottleneck">Two separate models are trained on different datasets on the same task of Parkinson subtype classification. Figure extracted from the original research article at https://www.nature.com/articles/s42256-023-00702-9</figcaption>
</figure>
 
# My comments and future research directions

Generally, in the deep learning literature, it is acknowledged that the usage of DNNs comes at the expense of poor explainability. Despite achieving high classification accuracy, these models are black-boxes. Nonetheless, there are ways to identify what are the features that the neural networks pay the most attention when deciding on a classification label, mainly by looking at its last layer's activation and tracing back to the input space which input feature is associated to it. In CNNs, the technique employed by the research team is called the ShAP (SHapley Additive exPlanations) method. 

The authors managed to identify in both the DNN and CNN that the mitochondria, lysosome and the interaction of both features mainly contributed to the classification decisions of both models. 

One future research direction concerns whether integrating both data sources can improve performance and yield explainability, because the original work trains separate models, trained on different datasets. 

One source of inspiration is from <d-cite key="li_2023_v1t"></d-cite>, where they integrate image data along with a mouse's behavioral features to predict its neural responses collected from neural recordings. Another source of inspiration is drawn from concept-bottleneck models  <d-cite key="koh_2020_concept"></d-cite>. There, a CNN in charge of processing images doesn't learn to output a classification label, but instead to output features that are relevant to the image. These features, in turn, are annotations of the image stored in tabular:

<figure>
  <img src="/assets/img/bottleneck.png" alt="Sorry. Image couldn't load." width="100%" height="auto">
  <figcaption id="bottleneck">A depiction of the pipeline of a concept-bottleneck model. The first half outputs a set of concepts given an image, which can be learnt from intricate annotations, or metadata, of the image. The second half outputs a classification label. Figure extracted from the original paper </figcaption>
</figure>

Altogether, with regards to the work by <d-cite key="dsa_2023_prediction"></d-cite>, one interesting extension to their CNN is to have it not predict a Parkinson subtype, but rather learn to predict the cell's physiological features stored as tabular data given image input. Subsequently, use the features to train a multi-class regressor using standard softmax to output a classification label. The prospect is that this hybrid model can leverage the high accuracy prediction of the CNN, whilst being explainable thanks to the logistic regressor. 

As a further improvement, we can use a [Slot Transformer](https://arxiv.org/abs/2210.11394) instead of the CNN with the hope of learning a disentangled representation given the image with its annotations. However, the architecture will be more computationally expensive. A pretrained Slot Transformer that already learnt to disentangle CLEVR-Scenes may be more powerful than training it from scratch.    