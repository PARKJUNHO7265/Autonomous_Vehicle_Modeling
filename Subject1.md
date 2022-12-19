<h3>주제</h3>

속도와 가속도 사이의 역학을 간단한 전달함수로 설정하여 차량을 모델링해보자.

<h3>모델링 과정</h3>

속도와 가속도 사이 역학은 MathWorks의 Adaptive Cruise Control에서 참고하였다.

아래 그림은 초기 속도와 초기 위치를 입력값으로 받고 Reference 속도에 따라 이후 속도가 제어되는 모델이다.

Saturation Block을 통해 가속도와 속도의 limit을 설정하였다.</br>

![image](https://user-images.githubusercontent.com/87568714/208416518-fca6efe6-9498-49ca-8343-1145783cde33.png)
