<h3>주제</h3>

자율주행 자동차 모델링

<h3>내용</h3>

간단한 수식의 자동차를 모델링하여 다양한 제어 모델을 적용해보고 3D 시뮬레이션을 통해 제어 모델의 정상적인 작동 여부를 확인해보자.

<h3>목차</h3>

주제 1. 속도와 가속도 사이의 역학을 간단한 전달함수로 설정하여 차량을 모델링해보자.</br></br>

주제 2. Simulink에서 제공하는 Adaptive Cruise Control 모델을 적용해보자.</br></br>

주제 3. 위에서 만든 모델을 Simulink에서 제공하는 3D Animation Block을 통해 시뮬레이션 해보자.</br></br>

주제 4. 간단한 차량 측면 동작을 모델링해보자.</br></br>

주제 5. 다른 주행 모드도 적용해보자.</br></br>


<h3>주제 1</h3>

속도와 가속도 사이의 역학을 간단한 전달함수로 설정하여 차량을 모델링해보자.

<h3>모델링 과정</h3>

속도와 가속도 사이 역학은 MathWorks의 Adaptive Cruise Control에서 참고하였다.

아래 그림은 초기 속도와 초기 위치를 입력값으로 받고 Reference 속도에 따라 이후 속도가 제어되는 모델이다.

Saturation Block을 통해 가속도와 속도의 limit을 설정하였다.</br>

![image](https://user-images.githubusercontent.com/87568714/209339540-05c2b1f8-1480-494c-926c-5be759a95a81.png)

올바르게 설계되었는지 확인하기 위해 초기 속도를 10m/s, 초기 가속도를 100m/s, Reference 속도를 20으로,

Saturation 블록은 속도를 positive 영역으로, 가속도를 -3에서 2로 설정하고 시뮬레이션 하였다.</br>

![image](https://user-images.githubusercontent.com/87568714/208418711-95644abb-734b-4c8b-83db-e5a3e84466e0.png)

속도 그래프는 위와 같이 초기 속도 10에서 약간의 overshoot와 함께 20으로 증가하는것을 확인할 수 있다.</br>

![image](https://user-images.githubusercontent.com/87568714/208418935-102fffcc-44c7-4de2-a657-66e5e36f288e.png)

위치 그래프는 100에서 시작하여 5초 이후로는 속도 20에 맞춰 일정하게 증가하는것을 확인할 수 있다.

설계 의도대로 모델링된것을 확인할 수 있었다.

<h3>주제 2</h3>

Simulink에서 제공하는 Adaptive Cruise Control 모델을 적용해보자.

<h3>Adaptive Cruise Control System Block</h3>

Adaptive Cruise Control 블록의 파라미터는 아래 그림과 같이 설정할 수 있다.</br>

![image](https://user-images.githubusercontent.com/87568714/209351415-31a1d2ff-4ded-49ed-bd8b-2a314419fa0b.png)

Ego Vehicle Model에서는 초기속도, 최대속도, 앞차와의 최소거리를 설정할 수 있고,

Adaptive Cruise Controller Constraints에서는 최대 가속도와 최소 가속도를,

Model Predictive Controller Settings에서는 해당 블록의 샘플링 빈,

Controller Behavior에서는 숫자가 1에 가까울수록 응답시간이 빨라지면서 빠른 변화를 보인다.

<h3>모델링 과정</h3>

앞서 설계했던 차량 모델에 Adaptive Cruise Control을 적용하여 차량이 앞선 차량과 자동으로 거리유지를 할 수 있도록 모델링해보자.</br>

![image](https://user-images.githubusercontent.com/87568714/209345521-7e5a3cc2-802b-4d5f-8a9d-8c924f86057f.png)

위 그림은 차량간의 거리유지를 확인하기 위해 차량 모델 두개를 각각 Leading Vehicle과 Following Vehicle으로 설정하였다.

Leading Vehicle의 속도와 차량간의 상대적 거리를 아래와 같이 Following Vehicle의 Adaptive Cruise Control 블록의 입력값으로 받았다.</br>

![image](https://user-images.githubusercontent.com/87568714/209345111-8240df38-5b21-4919-9687-e5477fa2ddf6.png)

시뮬레이션을 위해 Leading Vehicle의 속도는 20m/s, 초기 위치는 100m, Following Vehicle의 속도는 30m/s, 초기 위치는 0m로 설정하였다.

위 조건으로 시뮬레이션 했을때 두 차량간의 거리는 다음과 같았다.</br>

![image](https://user-images.githubusercontent.com/87568714/209346859-abbaf8fe-fe42-4a3f-9493-2c59fcf7b8b1.png)

초기 상대적 거리 100m에서 점점 거리가 줄어들어 대략 38m 정도로 줄어들고 이후로 더이상 거리가 좁혀지지 않는것을 확인할 수 있다.

그렇다면 두 차량의 속도는 어떠할까?</br>

![image](https://user-images.githubusercontent.com/87568714/209347479-5aae89c0-1343-4b70-ada2-64c39aba7bf7.png)

Following Vehicle의 속도가 30m/s 에서 점점 감소하여 20m/s로 수렴한것을 확인할 수 있다.

따라서 두 차량간의 거리가 정상적으로 유지되는것을 확인할 수 있다.














