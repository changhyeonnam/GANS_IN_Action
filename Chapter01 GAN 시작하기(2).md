# Chapter01 GAN 시작하기(2)

Created: Mar 3, 2021 4:55 PM

### 1.3 GAN 시스템

![GANS_IN_Action_images](/GANS_IN_Action_images/Untitled.png)

1. trainig dataset

    dataset serves as input (x) to the Discriminator network.

2. Random noise vector

    raw input (z) to the Generator network. This input is a vector of random numbers that the Generator uses as a starting point for synthesizing fake examples.

3. Generator network

    The Generator takes in a vector of random numbers (z) as input and outputs fake examples (x*). Its goal is to make the fake examples it produces indistinguishable from the real examples in the training dataset.

4. Discriminator

    The Discriminator takes as input either a real example (x) coming from the training set or a fake example (x*) produced by the Generator. For each example, the Discriminator determines and outputs the probability of whether the example is real.

5. Iterative training/tuning

    For each of the Discriminator’s predictions, we determine how good it is—much as we would for a regular classifier—and use the results to iteratively tune the Discriminator and the Generator networks through backpropagation:

- Discriminator의 weight와 bias는 분류 정확도를 최대화 하도록 업데이트 한다. ( x를 진짜로 x* 가짜로)
- Generator의 weight와 bias는 Discriminator가 x*를 진짜로 잘못 분류할 확률을 최대화 하도록 업데이트한다.

### 1.3.1 GAN 훈련하기

GAN 훈련 알고리즘

1. Discriminator 훈련하기
- 훈련 데이터셋에서 랜덤하게 진짜 샘플 x를 선택한다.
- 새로운 랜덤한 잡음 벡터  z를 얻어서 생성자 네트워크를 이용해 가짜 샘플 x*를 합성한다.
- 판별자 네트워크를 이용해 x와 x*를 분류한다.
- 분류 오차를 계산하고 전체 오차를 역전파해서 판별자의 훈련 가능한 파라미터를 업데이트하고 분류 오차를 **최소화** 시킨다.

2. 생성자 훈련하기

- 생성자 네트워크를 사용한 새로운 랜덤한 노이즈 벡터 z에서 샘플 x*를 합성한다.
- Disriminator 네트워크를 이용해 x*를 분류한다.
- 분류 오차를 계산하고 역전파해서 생성자의 훈련 가능한 파라미터를 업데이트하고 판별자의 오차를 **최대화** 한다.

![GANS_IN_Action_images](/GANS_IN_Action_images/Untitled01.png)


### 1.3.2 균형에 도달하기

GAN을 구성하는 두 네트워크는 목표가 서로 다르다. 한 네트워크가 좋아질 때 다른 하나는 나빠진다. 언제 훈련을 중단해야 하는지 어떻게 결정할 수 있을까?

게임이론 (game theory)에 익숙하다면 이런 상황을 한 사람의 이득이 곧 다른 사람의 손해가 되는 제로섬 게임 (zero sum game)으로 인식할 것이다. 모든 제로섬 게임은 참가자 모두 자신의 상황을 더는 개선할 수 없거나, 자신의 행위를 변경함으로써 이익을 볼 수 없는 내시균형(Nash equilibrium)에 도달한다.

GAN은 다음 조건이 충족될 때 내시 균형에 도달한다.

- 생성자가 훈련 데이터셋의 실제 데이터와 구별이 안되는 데이터를 생성한다.
- 판별자가 할 수 있는 최선이 특정 샘플이 진짜 인지 가짜인지 랜덤하게 추측하는 것 뿐이다. 즉 샘플이 진짜 인지 아닌지 50대 50의 확률로 추측합니다.

균형에 도달하면 GAN은 수렴(converged) 했다고 말한다. 실무에서는 GAN의 내시 균형을 찾기가 불가능에 가까운데 이는 nonconvex game의 수렴도달에는 복잡성이 매우 크기 때문이다. 실제로 GAN의 수렴은 GAN은 연구에서 가장 중요한 미결 문제 중 하나이다. 다행이 이 문제가 GAN 연구나 여러 혁신적인 GAN 애플리케이션 개발을 방해하지는 않는다.

### 1.4 왜 GAN을 공부해야 할까?

가장 주목할만한 것은 초현실적인 이미지를 생성해내는 GAN의 능력이다. 예시로 ProGAN을 이용해 생성할 수 있다.

GAN의 또 다른 놀라운 성과는 이미지 대 이미지 image to image 변환이다. GAN은 한 domain 내의 이미지를 다른 이미지로 변환할 수 있다. 말 이미지를 얼룩말 이미지로 바꿀 수 있고, 이를 원래대로 되돌릴 수 있다. 이를 가능하게 하는 모델은 CycleGAN이다.

아마존은 패션 카테고리의 추천시스템에 GAN을 이용해난 실험을 하고 있다. 수없이 많은 의상을 분석해 주어진 스타일에 어울리는 새 아이템을 생성하도록 훈련한다. 의료 연구에서는 진단의 정확도를 개선하고자 GAN을 이용해 합성한 데이터로 데이터셋을 증가시키기도 한다.
