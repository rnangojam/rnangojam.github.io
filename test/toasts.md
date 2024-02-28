---
sort: 2
---

# Turtlesim Package - Node, Topic, Service, Action

> Turtlesim Package를 통해 Node, Topic, Service, Action을 공부

ROS 2는 사용자들에게 Node, Topic, Service, Action를 손쉽게 알려주기 위한 튜토리얼과 같은 개념으로 Turtlesim이란 Package를 개발하였다. 

실제로 Turtlesim을 통해 앞서 말한 개념들을 직관적으로 이해할 수 있으며 사용도 어렵지 않아 유용한 Package이다.

앞에서 ROS 2 설치 과정을 따라왔다면 Turtlesim package는 이미 설치되어 있을 것이며, 혹여 설치되어 있지 않더라도 다음의 코드로 설치할 수 있다.

```s
sudo apt update
sudo apt install ros-foxy-turtlesim    # Turtlesim 설치
```

**노드(Node)란?**

노드(Node)는 ROS의 최소 단위의 실행 가능한 프로세스이며 노드를 이용해 프로그램을 재사용성을 극대화 할 수 있다. 그리고 이러한 노드들과 노드의 실행을 위한 정보를 묶어놓은 것을 패키지(Package)라 하며 패키지의 묶음을 메타 패키지(Meta Package)라 한다.

다음의 코드를 입력하면 현재 ROS 2에 설치된 패키지들을 볼 수 있다.

```s
ros2 pkg list                           #설치된 패키지 list
```
앞서 패키지들 안에는 노드가 포함되어 있다 하였는데 다음의 코드로 포함된 노드를 볼 수 있다.

```s
ros2 pkg excutables 'package name'      # 패키지의 노드 확인
```
Turtlesim 패키지의 노드를 확인해 보자

```s
ros2 pkg excutables turtlesim
```



