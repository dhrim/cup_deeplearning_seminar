# 개요

GCP(Google Colud Platform)에 서버를 생성하고 docker 하에서 개발환경 구축하는 방법을 기술한다.

GCP GCE + python3 + TensorFlow1.5 + Docker + jupyter

2019/07 현재 TensorFlow2.0의 Docker 이미지가 이미 존재한다. 하지만 일부로 특정 버전 1.5의 것으로 구축해 본다.


# GCP 계정 생성

gmail 계정이 있으면 GCP 계정이 있는 것이다.

없으면 https://gmail.com으로 가서 계정을 생성한다.

 <br>

GCP 홈으로 가서 사용 승인을 한다.

https://console.cloud.google.com



# GCE 생성

GCE(Google Compute Engine)는 GCP에 가상 머신을 의미한다.

GCP에 생성된 서버를 의미한다.



<br>

구글 계정으로 로그인한 후 GCP console 홈으로 간다.

https://console.cloud.google.com



 <br>

좌측 메뉴에서 ‘Compute Engine’ > ‘VM instances’를 클릭

![vm_instance](/img/gcp/vm_instance.png)



<br>

‘Create’ 클릭

![create](/img/gcp/create.png)



<br>

‘Marketplace’를 클릭

![marketplace](/img/gcp/marketplace.png)



<br>

상단 검색창에 ‘deep learning’ 입력하고 엔터.

검색된 결과에서 ‘Deep Learning VM’ 클릭

![deeplearningvm](/img/gcp/deeplearningvm.png)



<br>

‘LAUNCH ON COMPUTE ENGINE’ 클릭

![launch](/img/gcp/launch.png)



<br>

가격은 다음과 같다.

![price](/img/gcp/price.png)



<br>

프로젝트를 선택. 여기서는 기존에 생성되어 있던 ‘dhrim-test’를 선택. 그리고 하단의 ‘OPEN’ 클릭.

![open_configure](/img/gcp/open_configure.png)



<br>

다음과 같이 설정

- deployment name : ‘deeplearning-practice'

- GPU ‘install NVIDIA GPU …’를 체크

나머지는 default 그대로 하고 좌측 하단의 ‘Deploy’를 클릭.


![configure](/img/gcp/configure.png)


# Filewall Rule 생성
요 룰은 매 GCE 생성마다 할 필요 없고, 오직 1번만 생성하면 된다.

GCE에서 구동된 jupyter의 포트 8888로 접근 가능하도록 설정하는 것이다.

<br>


GCP console의 좌측 메뉴 ‘VPC network’ > ‘Filewall rules’ 클릭.

상단의 ‘CREATE FIREWALL RULE’ 클릭

![create_filewall](/img/gcp/create_filewall.png)



<br>

Name에 ‘default-allow-8888’ 입력



Targets를 ‘All instances in the network’으로 선택.

Source Ip ranges에 ‘0.0.0.0/0’ 입력.

‘Protocols and ports’ > ‘Specified protocols and ports’ 선택하고 ‘tcp’ 체크. 값에 8888 입력.

![firewall_configure](/img/gcp/firewall_configure.png)

하단 ‘Create’ 클릭.





# GCE 생성
‘Compute Engine’ > ‘VM Instance’ 메뉴 선택.

‘Create’ 버튼 클릭.



<br>

‘Market Place’ 메뉴 선택하고, 상단 검색창에 ‘TensorFlow CPU Production’로 검색.



‘TensorFlwo CPU Production’ 클릭



스펙과 가격은 다음과 같다.

![gce_price](/img/gcp/gce_price.png)



<br>

TensorFlow는 어짜피 docker로 다시 설치해야 한다. 단지 CE를 편하게 생성하자는 것.

‘LAUNCH ON COMPUTE ENGINE’ 클릭.

이름만 ‘tensorflow1-10-python3-cpu-1’으로 하고, 전부 default값을 사용하여 ‘Deploy’ 버튼 클릭.


# SSH 연결

메뉴 ‘Compute Engine’ > ‘VM 인스턴스’에서 해당 인스턴스 선택하고 우측 ‘SSH’ > ‘브라우저 창에서 열기’ 클릭.

![open_ssh](/img/gcp/open_ssh.png)



<br>

아래 명령들은 살펴보기 위한 것들.

```
dhrim00@tensorflow1-10-python3-cpu-1-vm:/jet/prs$ pwd
/jet/prs

dhrim00@tensorflow1-10-python3-cpu-1-vm:/jet/prs$ su -
Password:
su: Authentication failure

dhrim00@tensorflow1-10-python3-cpu-1-vm:/jet/prs$ sudo ls -al /
total 88
...

dhrim00@tensorflow1-10-python3-cpu-1-vm:/jet/prs$ python
Python 3.6.3 (default, Feb  1 2018, 01:19:17)
>>> import tensorflow as tf
>>> print(tf.__version__)
1.10.0
>>> exit()

dhrim00@tensorflow1-10-python3-cpu-1-vm:/jet/prs$ docker
bash: docker: command not found

dhrim00@tensorflow1-10-python3-cpu-1-vm:/jet/prs$ lsb_release -a
No LSB modules are available.
Distributor ID: Debian
Description:    Debian GNU/Linux 9.5 (stretch)
Release:        9.5
Codename:       stretch

dhrim00@tensorflow1-10-python3-cpu-1-vm:/jet/prs$ uname -m
x86_64
```

sudo 실행 가능. root 비번은 모르겠고, python3.6, tensorflow1.10 있고

docker는 설치 안되어 있다.


<br>

Debian 9.5, 아키텍쳐는 x86_64


# Docker 설치

Utuntu가 아니라 Debian이라서 문서 https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-debian-9를 참조.

별다른 오류 없이 잘 설치된다.

```
$ sudo apt update
$ sudo apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common
$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
$ sudo apt update
$ apt-cache policy docker-ce
$ sudo apt install docker-ce
$ sudo systemctl status docker

$ docker --version
Docker version 18.09.6, build 481bc77
```

sudo 권한 주기
```
$ sudo usermod -aG docker ${USER}
```

그리고 다시 접속


```
$ id -nG
dhrim00 adm dip video plugdev docker google-sudoers
```
그룹 docker에 포함되어 있다.




# Docker 실행
이미지는 tensorflow/tensorflow:1.4.0-py3

이미지 이름은 https://hub.docker.com/r/tensorflow/tensorflow/tags?page=14 에서 찾았다.



실행 명령
```
$ docker run -it -p 8888:8888 -v ${HOME}/notebooks:/notebooks tensorflow/tensorflow:1.4.0-py3
```


실행 시키면 디펄트로 jupyter가 실행된다.

요건 docker 이미지 tensorflow/tensorflow의 설정.
```
dhrim00@tensorflow1-10-python3-cpu-1-vm:/jet/prs$ docker run -it -p 8888:8888 -v ${HOME}/notebooks:/notebooks tensorflow/tensorflow:1.4.0-py3

1.4.0-py3: Pulling from tensorflow/tensorflow

...

Status: Downloaded newer image for tensorflow/tensorflow:1.4.0-py3

[I 04:02:47.529 NotebookApp] The Jupyter Notebook is running at:
[I 04:02:47.529 NotebookApp] http://[all ip addresses on your system]:8888/?token=d134c088f109a0c889eba2f2a884a4459fa815587f1a5931
[I 04:02:47.529 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 04:02:47.530 NotebookApp]

    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8888/?token=d134c088f109a0c889eba2f2a884a4459fa815587f1a5931
```

Ctrl+p, Ctrl+q를 하면 jupyter를 중지하지 않고 host shell로 동라올 수 있다.

Ctrl+c하면, jupyter가 죽은 채로 host shell로 온다.

<br>


다음과 같이 실행되고 있고, CE의 외부로 8888 포트로 listen하고 있음을 볼 수 있다.

```
dhrim00@tensorflow1-10-python3-cpu-1-vm:/jet/prs$ docker ps
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS              PORTS                              NAMES
ce728e11b17c        tensorflow/tensorflow:1.4.0   "/run_jupyter.sh --a…"   5 minutes ago       Up 5 minutes        6006/tcp, 0.0.0.0:8888->8888/tcp   thirsty_haslett

dhrim00@tensorflow1-10-vm:/jet/prs$ netstat -an | grep 8888
tcp6       0      0 :::8888                 :::*                    LISTEN
```

여기 까지 하면 SSH 콘솔을 닫아도 계속 실행되고 있다. SSH 콘솔 닫아도 juptyer 사용하는데 문제 없다는 얘기다.



# Docker내의 설치

git, wget, protoc 설치
```
root@d7a458e07030:/notebooks# apt upgrade
root@d7a458e07030:/notebooks# apt install git
root@d7a458e07030:/notebooks# apt install wget
root@d7a458e07030:/notebooks# apt install protobuf-compiler
 ```


# Jupyter 연결

외부 ip 찾기

```
$ curl -H "Metadata-Flavor: Google" http://169.254.169.254/computeMetadata/v1/instance/network-interfaces/0/access-configs/0/external-ip
35.224.12.113
```
혹은 GCP console의 VM instance 리스트에서

![vm_list](/img/gcp/vm_list.png)



<br>

요 경우는 35.224.12.113이다.



웹브라우저에서 다음 url 호출

http://35.224.12.113:8888



암호는 SSH 콘솔에서 다음 명령으로 알수 있다.
```
$ docker exec a8841d97109c jupyter notebook list
Currently running servers:
http://localhost:8888/?token=9770cdab3ee3f69b4f7af43a36e8d042937a1bb1df4739d2 :: /notebooks
```

'token=' 다음의 문자열이 토큰이다.



‘Password or token’에 요 토큰을 입력해준다.


![jupyter_auth](/img/gcp/jupyter_auth.png)



<br>

다음은 실행된 모습

![jupyter_look](/img/gcp/jupyter_look.png)



<br>


Docker 팁들


Docker VM에 연결
다음과 같은 명령어로 Docker에 접속할 수 있다.

```
dhrim00@tensorflow1-10-vm:/jet/prs$ docker ps
CONTAINER ID        IMAGE                        ...
c40ac72ad4a6        tensorflow/tensorflow:1.4.0  ...

dhrim00@tensorflow1-10-vm:/jet/prs/notebooks$ docker exec -it c40ac72ad4a6 /bin/bash
root@c40ac72ad4a6:/notebooks# ps -ef | grep jupyter
root         1     0  0 06:12 pts/0    00:00:00 bash /run_jupyter.sh --allow-root
root         6     1  0 06:12 pts/0    00:00:00 /usr/bin/python /usr/local/bin/jupyter-notebook --allow-root
root        23    13  0 06:17 pts/1    00:00:00 grep --color=auto jupyter
root@c40ac72ad4a6:/notebooks# pwd
/notebooks
root@c40ac72ad4a6:/notebooks# touch new_file.txt
```


# Docker에 생성될 파일들

Docker의 파일 시스템은 ${Home}/notebook에 마운트 되어 있다.

그래서 Docker내에서 생성된 파일들은 ${Home}/notebooks 밑에서 볼 수 있다.

본 문서의 경우 ${Home} 은 dhrim00이다.

```
# Docker에 연결하고
dhrim00@tensorflow1-10-vm:/jet/prs$ docker exec -it db48a3202769 /bin/bash

# Docker 내에서 파일 만들기
root@db48a3202769:/notebooks# touch new_file.txt

root@db48a3202769:/notebooks# ls -al
total 8
drwxr-xr-x 2 root root 4096 Jun 21 06:22 .
drwxr-xr-x 1 root root 4096 Jun 21 06:21 ..
-rw-r--r-- 1 root root    0 Jun 21 06:22 new_file.txt

# 연결 끊고
root@db48a3202769:/notebooks# ls -al

# host에서 파일 보기
dhrim00@tensorflow1-10-vm:/jet/prs$ ls -al ${HOME}/notebooks
total 8
drwxr-xr-x 2 root    root    4096 Jun 21 06:22 .
drwxr-xr-x 5 dhrim00 dhrim00 4096 Jun 21 06:21 ..
-rw-r--r-- 1 root    root       0 Jun 21 06:22 new_file.txt
```


# Reference

- GCP

    - CE 이미지 고르기 : https://cloud.google.com/deep-learning-vm/docs/images

    - 딥러닝 VM 이미지 홈 : https://cloud.google.com/deep-learning-vm/

- Docker

    - tensorflow 이미지 리스트 : https://hub.docker.com/r/tensorflow/tensorflow/tags?page=14

    - Ubuntu에 Docker 설치 : https://www.bsidesoft.com/?p=7820

    - Debian에 Docker 설치 : https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-debian-9

    - docker로 환경 구축하기 : http://moducon.kr/2018/wp-content/uploads/sites/2/2018/12/leesangsoo_slide.pdf







