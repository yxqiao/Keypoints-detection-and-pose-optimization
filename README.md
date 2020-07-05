# Q-Learning

This project contains the source code for Q-learning with the DDQN trick. We implement the DDQN in two classific model provided by gyms, CartPole and Acrobot. 

The code is testes in the following enviroment.

- OpenAI Gym’s version of CartPole and Acrobot
- Pytorch

## Keypoint Detection

In the first step, We will train a heatmap-based neural network that given an image of an object, it estimates the location of the keypoints in the image. For this part, we already have the keypoint annotations for each image, so that the training of the network is in a fully supervised manner.

For the keypoint representation, we will use heatmaps. Given an image of input size 256 x 256, the output heatmaps for k keypoints will be a 3D tensor of size k x 64 x 64. This is effectively a collection of k 2D heatmaps with dimensions 64 x 64. Each 2D heatmap corresponds to one keypoint and is responsible to localize this particular keypoint in the image. During training, we synthesize the heatmaps by identifying the location of each keypoint on the 2D image and placing a 2D Gaussian centered on this location on the corresponding heatmap. The Gaussian models our confidence for the location of the keypoint. In other words, points that are close to the actual location have high likelihood, while points that are far from the keypoint location have small likelihood.

Having synthesized these heatmaps, we have created input-output pairs for training. We use RMSprop for the optimization, the learning rate is equal to 2.5e-4, while we set the batch size equal to 8. To avoid overfitting, we also use data augmentation, in the form of scale augmentations and 2D rotations in the input.


## Pose Optimization

For the second step, we use the coordinates of detected keypoints to estimate the 6DoF pose of the object. We need to align the CAD model of the object to the detected 2D keypoints, i.e., estimate the rotation R ∈ R3x3 and the translation T ∈ R3x1 between the object and the camera frame such that the projection of the CAD model aligns with the object in the image.

### Result

The goal of the CartPole is to stay upright as long as possible. Without training the CartPole can easily fall down. With 2000 iterations of training using our network, the CartPole can stay upright.

<p align="center">
  <img src="document/img/carpolefinal2.gif" width="300"><br>
</p>

For each iteration, the mini-batch size I use is 64. I track loss and average cumulative reward evaluated on 50 trajectories for each iteration. I update the target network every 100 iterations. Each time I update the target network, the loss will increase as shown in figure 1. The average reward will increase and become stable over time. 

<p align="center">
  <img src="document/img/avg_reward1.png" width="400"><br>
</p>

 

