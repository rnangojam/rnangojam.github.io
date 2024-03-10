---
sort: 4
---

# # Turtlesim Package - Service

> Turtlesim Package를 통해 Service를 공부

서비스(Service)는 동기식 양방향 메시지 송수신 방식이다. 서비스를 요청하는 쪽을 Service Client, 요청받은 서비스를 수행 후 응답하는 쪽을 Service Server라 한다.

다음의 코드로 현재 실행중인 노드의 서비스와 서비스의 형태를 알 수 있다.
```s
ros2 service list                       # 서비스 목록
ros2 service type "service name"        # 서비스 형태
ros2 service list -t                    # 서비스 목록 및 형태
ros2 service find std_srvs/srv/Empty    # 서비스 형태로 서비스 명을 찾기
```
실제로 서비스 서버에게 요청을 해보자. 다음의 코드로 서버에 요청을 할 수 있다.

```s
ros2 service call <service name> <service type> "<arguments>"
```
터미널에 다음과 같이 입력해 teleop key를 활성화시켜 거북이를 움직인 후 /clear 서비스를 요청해보자.
```s
ros2 run turtlesim turtle_teleop_key
ros2 service call /clear std_srvs/srv/Empty  # 다른 터미널에 입력
```

거북이의 궤적이 사라진 것을 알 수 있다.

/kill, /reset, /set_pen, /spawn을 요청해보자.

```s
ros2 service call /kill turtlesim/srv/Kill "name: 'turtle'"
```

/kill 요청을 하게되면 화면에 있던 거북이가 사라지는 모습을 볼 수 있다. 다시 나타내려면 /spawn 아니면 /reset을 사용하면 된다.
```s
ros2 service call /reset std_srvs/srv/Empty
ros2 service call /spawn turtlesim/srv/Spawn "{x: 5.5, y: 9, theta: 1.57, name: 'abcd'}" # 거북이의 위치 지정 및 이름 지정
```

/reset을 사용하게 되면  처음 turtlesim node를 동작시킨 것처럼 중앙에 위치하지만, /spawn을 사용하게 되면 위치와 거북이 이름까지 지정할 수 있다. 그리고 topic list를 실행시키면 해당 거북이의 이름으로 topic이 있는 것을 알 수 있다.

/set_pen은 거북이의 궤적의 색, 크기를 지정할 수 있는 서비스이다.
```s
ros2 service call /abcd/set_pen turtlesim/srv/Setpen "{r: 255, g: 255, b: 255, width: 10}"
```

여기까지 서비스에 대해 알아보았다.