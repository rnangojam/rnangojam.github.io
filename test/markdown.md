---
sort: 1
---

# ROS 2 환경 구성

> WSL2 및 Ubuntu-20.04 설치


Text can be **bold**, _italic_, or ~~strikethrough~~. [Links](https://github.com) should be blue with no underlines (unless hovered over).

ROS 2는 여러 환경에서 사용이 가능하다. 예를 들어 Ubuntu, Docker, WSL 등등이 있다. 그중에서 WSL2를 이용하여 ROS 2를 사용하기 위한 밑작업을 할 예정이다.


# WSL2 설정

WSL은 Window Subsystem for Linux의 약자로 윈도우 환경에서 리눅스 환경을 사용할 수 있도록 해주는 기능이다. 이를 이용하여 Ubuntu 22.04를 설치할 예정이다.

wSL2를 사용하기 전 해야 할 설정이 있다. Windows 11기준으로 제어판 - 프로그램 및 기능 - Windows 기능 켜기/끄기 - Hyper-V, Linux용 Windows 하위 시스템, 가상 머신 플랫폼에 체크 후 재부팅을 시도한다. 

재부팅 후 Windows 검색에서 Windows PowerShell을 관리자 권한으로 실행한다. 그리고 아래의 코드를 입력한다.

```s
wsl --install     # WSL 설치
wsl -l -o         # WSL에 설치 가능한 Linux 배포판 리스트
wsl --install -d Ubuntu-20.04   # WSL에 Ubuntu 설치
```

설치가 완료되면 ID를 설정하는 멘트가 나오며,  원하는 ID로 설정하면 된다. 그리고 Password를 설정하는데, Password를 입력하면 안보이는 것이 정상이므로 원하는 Password를 입력 후 Enter를 누르면 끝이다. 아래의 코드를 입력하여 Ubuntu-20.04가 나타나면 성공이다.

```s
wsl -l -v         # WSL List 확인 및 version 확인
```



마지막으로 Windows 검색 - Microsoft Store - Ubuntu 검색 후 Ubuntu 20.04 설치 - Username 및 PWD 설정(PWD는 안보이는 것이 정상)을 하면 ROS 2 환경 구성 준비의 준비가 완료되었다. 



# ROS 2 설치

> ROS 2 설치 및 기본 환경 구성

맨 처음으로 Ubuntu를 실행하였다면 관리자 권한(sudo)이 없을 것이다. 다음의 코드로 사용자에게 관리자 권한을 부여하자.

```
usermod -aG sudo 'username'  # username에 관리자 권한 부여
sudo whoami                  # root가 출력되면 성공
```

관리자 권한을 얻었다면 Ubuntu를 Update,Upgrade한다.

Upgrade를 진행하다가 Upgrade에 대해 Y/n을 물어볼수도 있는데, 코드 끝에 -y를 붙이면 자동으로 y가 입력된 것으로 동작한다. 

-y를 붙이지 않아도 y - Enter를 통해 진행이 가능하다.
```
sudo apt update              # Ubuntu Update
sudo apt upgrade -y          # Ubuntu Upgrade
```
ROS 2 설치를 가장 쉽게 하는 방법은 [ROS 공식 가이드](https://docs.ros.org/en/foxy/Installation.html)를 따르는 방법이다.

교재의 방법과 다른 점이 없지만 일단 교재를 따라 가보도록 하겠다.

**1. locale 설정**
```s
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
locale                # en_US.UTF-8로 설정이 되어있는지 확인
```

**2. apt repository 설정**
```s
sudo apt install software-properties-common
sudo add-apt-repository universe

sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
# sudo ~ .gpg까지 한 코드

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
# echo ~ /null까지 한 코드
```

**3. Ubuntu Update, Upgrade**
```s
sudo apt update             
sudo apt upgrade -y 
```
**4. ROS 2 Package 설치(오래 걸림)**
```s
sudo apt install ros-foxy-desktop ros-foxy-desktop-rmw-fastrtps* ros-foxy-rmw-cyclonedds*
```
**5. ROS 2 Package 설치 확인**

```s
source /opt/ros/foxy/setup.bash
ros2 run demo_nodes_cpp talker
```
Ubuntu-20.04를 다른 터미널로 실행시킨 후 다음의 코드를 입력한다.

```s
source /opt/ros/foxy/setup.bash
ros2 run demo_nodes_py listener
```
두 터미널에서 'Hello World'를 주고 받는 것이 보이면 ROS 2 Package를 정상적으로 설치한 것이다.

**6. ROS 개발 tool 설치**
```s
sudo apt update && sudo apt install -y \
  build-essential \
  cmake \
  git \
  libbullet-dev \
  python3-colcon-common-extensions \
  python3-flake8 \
  python3-pip \
  python3-pytest-cov \
  python3-rosdep \
  python3-setuptools \
  python3-vcstool \
  wget
# sudo ~ wget까지 한 코드
```
```s
python3 -m pip install -U \
  argcomplete \
  flake8-blind-except \
  flake8-builtins \
  flake8-class-newline \
  flake8-comprehensions \
  flake8-deprecated \
  flake8-docstrings \
  flake8-import-order \
  flake8-quotes \
  pytest-repeat \
  pytest-rerunfailures \
  pytest
# python3 ~ pytest까지 한 코드
```

```s
sudo apt install --no-install-recommends -y \
  libasio-dev \
  libtinyxml2-dev \
  libcunit1-dev
# sudo ~ -dev까지 한 코드
```
**7. ROS 2 Build test**

ROS 2 설치를 마친 후 ROS 2를 사용할 때 쓸 자신의 Work Space를 build하는 내용이다. 

build가 정상적으로 된다면 ROS 2를 성공적으로 설치를 한 것이다.

robot_ws 대신 원하는 이름을 지어도 상관없다. 

```s
source /opt/ros/foxy/setup.bash
mkdir -p ~/robot_ws/src
cd ~/robot_ws
colcon build --symlink-install
```


**8. Run command 설정**

ROS 2를 사용하다보면 다수의 터미널을 동시에 작업해야 할 때가 있다.

하지만 터미널마다 'source ~'를 설정해야 하는데 여간 귀찮은 일이 아니다. 

그래서 bashrc란 파일을 통해 자동으로 실행되도록 설정할 수 있다.

여기서 nano는 편집기이고 vim, xed 등이 있으며 원하는 편집기를 사용하면 된다. 
```s
sudo apt install nano        # nano 설치
nano ~/.bashrc               # bashrc파일을 편집기로 open
```

파일을 편집기로 열었다면 ctrl + 아래 화살표를 통해 맨 밑으로 커서를 이동시킨다.

그리고 다음의 내용을 추가한다.

```s
source /opt/ros/foxy/setup.bash
source ~/robot_ws/install/local_setup.bash

source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash
source /usr/share/vcstool-completion/vcs.bash
source /usr/share/colcon_cd/function/colcon_cd/bash
export _colcon_cd_root=~/robot_ws

export ROS_DOMAIN_ID=7
export ROS_NAMESPACE=robot1

export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
#export RMW_IMPLEMENTATION=rmw_connext_cpp
#export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
#export RMW_IMPLEMENTATION=rmw_gurumdds_cpp

#export RCUTILS_CONSOLE_OUTPUT_FORMAT='[{severity}{time}][{name}]:{message}({function_name}() at {file_name}:{line_number})'
export RCUTILS_CONSOLE_OUTPUT_FORMAT='[{severity}]:{message}'
export RCUTILS_COLORIZED_OUPUT=1
export RCUTILS_LOGGING_USE_STDOUT=0
export RCUTILS_LOGGING_BUFFERED_STREAM=0

alias cs='cd ~/robot_ws/src'
alias ccd='colcon_cd'

alias cb='cd ~/robot_ws && colcon build --symlink-install'
alias cbs='colcon build --symlink-install'
alias cbp='colcon build --symlink-install --packages-select'
alias cbu='colcon build --symlink-install --packages-up-to'
alias ct='colcon test'
alias ctp='colcon test --packages-select'
alias ctr='colcon test-result'

alias rt='ros2 topic list'
alias re='ros2 topic echo'
alias rn='ros2 node list'

alias killgazebo='killall -9 gazebo & killall -9 gzserver & killall -9 gzclient'

alias af='ament_flake8' 
alias ac='ament_cpplint'

alias testpub='ros2 run demo_nodes_cpp talker'
alias testsub='ros2 run demo_nodes_cpp listener'
alias testpubimg='ros2 run image_tools cam2image'
alias testsubimg='ros2 run image_tools showimage'

# 위의 Run commmand를 필수적으로 설정할 필요는 없다.
```
**9. 통합 IDE 설치**

ROS 2에서 유용한 IDE는 VSCode이다. VSCode의 가장 큰 장점은 여러가지 종류의 프로그래밍 언어를 지원하며 확장(Extension)을 통해 가시성, 효율성을 높일 수 있다는 것이다. 

VSCode는 [공식 홈페이지](https://code.visualstudio.com/)에서 쉽게 다운이 가능하며 Ubuntu 터미널에서 다음의 코드로 동작이 가능하다.

```s
code
```

혹시 좋지 않은 이유로 재설치가 필요한 경우 다음의 코드로 ROS 2를 삭제 가능하다.

```s
sudo apt remove ros-foxy-* && sudo apt autoremove
```
