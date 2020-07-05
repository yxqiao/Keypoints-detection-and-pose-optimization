# Keypoint Detection and Pose Optimization

This project contains the source code for Keypoint detection and pose optimization problem.

The code use the following package.

- Numpy
- Pytorch

## Keypoint Detection

In the first step, We will train a heatmap-based neural network that given an image of an object, it estimates the location of the keypoints in the image. For this part, we already have the keypoint annotations for each image, so that the training of the network is in a fully supervised manner.

For the keypoint representation, we will use heatmaps. Given an image of input size 256 x 256, the output heatmaps for k keypoints will be a 3D tensor of size k x 64 x 64. This is effectively a collection of k 2D heatmaps with dimensions 64 x 64. Each 2D heatmap corresponds to one keypoint and is responsible to localize this particular keypoint in the image. During training, we synthesize the heatmaps by identifying the location of each keypoint on the 2D image and placing a 2D Gaussian centered on this location on the corresponding heatmap. The Gaussian models our confidence for the location of the keypoint. In other words, points that are close to the actual location have high likelihood, while points that are far from the keypoint location have small likelihood.

Having synthesized these heatmaps, we have created input-output pairs for training. We use RMSprop for the optimization, the learning rate is equal to 2.5e-4, while we set the batch size equal to 8. To avoid overfitting, we also use data augmentation, in the form of scale augmentations and 2D rotations in the input.


## Pose Optimization

For the second step, we use the coordinates of detected keypoints to estimate the 6DoF pose of the object. We need to align the CAD model of the object to the detected 2D keypoints, i.e., estimate the rotation R ∈ R3x3 and the translation T ∈ R3x1 between the object and the camera frame such that the projection of the CAD model aligns with the object in the image.

### Result

Ground truth image key points:

<p align="center">
  <img src="/img/GT of keypoitns.png" width="300"><br>
</p>

Prediction heatmaps:

<p align="center">
  <img src="/img/predicted heatmaps.png" width="400"><br>
</p>

Pediction image key points:
<p align="center">
  <img src="/img/prediction of keypoints.png" width="400"><br>
</p>

Generated examples:

<p align="center">
  <img src="/img/pose optimization.png" width="400"><br>
</p>

