---
layout: page
title: Un-EvMoSeg 
description: Unsupervised Event-based Independent Motion Segmentation 
img: assets/img/UnEvMoSeg/figure1_v1.png
importance: 1
category: academic
---

### Abstract

Event cameras are a novel type of biologically inspired vision sensor known for their high temporal resolution, high dynamic range, and low power consumption. Because of these properties, they are well-suited for processing fast motions that require rapid reactions. Although event cameras have recently shown competitive performance in unsupervised optical flow estimation and in visual odometry when combined with frame-based cameras, performance in detecting independently moving objects (IMOs) is lacking behind, although this is a task where event-based methods would be very useful based on their low latency and HDR properties. Previous approaches to event-based IMO segmentation have been heavily dependent on labeled data.  However, biological vision systems have developed the ability to avoid moving objects through daily tasks without being given explicit labels. In this work, we propose the first event framework that generates IMO pseudo-labels using geometric constraints. Due to its unsupervised nature, our method can handle an arbitrary number of not predetermined objects and is easily scalable to datasets where expensive IMO labels are not readily available. We evaluate our approach on the EVIMO dataset and show that it performs competitively with supervised methods, both quantitatively and qualitatively.


### Pipeline

<div class="row justify-content-center">
    <div class="col-sm-8">
        {% include figure.html path="assets/img/UnEvMoSeg/Figure2.png" title="pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Pipeline of Un-EvMoSeg
</div>

__Left Dotted Box__: we train a network to directly predict IMO masks from events. __Rest of Figure__: we
use a geometric self-labeling method to generate binary IMO pseudo-labels that supervise the IMO segmentation network. Our framework
uses off-the-shelf optical flow (fine-tuned on image-based flow) and input depth. The camera motion fitted from flow and depth through
RANSAC is used to compute rigid flow from the camera only. Pseudo-labels are generated through adaptive thresholding techniques based
on the magnitude of estimated IMO motion field. Running inference Un-EvMoSeg is simple without parameter turning because while the
training process requires geometry-based labels, only events are used for prediction. We take the best of both worlds of deep learning and
optimization: `1)` simple and robust inference with a simple feed-forward pass, and `2)` scalable with no expensive annotations required to
train the network. 




### Results

<div class="row justify-content-center">
    <div class="col-sm-8">
        {% include figure.html path="assets/img/UnEvMoSeg/qual_v1.png" title="result" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Qualitative Segmentation Results of EMSGC, SpikeMS and our Un-EvMoSeg. Our unsupervised approach outperforms EMSGC and SpikeMS, and compares competitively against the supervised CNN approach.
</div>
