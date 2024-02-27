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

```
wsl --install     # WSL 설치
wsl -l -o         # WSL에 설치 가능한 Linux 배포판 리스트
wsl --install -d Ubuntu-20.04   # WSL에 Ubuntu 설치
```

설치가 완료되면 ID를 설정하는 멘트가 나오며,  원하는 ID로 설정하면 된다. 그리고 Password를 설정하는데, Password를 입력하면 안보이는 것이 정상이므로 원하는 Password를 입력 후 Enter를 누르면 끝이다. 아래의 코드를 입력하여 Ubuntu-20.04가 나타나면 성공이다.

```
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

1. locale 설정
```
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
locale                # en_US.UTF-8로 설정이 되어있는지 확인
```

2. apt repository 설정
```
sudo apt install software-properties-common
sudo add-apt-repository universe

sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
# sudo ~ .gpg까지 한 코드

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
# echo ~ /null까지 한 코드
```

3. Ubuntu Update, Upgrade
```
sudo apt update             
sudo apt upgrade -y 
```
4. ROS 2 Package 설치(오래 걸림)
```
sudo apt install ros-foxy-desktop ros-foxy-desktop-rmw-fastrtps* ros-foxy-rmw-cyclonedds*
```
5. ROS 2 Package 설치 확인

```
source /opt/ros/foxy/setup.bash
ros2 run demo_nodes_cpp talker
```
Ubuntu-20.04를 다른 터미널로 실행시킨 후 다음의 코드를 입력한다.

```
source /opt/ros/foxy/setup.bash
ros2 run demo_nodes_py listener
```
두 터미널에서 'Hello World'를 주고 받는 것이 보이면 ROS 2 Package를 정상적으로 설치한 것이다.

6. ROS 개발 tool 설치
```
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
```
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

```
sudo apt install --no-install-recommends -y \
  libasio-dev \
  libtinyxml2-dev \
  libcunit1-dev
# sudo ~ -dev까지 한 코드
```
7. ROS 2 Build test

ROS 2 설치를 마친 후 ROS 2를 사용할 때 쓸 자신의 Work Space를 build하는 내용이다. 

build가 정상적으로 된다면 ROS 2를 성공적으로 설치를 한 것이다.

```
source /opt/ros/foxy/setup.bash
mkdir -p ~/robot_ws/src
cd ~/robot_ws
colcon build --symlink-install
```
  

8.


```javascript









```
### Header 3

```
This is a code block following a header.
```

#### Header 4

- This is an unordered list following a header.
- This is an unordered list following a header.
- This is an unordered list following a header.

##### Header 5

1. This is an ordered list following a header.
2. This is an ordered list following a header.
3. This is an ordered list following a header.

###### Header 6

| What    | Follows  |
| ------- | -------- |
| A table | A header |
| A table | A header |
| A table | A header |

---

There's a horizontal rule above and below this.

---

Here is an unordered list:

- Salt-n-Pepa
- Bel Biv DeVoe
- Kid 'N Play

And an ordered list:

1. Michael Jackson
2. Michael Bolton
3. Michael Bublé

And an unordered task list:

- [x] Create a sample markdown document
- [x] Add task lists to it
- [ ] Take a vacation

And a "mixed" task list:

- [ ] Steal underpants
- ?
- [ ] Profit!

And a nested list:

- Jackson 5
  - Michael
  - Tito
  - Jackie
  - Marlon
  - Jermaine
- TMNT
  - Leonardo
  - Michelangelo
  - Donatello
  - Raphael

Definition lists can be used with HTML syntax. Definition terms are bold and italic.

<dl>
    <dt>Name</dt>
    <dd>Godzilla</dd>
    <dt>Born</dt>
    <dd>1952</dd>
    <dt>Birthplace</dt>
    <dd>Japan</dd>
    <dt>Color</dt>
    <dd>Green</dd>
</dl>


<!-- prettier-ignore-end -->

---

Code snippets like `var foo = "bar";` can be shown inline.

Also, `this should vertically align` ~~`with this`~~ ~~and this~~.

Code can also be shown in a block element.

```
var foo = "bar";
```

Code can also use syntax highlighting.

```javascript
var foo = "bar";
```

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```javascript
var foo =
  "The same thing is true for code with syntax highlighting. A single line of code should horizontally scroll if it is really long.";
```

Inline code inside table cells should still be distinguishable.

| Language   | Code               |
| ---------- | ------------------ |
| Javascript | `var foo = "bar";` |
| Ruby       | `foo = "bar"`      |

---

Small images should be shown at their actual size.

![Octocat](https://koreatechackr-my.sharepoint.com/personal/1324werqt_koreatech_ac_kr/_layouts/15/embed.aspx?UniqueId=e65ff94e-d479-4d11-a249-f1a5c3e0ff98)

Large images should always scale down and fit in the content container.

![asdf]("https://koreatechackr-my.sharepoint.com/personal/1324werqt_koreatech_ac_kr/_layouts/15/embed.aspx?UniqueId=e65ff94e-d479-4d11-a249-f1a5c3e0ff98")

![Branching](https://koreatechackr-my.sharepoint.com/personal/1324werqt_koreatech_ac_kr/_layouts/15/embed.aspx?UniqueId=e65ff94e-d479-4d11-a249-f1a5c3e0ff98)

```
This is the final element on the page and there should be no margin below this.
```
