<h3>주제</h3>

자율주행 자동차 모델링

<h3>내용</h3>

간단한 수식의 자동차를 모델링하여 다양한 제어 모델을 적용해보고 3D 시뮬레이션을 통해 제어 모델의 정상적인 작동 여부를 확인해보자.

<h3>목차</h3>

주제 1. 속도와 가속도 사이의 간단한 전달함수를 활용하여 차량을 모델링해보자.</br></br>

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

![image](https://user-images.githubusercontent.com/87568714/208416518-fca6efe6-9498-49ca-8343-1145783cde33.png)

올바르게 설계되었는지 확인하기 위해 초기 속도를 10m/s, 초기 가속도를 100m/s, Reference 속도를 20으로,

Saturation 블록은 속도를 positive 영역으로, 가속도를 -3에서 2로 설정하고 시뮬레이션 하였다.</br>

![image](https://user-images.githubusercontent.com/87568714/208418711-95644abb-734b-4c8b-83db-e5a3e84466e0.png)

속도 그래프는 위와 같이 초기 속도 10에서 약간의 overshoot와 함께 20으로 증가하는것을 확인할 수 있다.</br>

![image](https://user-images.githubusercontent.com/87568714/208418935-102fffcc-44c7-4de2-a657-66e5e36f288e.png)

위치 그래프는 100에서 시작하여 5초 이후로는 속도 20에 맞춰 일정하게 증가하는것을 확인할 수 있다.

설계 의도대로 모델링된것을 확인할 수 있었다.
