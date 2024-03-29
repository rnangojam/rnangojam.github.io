---
sort: 2
---

# Turtlesim Package - Node

> Turtlesim Package를 통해 Node를 공부

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

**Turtlesim Package의 Node 실행**

정상적인 ROS2 패키지라면 turtlesim draw_square, turtlesim mimic, turtlesim turtle_teleop_key, turtlesim turtlesim_node 4개의 노드가 출력될 것이다.

여기서 많이 쓰는 노드는 turtlesim turtle_teleop_key, turtlesim turtlesim_node이며 직접 실행시키는 방법은 다음의 코드이다.

```s
ros2 run turtlesim turtlesim_node   
ros2 run turtlesim turtle_teleop_key
```

WSL2 사용자의 경우 위의 코드를 실행시키면 DISPLAY error가 뜰 것이다. 그 이유는 노드를 실행시키면 GUI가 실행되는데 그 GUI를 띄울 DISPLAY를 설정해주지 않아서 이다. 

위의 경우 [X11](https://sourceforge.net/projects/xming/)을 설치하면 해결이 된다. 

설치 후 XLaunch를 실행시켜 DISPLAY number를 0으로 지정한 후 다음

Start no client 후 다음

그냥 다음을 눌러 넘어가도 된다. 2번째 항목을 선택하였으면 후에 터미널에 코드를 한개 더 작성해야 한다.

마침을 누른 후 Windows키 + R로  실행창을 연 후 cmd를 입력해 cmd 터미널을 실행시킨다.

다음의 코드를 입력한다.

```s
ipconfig                           # 연결된 IP를 보여준다. 
```
우리가 관심있는 IP는 **이더넷 어댑터 vEthernet (WSL (Hyper-V firewall))**부분의 IP이다.

IPv4 주소를 복사 후 Ubuntu 터미널로 돌아와 다음의 코드를 입력한다.

```s
export DISPLAY='IP주소':0.0
export LIBGL_ALWAYS_INDIRECT=1     # 2번째 항목 선택 시 입력
```

그 후 다음의 코드를 실행시켜 노드를 실행시키자. 한 코드를 작성시키면 그 터미널에는 코드가 더이상 작성이 안되므로 터미널을 한개 더 열어 코드를 입력할 것.

```s
ros2 run turtlesim turtlesim_node
ros2 run turtlesim turtle_teleop_key
```
turtle_teleop_key를 실행시킨 터미널을 클릭하고 키보드로 움직여보면 잘 움직이는 것을 볼 수 있다.

다음의 코드를 통해 현재 실행중인 노드, 토픽(Topic), 서비스(Service), 액션(Action)을 알 수 있다. 이 코드는 새로운 터미널을 열어 입력하면 된다.

```s
ros2 node list
ros2 topic list
ros2 service list
ros2 action list
```

실행중인 노드를 GUI로 보는 방법이 있다. 아래 코드로 rqt를 설치 후 실행시켜 그래프로 보자.

```s
sudo apt update                             # Ubuntu update
sudo apt install ~nros-foxy-rqt*            # rqt 설치
rqt_graph                                   # rqt 실행
```

현재 작동중인 노드는 총 3개이다. turtlesim, teleop_turtle, rqt_gui_py_node_4204 노드인데, 위에서 언급한 ros2 node list를 통해 알 수 있다. 여기서 rqt는 다양한 툴을 복수로 실행하기 위해 노드 뒤에 프로세스 아이디가 붙는 것을 명심하자. 따라서 아이디가 달라질 수 있다.

만일 동일한 노드를 여러개 실행시키고 싶다면 새로운 터미널에 동일한 코드를 입력하면 된다. 하지만 이는 동일한 노드 이름으로 실행되어 시스템을 구성하는 데 별로 좋은 방법이 아니다. 다음의 코드를 통해 노드명을 변경할 수 있다.

```s
ros2 run turtlesim turtlesim_node __node:=new_turtle     # new_turtle이란 노드 생성
```
그리고 rqt_graph창의 좌측 상단의 새로고침을 누르면 노드가 한개 더 나타날 것이다. 방금 설정한 노드이다.

노드의 정보를 확인하려면 다음의 명령어를 사용하면 된다.
```s
ros2 node info '/노드명'
```
