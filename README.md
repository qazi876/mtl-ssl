# Multi-task Self-supervised Object Detection via Recycling of Bounding Box Annotations (CVPR 2019)

To make better use of given limited labels, we propose a novel object detection approach that takes advantage of both multi-task learning (MTL) and self-supervised learning (SSL).
We propose a set of auxiliary tasks that help improve the accuracy of object detection. 

[Here is a guide to the source code.](object_detection/README.md)


## Reference

If you are willing to use this code or cite the paper, please refer the following:
```bibtex
@inproceedings{lee2019multi,
 author = {Wonhee Lee and Joonil Na and Gunhee Kim},
 title = {Multi-task Self-supervised Object Detection via Recycling of Bounding Box Annotations},
 booktitle = {Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition},
 year = {2019}
}
```


### CVPR Poster [[pdf](doc/cvpr19_poster_v5.pdf)]

<p align="center">
  <img src="object_detection/g3doc/img/mtl_ssl_detection.png" width=960 height=480>
</p>



## Introduction [[pdf](doc/mtl-ssl.pdf)]


### Multi-task Learning

Multi-task learning (MTL) aims at jointly training multiple relevant tasks with less annotations to improve the performance of each task. 

<p align="center">
  <img src="object_detection/g3doc/img/mtl_1.png" width=300 height=220>
  <img src="object_detection/g3doc/img/mtl_2.png" width=450 height=220>
</p>

[1] [An Overview of Multi-Task Learning in Deep Neural Networks](https://arxiv.org/pdf/1706.05098.pdf)

[2] [Mask R-CNN](https://arxiv.org/pdf/1703.06870.pdf)



### Self-supervised Learning

Self-supervised learning (SSL) aims at training the model from the annotations generated by itself with no additional human effort.


<p align="center">
  <img src="object_detection/g3doc/img/ssl_1.png" width=614 height=220>
  <img src="object_detection/g3doc/img/ssl_2.png" width=220 height=220>
</p>


[3] [Learning Representations for Automatic Colorization](https://arxiv.org/pdf/1603.06668.pdf)

[4] [Unsupervised learning of visual representations by solving jigsaw puzzles](https://arxiv.org/pdf/1703.06870.pdf)



### Annotation Reuse
Reusing labels of one task is not only helpful to create new tasks and their labels but also capable of improving the performance of the main task through pretraining. 
Our work focuses on recycling bounding box labels for object detection.

<p align="center">
  <img src="object_detection/g3doc/img/reuse_1.png" width=394 height=300>
  <img src="object_detection/g3doc/img/reuse_2.png" width=414 height=300>
</p>

[5] [Look into Person: Self-supervised Structure-sensitive Learning and A New Benchmark for Human Parsing](https://arxiv.org/pdf/1703.05446.pdf)

[6] [Mix-and-Match Tuning for Self-Supervised Semantic Segmentation](https://arxiv.org/pdf/1712.00661.pdf)


### Our approach

The key to our approach is to propose a set of auxiliary tasks that are relevant but not identical to object detection. 
They create their own labels by recycling the bounding box labels (e.g. annotations of the main task) in an SSL manner while regarding the bounding box as metadata. 
Then these auxiliary tasks are jointly trained with the object detection model in an MTL way.



## Approach

### Overall architecture

<p align="center">
  <img src="object_detection/g3doc/img/ours_1.png" width=950 height=370>
</p>

It shows how the object detector (i.e. main task model) such as Faster R-CNN makes a prediction for a given proposal box (red) with assistance of three auxiliary tasks at inference.
The auxiliary task models (shown in the bottom right) are almost identical to the main task predictor except no box regressor.
The refinement of detection prediction (shown in right) is also collectively done by cooperation of the main and auxiliary task models.
K is the number of categories.

### 3 auxiliary tasks

<p align="center">
  <img src="object_detection/g3doc/img/ours_2.png" width=343 height=370>
</p>

This is an example of how to generate labels of auxiliary tasks via recycling of GT bounding boxes.

* The multi-object soft label assigns the area portions occupied by each class’s GT boxes within a window.
* The closeness label scores the distances from the center of the GT box to those of other GT boxes.
* The foreground label is a binary mask between foreground and background.


## Results
We empirically validate that our approach effectively improves detection performance on various architectures and datasets.
We test two state-of-the-art region proposal object detectors, including Faster R-CNN and R-FCN,
with three CNN backbones of ResNet-101, InceptionResNet-v2, and MobileNet on two benchmark datasets of PASCAL VOC and COCO.

<p align="center">
  <img src="object_detection/g3doc/img/results_1.png" width=418 height=295>
  <img src="object_detection/g3doc/img/results_2.png" width=418 height=240>
</p>

<p align="center">
  <img src="object_detection/g3doc/img/results_3.png" width=418 height=240>
  <img src="object_detection/g3doc/img/results_4.png" width=418 height=195>
</p>


## Qualitative results

Qualitative comparison of detection results between baseline (left) and our approach (right) in each set.
We divide the errors into five categories (Localization, Classification, Redundancy, Background, False Negative).
Our approach often improves the baseline’s detection by correcting several false negatives and false positives such as 
background, similar object and redundant detection.

<p align="center">
  <img src="object_detection/g3doc/img/qr_1.png" width=520 height=353>
</p>
<p align="center">
  <img src="object_detection/g3doc/img/qr_2.png" width=520 height=353>
</p>
<p align="center">
  <img src="object_detection/g3doc/img/qr_3.png" width=520 height=353>
</p>
<p align="center">
  <img src="object_detection/g3doc/img/qr_4.png" width=520 height=353>
</p>
<p align="center">
  <img src="object_detection/g3doc/img/qr_5.png" width=520 height=353>
</p>

