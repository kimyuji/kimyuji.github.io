---
layout: single
title: "[SKT AI Fellowship] ASAP 7번 연구 과제 계획"
date: 2021-06-18 23:00
tag: [Pytorch, VSCode, Github, PnP, Camera_Pose_Estimation, ]
use_math: true
published: false
---


# 문제 정의

### 카메라 위치/방향 추정 기술

해당 연구는 스마트폰을 통해 실시간으로 들어오는 2D 이미지와 Visual Geo-DB를 사용하여 **스마트폰 카메라의 위치/방향 추정 기술을 개발**하는 것 입니다. 스마트폰 카메라로 주변을 촬영하면 사용자의 정확한 위치를 알려주는 기술로, 요즘 주목 받고 있는 자율주행 자동차 및 metaverse와 같은 서비스의 대중화에 큰 역할을 할 수 있습니다. 

![Untitled](https://user-images.githubusercontent.com/68378932/122576304-82990f80-d08c-11eb-9518-ae4cb6c9ebae.png)

스마트폰으로 촬영한 이미지를 기존 DB와 비교하는 과정을 거쳐 스마트폰의 pose를 추정하는 기술로 **PnP-Net**을 활용할 예정입니다. 

위와 같은 단계를 거쳐 최종 output인 카메라의 pose를 반환할 예정이며, 이를 통해 사용자의 정확한 위치를 파악할 수 있습니다. 



### 광고판/간판 검출 기술
![Untitled 1](https://user-images.githubusercontent.com/68378932/122576289-8036b580-d08c-11eb-9023-b9393fb37a11.png)

간판과 같이 실제 사회에는 위치 추정 기술의 성능을 저하시키는 요소들이 분포되어 있습니다. 이러한 방해 요소를 제거하는 것은 pose estimation에서 중요한 처리 과정이라고 할 수 있습니다.  해당 연구는 실내/외 2D 이미지에서 광고판/간판과 같은 성능 저하 요소를 detection 및 segmentation하여 처리할 예정입니다. 저희는 **SpineNet을 backbone으로 한 Mask R-CNN** 모델을 사용하여 실생활 이미지 내 방해요소를 올바르게 검출하고, 이를 통해 알고리즘의 성능 향상을 이끌어낼 수 있습니다. 




# 연구 목표

저희의 연구 목표는 다음과 같습니다.

### 1. **Robust**
카메라 위치 추정 기술에 사용되는 데이터는 2D 특징점과 3D 특징점이 Mismatch된 Outlier가 굉장히 많습니다. Outlier에 대응할 수 있는 **Iteratively Re-Weighted Least Square**을 이용하여 **Robust**한 모델을 개발하는 것이 목표입니다.
### 2. Real-Time
스마트폰 카메라를 통해 들어오는 실시간 이미지를 처리하기 위해 **Real-time** 처리가 필수입니다.
실시간 데이터를 최대한 빠르게 처리할 수 있는 **Low Complexity** 모델을 개발하는 것이 목표입니다.
### 3. Hybrid
Camera Pose Estimation의 Classical Algorithm인 PnP를 Develop하고**,** 이를 딥러닝 네트워크와 결합하여 두 기술의 장점을 모두 활용한 **Hybrid Model**을 개발 하는 것이 목표입니다. 
### 4. End-to-End
위에서 언급한 3가지 목표를 달성할 수 있으면서 튜닝 및 사용자의 개입 없이 한번에 처리하는 **End-to-End** 모델을 개발하는 것이 목표입니다.




# PnP-Net
![Untitled 2](https://user-images.githubusercontent.com/68378932/122576297-82007900-d08c-11eb-9796-a923492356dc.png)

카메라 위치/방향 추정 기술의 메인 모델은 PnP-Net입니다. PnP-Net은 딥러닝과 PnP solver을 결합한 hybrid model입니다. fully connected layer로 이루어진 네트워크를 통해 위치와 방향에 대한 대략적인 예측을 하고, 이는 PnP solver의 initialization 값이 됩니다. IRLS-LM 방법을 활용한 PnP solver의 반복을 통해, PnP-Net은 정확한 위치/방향을 반환합니다. 
 PnP-Net으로 matching된 2D-3D 특징점을 통해 카메라의 pose를 추정할 예정이며, PnP-Net에 연산량을 더 줄일 수 있는 다양한 방법(conv1d의 활용, fully connected layer의 추가 등)을 시도하여 연구목표에 맞는 모델을 개발할 예정입니다. 



# SpineNet : Mask R-CNN
![Untitled 3](https://user-images.githubusercontent.com/68378932/122576301-82007900-d08c-11eb-92db-c10165648f37.png)

광고판/감판 검출 모델로 대표적인 instance segmentation 모델인 Mask-R-CNN 모델을 활용할 예정입니다. SpineNet(Scale-Permuted Model)을 모델의 backbone으로 변경할 예정이며, SpineNet의 모델 구조상 각 block마다 scale level 및 block type을 변경할 수 있어 recognition 및 localization에 적합합니다.  




# 기대효과
- 전망성

  실시간으로 들어오는 이미지를 안정적으로 처리하면서 신뢰성 있는 위치 정보를 제공할 수 있을 것입니다. 또한, 이동 물체에 대한 위치가 추정될 경우, 이를 실내 도보 네비게이션, 위치 공유, Smart Building/Factory, 로봇/드론, 대면적 AR 서비스 등 산업 전반에 활용되어 위치추정 task의 범용성을 높일 수 있습니다. 

- 활용 영역

  - 물체에 대한 3D 모델 생성
  - 실내 로봇(로봇청소기, 서비스 로봇, 자율주행 자동차, 드론 등) 자율 주행
  - 실내 보행자 길 안내
  - AR Smart glass 위치 실시간 추정



# Reference

Sheffer, Roy, and Ami Wiesel. "PnP-Net: A hybrid Perspective-n-Point Network." arXiv preprint arXiv:2003.04626 (2020).

Sarlin, Paul-Edouard, et al. "Superglue: Learning feature matching with graph neural networks." Proceedings of the IEEE/CVF conference on computer vision and pattern recognition. 2020.

He, Kaiming, et al. "Mask r-cnn." Proceedings of the IEEE international conference on computer vision. 2017.

Du, Xianzhi, et al. "Spinenet: Learning scale-permuted backbone for recognition and localization." Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition. 2020.


