---
sort: 3
---

# # Turtlesim Package - Topic

> Turtlesim Package를 통해 Topic을 공부

토픽(Topic)이란 비동기식 단방향 메시지 송수신 방식이며 msg 인터페이스 형태의 메시지를 publish하는 Publisher와 메시지를 subscribe하는 subscriber간의 통신이다. 1:1 통신부터 1:N 통신도 가능하며 N:1, N:N도 가능하다. 토픽은 ROS 메시지 통신에서 가장 널리 쓰이는 방법이다.

다음의 코드를 통해 토픽의 정보를 알 수 있다.
```s
ros2 node info
```

