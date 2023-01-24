<h2>주제</h2>

자율주행 자동차 모델링

<h2>내용</h3>

간단한 수식의 자동차를 모델링하여 다양한 제어 모델을 적용해보고 3D 시뮬레이션을 통해 제어 모델의 정상적인 작동 여부를 확인해보자.

<h2>목차</h2>

주제 1. 속도와 가속도 사이의 역학을 간단한 전달함수로 설정하여 차량을 모델링해보자.</br></br>

주제 2. Simulink에서 제공하는 Adaptive Cruise Control 모델을 적용해보자.</br></br>

주제 3. 차량 간 거리에 따라 다른 주행 모드도 적용해보자.</br></br>

주제 4. 위 모델을 Simulink에서 제공하는 3D Animation Block을 통해 시뮬레이션 해보자.</br></br>


<h2>주제 1 : 기본 차량 모델 설계하기</h2>

속도와 가속도 사이의 역학을 간단한 전달함수로 설정하여 차량을 모델링해보자.

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

설계 의도대로 모델링된것을 확인할 수 있었다.</br></br></br>

<h2>주제 2 : Adaptive Cruise Control 적용하기</h2>

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

따라서 두 차량간의 거리가 정상적으로 유지되는것을 확인할 수 있다.</br></br></br>

<h2>주제 3 : 차량 간 거리에 따른 다른 주행모드 적용하기</h2>

Adaptive Cruise Model이 차량간의 거리를 일정하게 유지시켜준다면 차량간의 거리가 매우 먼 경우에는 ACC의 필요성이 떨어진다.

따라서, 차량간의 거리가 일정 거리 이상인 경우는 ACC를 종료하고 일정 거리 미만 일때는 다시 작동하는 모델을 설계해보자.

![image](https://user-images.githubusercontent.com/87568714/214213761-5facbb02-0a71-49ef-8bbb-229c6c35aa77.png)

StateFlow를 이용하여 차량간의 거리가 60m보다 큰 경우는 ACC를 종료하고, 60m이하인 경우는 ACC를 켰으며, 

30m이하로 거리가 떨어지면 급감속하도록 브레이크를 작동하도록 설계하였다.

![image](https://user-images.githubusercontent.com/87568714/214213995-ad8d1f48-8837-4a4e-89d1-e2770257d58b.png)

최종적인 차량의 모델링은 위와 같다. Multiport Switch를 이용하여 조건에 StateFlow를 적용하고 각각의 port에 주행모드를 입력하였다.

이후 모델을 10초간 시뮬레이션하여 Multiport Switch의 출력값과 차량간의 거리를 살펴보았다.

![image](https://user-images.githubusercontent.com/87568714/214216472-479cf2c3-75a2-4297-b3d3-a7b5072cab61.png)

![image](https://user-images.githubusercontent.com/87568714/214216257-0dea2a0d-604c-4223-9571-a754140a3366.png)

차량간의 거리가 100m에서 시작하여 2초후 60m가 되어 ACC가 켜졌고 4초후 30m가 되어 브레이크가 작동되었으며 대략 9.3초후 다시 60m를 넘어 ACC가 종료된것을 확인할 수 있다.<br/><br/><br/>

<h2>주제 4 : 3D Animation Block으로 테스트하기</h2>

VR SINK라는 블록을 활용하여 앞서 설계한 모델을 시뮬레이션 해보자.

먼저 VR SINK 블록에 사용한 가상세계 파일은 https://github.com/MustafaSaraoglu/AutonomousVehicleModeling 의 PlatoonWorld.WRL 파일을 사용하였다.

15초간 시뮬레이션하였으며 아래와 같이 차량간의 거리가 점점 가까워지자 뒤 차량의 속도가 점점줄어 이후 다시 출발하는것을 확인할 수 있다.

![제목 없는 동영상 - Clipchamp로 제작 (1)](https://user-images.githubusercontent.com/87568714/214252492-98ab0292-585f-46be-bab2-2e09ceb4682c.gif)


