# SlimYOLOv3: Narrower, Faster and Better for UAV Real-Time Applications

This page is for the [SlimYOLOv3: Narrower, Faster and Better for UAV Real-Time Applications](http://arxiv.org/abs/1907.11093)


![img](./table.jpg)


![img](./metrics.jpg)

## Abstract

Drones or general Unmanned Aerial Vehicles (UAVs), endowed with computer vision function by on-board cameras and embedded systems, have become popular in a wide range of applications. However, real-time scene parsing through object detection running on a UAV platform is very challenging, due to the limited memory and computing power of embedded devices. To deal with these challenges, in this paper we propose to learn efficient deep object detectors through performing channel pruning on convolutional layers. To this end, we enforce channel-level sparsity of convolutional layers by imposing L1 regularization on channel scaling factors and prune less informative feature channels to obtain “slim” object detectors. Based on such approach, we present SlimYOLOv3 with fewer trainable parameters and floating point operations (FLOPs) in comparison of original YOLOv3 (Joseph Redmon et al., 2018) as a promising solution for real-time object detection on UAVs. We evaluate SlimYOLOv3 on VisDrone2018-Det benchmark dataset; compelling results are achieved by SlimYOLOv3-SPP3 in comparison of unpruned counterpart, including ~90.8% decrease of FLOPs, ~92.0% decline of parameter size, running ~2 times faster and comparable detection accuracy as YOLOv3. Experimental results with different pruning ratios consistently verify that proposed SlimYOLOv3 with narrower structure are more efficient, faster and better than YOLOv3, and thus are more suitable for real-time object detection on UAVs.


## Demo

[![asciicast](results/result.jpg)](results/demo.mp4)


## Requirements

1. pytorch >= 1.0

2. [darknet](https://pjreddie.com/darknet/yolo/)

3. [ultralytics/yolov3](https://github.com/ultralytics/yolov3)

## Procedure of learning efficient deep objectors for SlimYOLOv3

![img](./procedure.jpg)


### 1. Normal Training

    ./darknet/darknet detector train VisDrone2019/drone.data  cfg/yolov3-spp3.cfg darknet53.conv.74.weights

### 2. Sparsity Training

    python yolov3/train_drone.py --cfg VisDrone2019/yolov3-spp3.cfg --data-cfg VisDrone2019/drone.data -sr --s 0.0001 --alpha 1.0

### 3. Channel Prunning

    python yolov3/prune.py --cfg VisDrone2019/yolov3-spp3.cfg --data-cfg VisDrone2019/drone.data --weights yolov3-spp3_sparsity.weights --overall_ratio 0.5 --perlayer_ratio 0.1


### 4. Fine-tuning

    ./darknet/darknet detector train VisDrone2019/drone.data  cfg/prune_0.5.cfg weights/prune_0.5/prune.weights


## Test

### Pretrained models


### Use darknet for evaluation

    ./darknet/darknet detector valid VisDrone2019/drone.data cfg/prune_0.5.cfg backup_prune_0.5/prune_0_final.weights

### Use pytorch for evaluation

    python3.6 yolov3/test_drone.py --cfg cfg/prune_0.5.cfg --data-cfg VisDrone2019/drone.data --weights backup_prune_0.5/prune_0_final.weights --iou-thres 0.5 --conf-thres 0.1 --nms-thres 0.5 --img-size 608

    python yolov3/test_single_image.py --cfg cfg/yolov3-tiny.cfg --weights ../weights/yolov3-tiny.weights --img_size 608 


## Citation

If you find this code useful for your research, please cite our paper:

```
@article{
  author    = {Pengyi Zhang, Yunxin Zhong, Xiaoqiong Li},
  title     = {SlimYOLOv3: Narrower, Faster and Better for Real-Time UAV Applications},
  journal   = {CoRR},
  volume    = {abs/1907.11093},
  year      = {2019},
  ee        = {https://arxiv.org/abs/1907.11093}
}

```