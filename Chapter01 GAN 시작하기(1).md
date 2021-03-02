# Chapter01 GAN 시작하기(1)

Created: Mar 3, 2021 12:44 AM

GAN을 소개하고 작동원리를 고수준에서 설명합니다. GAN은 두가지 서로 다른 네트워크 ( 생성자 판별자)로 구성됩니다. 두 네트워크가 경쟁하며 훈련하는 방식을 알아봅니다. 

알고리즘은 체스 챔피언과 대결하고, 주식 가격 변동을 예측하고, 부정한 신용카드 거래인지 분류할 수 있지만, 아마존 알렉사나 애플 시리와는 간단한 대화를 나누기도 어려웠다.

이런 상황은 2014년 몬트리올대학교 연구원 이언 굿펠로가 GAN을 발명한 이후로 완전히 바뀌었따. GAN은 신경망 하나가 아닌 두개의 구분된 신경망을 사용해 실제와 유사한 데이터를 생성합니다. 예를들어 진짜 같은 품질의 가짜 이미지를 생성하거나, 낙서를 사진 같은 이미지로 만들거나, 말 영상을 달리는 얼룩말 영상으로 만들 수 있었습니다. 모두 레이블된 훈련 데이터를 대량으로 준비하지 않아도 가능하다. 

### 1.1 GAN이란?

Generative Adversarial Netwrok (GAN)은 동시에 두 개의 모델을 훈련하는 머신러닝의 종류 입니다. Generator (생성자)는 가짜 데이터를 생성하도록 훈련되고 Discriminator (판별자)는 실제 샘플과 가짜 샘플을 구분하도록 훈련됩니다.

Generative (생성적) 이라는 용어는 이 모델의 목적을 나타 낸다. 즉, 새로운 데이터를 생성하는 것이다. GAN이 생성하기 위해 학습할 데이터는 훈련세트에 따라 결정됩니다. 

Adversarial (적대적) 이라는 용어는 GAN의 뼈대를 이루는 두 모델의 Generator와 Discriminator 사이의 게임 같은 경쟁 구도를 나타낸다. Generator의 목표는 훈련 데이터셋에 있는 실제 데이터와 구분이 안될 정도의 유사한 샘플을 만드는 것이다. Discriminator의 목표는 Generator가 만든 가짜 데이터를 훈련 데이터셋에 있는 실제 데이터와 구별하는 것이다. Generator와 Discriminator는 서로를 이기려는 경쟁을 지속합니다. 

GAN 구현의 복잡성 정도에 따라서 간단한 feed forward neural network (FNN chapter03)에서 Convolutional Neural Network (CNN chapter04)까지 다양하고, U-Net(chapter 09)같이 더 복잡한 것도 가능하다. 

### 1.2 GAN의 동작 방식

GAN을 설명하기 위해 흔히 사용되는 비유로 지폐 위조범(Generator)와 이를 잡으려는 형사(Discriminator)가 있습니다. 위조 지폐가 진짜 같아 보일수록 형사가 위조 지폐를 가려내는 능력도 뛰어나야 합니다. 

Generator의 목표는 훈련 데이터와 구별이 안될 정도로 훈련 데이터셋의 특징을 잘 나타난 샘플을 생성하는 것이다. object recognition algorithm은 이미지의 내용을 파악하기 위해 이미지의 패턴을 학습하지만, generator는 패턴을 인식하는 대신 이미지를 직접 처음부터 만들도록 학습한다. 실제 generator에 주어지는 입력값은 랜덤한 숫자 벡터이다.

Generator는 discriminator의 분류 결과에서 feedback을 받아 학습합니다. Discriminator의 목표는 특정 샘플이 진짜인지 가짜인지 구별하는 것이다. discriminator가 가짜 이미지에 속아 가짜를 진짜로 분류할 때 마다 generator는 자신이 임무를 잘 수행하고 있다는 것을 알게 된다. 반대로 generotor가 만든 이미지가 가짜라는 것을 discriminator가 포착할 때마다 generator는 더 그럴듯한 결과물을 생성해야 한다는 피드백을 받는다. 

Discriminator는 또한 지속해서 성능을 향상한다. Discriminator는 여타 분류기 처럼 자신의 예측과 실제 레이블 간의 차이를 통해 학습합니다. 따라서 generator가 더 그럴듯한 데이터를 만들수록 discriminator도 진짜와 가짜를 더 잘 구별하며 두 네트워크 모두 동시에 지속해서 성능이 향상된다.