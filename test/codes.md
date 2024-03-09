---
sort: 3
---

# # Turtlesim Package - Topic

> Turtlesim Package를 통해 Topic을 공부

토픽(Topic)이란 비동기식 단방향 메시지 송수신 방식이며 msg 인터페이스 형태의 메시지를 publish하는 Publisher와 메시지를 subscribe하는 subscriber간의 통신이다. 1:1 통신부터 1:N 통신도 가능하며 N:1, N:N도 가능하다. 토픽은 ROS 메시지 통신에서 가장 널리 쓰이는 방법이다.

다음의 코드를 통해 토픽의 정보를 알 수 있다.
```s
ros2 node info 'node name'        # 현재 노드의 토픽을 확인
ros2 topic list -t                # 현재 동작중인 모든 노드의 토픽을 확인
```
토픽의 관계를 rqt를 통해 visualize할 수 있다. 이를 통해 토픽의 퍼블리시와 서브스크라이브 상태를 확인할 수 있다.

```s
rqt_graph
```

다음의 코드로 해당 토픽의 정보를 확인할 수 있다.

```s
ros2 topic info 'topic name'       # 토픽의 퍼블리셔 및 서브스크라이버 정보 확인
```

토픽 간 메시지의 대역폭은 다음과 같다.

```s
ros2 topic bw 'topic name'         # 토픽의 초당 대역폭
```

전송 주기와 지연시간은 다음과 같다.

```s
ros2 topic hz 'topic name'         # 토픽의 전송 주기
ros2 topic delay 'topic name'      # 토픽의 지연시간
```

토픽을 퍼블리시하여 터틀심의 거북이를 마음대로 조종할 수 있다.

```s
ros2 topic pub <topic name> <msg type> "<args>"
```

다음 코드로 거북이를 움직여 보자

```s
ros2 topic pub --once /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```

거북이가 원 궤적으로 움직이는 것을 볼 수 있다. 여기서 --once 대신 --rate 1을 사용하면 1Hz주기가 되어 계속해서 원 궤적을 그리며 회전하는 것을 알 수있다.

이러한 토픽을 파일 형태로 저장하여 불러올 수 있는 기능이 있다. 바로 bag 기록인데 이를 이용하여 알고리즘을 개선하거나 분석할 수 있다.

```s
ros2 bag record <topic name1> <topic name2> <topic name3>
ros2 bag info "rosbag 파일 이름"        # bag 정보
ros2 bag play "rosbag 파일 이름"        # bag 재생
```

Ctrl + C로 녹화 종료를 할 수 있다.

