<h3>주제</h3>

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
