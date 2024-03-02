---
layout: page
title: Semantic SLAM
description: A Semantic SLAM Pipline Based on ORB-SLAM2 
img: assets/img/Sem_SLAM/cover.png
importance: 4
category: academic
---

In this project, we incorporate semantic information extracted from object detection model [YOLOv5](https://pytorch.org/hub/ultralytics_yolov5/) into the whole pipeline of stereo [ORB-SLAM2](https://github.com/raulmur/ORB_SLAM2) to associate the landmarks with semantic information. The position and semantic information from YOLOv5 are employed for landmark initialization and landmark semantic labeling. Meanwhile, Bayersian update rule is implemented to guarantee more accurate semantic label association to the landmark. ORB-SLAM2 provides the necessary and reliable mapping and localization information for data association. We evaluate our method on the [KITTI](https://www.cvlibs.net/datasets/kitti/eval_odometry.php) stereo dateset sequence00. In the end, a 3D points map with landmarks associated with semantic labels is built. [[code]](https://github.com/Alexander-guo/ese650-final-project)

### Approach

__Semantic Information Extraction__:

To extract the semantic information of the landmarks we are interested in, we use object detection model YOLOv5 to acquire the location of the object and confidence of the class label. The output YOLOv5 are the bounding box corners pixel coordinates which determine the position and dimension of a bounding box as well as the detection confidence $$ P $$  and the class index $$ c $$ of the label in [COCO dataset](https://cocodataset.org/#home), namely $$ \hat{y} = [x_{\min}, y_{\min}, x_{\max}, y_{\max}, P, c] $$. This detection output $$ \hat{y} $$ can help us model the class likelihood of the landmark.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.html path="assets/img/Sem_SLAM/left_cam.gif" title="yolov5" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Object Detection with YOLOv5
</div>

__Correspondence Matching__:

For each frame of stereo images, first we extract `SIFT` features from left and right images and apply `RANSAC` to eliminate outliers. Find the matched correspondences of `SIFT` features in left and right images. Next, For all the features in each of the bounding box in left frame, find the bounding box that their right frame feature correspondences vote most and determine this pair as bounding box correspondence. To alleviate the effect of overlapping bounding boxes, we do a left-right consistency check.

<div class="row justify-content-sm-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.html path="assets/img/Sem_SLAM/overlap_cars.png" title="matching" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Feature Matching in Stereo Vision
</div>

__Landmark Back Projection__:

We model the center of bounding box as the geometric measurement $$ p $$ of a landmark. We have the projection relation:

$$ \lambda p = K [R | T] {^W}P $$

where $$R$$ and $$ T $$ as the camera pose of current frame can be extracted from `ORB-SLAM2`. $$K$$ is the camera intrinsic matrix.
In the setting of stereo vision, the depth of landmark can be then retrieved by applying triangulation to the matches. Given the relation, the back projection of landmark position $${^W}P$$ is solved. 

__Landmark Initialization__:

To determine whether the landmark we see in current frame is a new landmark, we take the strategy: In frame $$k$$, for bounding box $$i$$, we calculate the Euclidean Distance $$ d_i = [d^1_i, \cdots, d^N_i] $$ between the measurement of current frame $$ ^WP^i_k $$ to each of the previous measurements $$ \mathcal{P} = \{^WP^n \}_N $$. We set a threshold $$ \epsilon $$ such that if $$ \forall d_i, d_i > \epsilon $$, then a new landmark is initialized. Otherwise, find the closest landmark $$ L_n $$ we have so far and update its measurement.

__Landmark Measurements Update__:

Assume there are object classes detected from YOLO in the set $$ \mathcal{C} = \{c_m\}^M_{m=1} $$ we are interested in, then the semantic label likelihood distribution of landmark $$ L_n $$ will be given as $$ F(L_n) $$ which is a discrete distribution with the dimension $$ M $$ and all the likelihood $$ F_m(L_n) $$, for $$c_m \in \mathcal{C} $$, should sum up to 1. 

* _Initialization_:
    When we initialize a new landmark $$L$$, from the YOLO detection, we set $$ F_m(L) = P $$, in which $$ P $$ is the
    detection label confidence from YOLO, $$m$$ is the detected label class. And we set all the other classes likelihood to be

    $$ F_{\bar{m}}(L) = \dfrac{1 - P}{M - 1} $$

* _Bayersian Update_:
    For landmark $$ L_n $$ at frame $$ k $$, the update is:

    $$\begin{align*}
        P(c^n_k | F_{1:k}, X_{1:k}) &= \dfrac{1}{Z} P(c^n_k | F_k, X_k) P(c^n_{k-1} | F_{1:k-1}, X_{1:k-1}) \\
        Z &= \sum_{c \in \mathcal{C}}P(c^n_k | F_k, X_k) P(c^n_{k-1} | F_{1:k-1}, X_{1:k-1}) 
    \end{align*}$$

    where $$ X $$ is camera pose.

### Pipeline

The whole pipeline is shown as following:
<div class="row justify-content-sm-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.html path="assets/img/Sem_SLAM/pipeline.png" title="pipeline" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Pipeline
</div>

### ROS Implementation

<div class="row justify-content-sm-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.html path="assets/img/Sem_SLAM/rosgraph.png" title="ROS_graph" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    ROS Nodes and Topics Graph
</div>

### Experiments on KITTI Dataset

<div class="row justify-content-sm-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.html path="assets/img/Sem_SLAM/cover.png" title="results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Results Visualized in RViz
</div>

<!-- The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above: -->
