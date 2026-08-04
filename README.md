Codyssey 개발 워크스테이션 미션

## 0. 프로젝트 개요

본 프로젝트는 터미널, Docker, Git/GitHub를 활용하여 개발 워크스테이션을 구축하고, 누구나 동일한 절차로 실행과 검증이 가능한 재현 가능한 개발 환경을 구성하는 것을 목표로 합니다.

터미널을 이용한 파일 및 디렉터리 관리와 권한 설정, Docker를 활용한 이미지·컨테이너·포트·스토리지 관리, Git과 GitHub를 통한 버전 관리 및 협업 환경 구성을 직접 수행하였습니다. 또한 각 단계의 명령어와 실행 결과를 기록하고, 웹 접속, 로그 확인, 변경 사항 반영, 데이터 유지 여부 등을 기준으로 정상 동작을 검증하였습니다.

최종적으로 모든 수행 과정과 검증 결과, 트러블슈팅 내용을 README에 문서화하여 저장소만으로 전체 실습 흐름과 결과를 확인할 수 있도록 구성하였습니다. 이를 통해 개발 도구의 단순 사용법뿐만 아니라 이미지와 컨테이너의 분리, 포트 매핑, 데이터 영속성, 로컬과 원격 저장소의 역할 등 개발 환경을 구성하는 핵심 원리를 이해하는 것을 목표로 하였습니다.

---

## 1. 실행 환경

| 항목 | 값 |
| --- | --- |
| OS | 15.7.4(24G517) |
| Shell | ZSH |
| Docker | Docker version 29.4.0 |
| Git | git version 2.53.0 |
| 비고 | OrbStack  |

---

## 2. 수행 항목 체크리스트

- [ ]  터미널 기본 조작 (이동/생성/복사/이동·이름변경/삭제/내용확인/빈파일생성)
- [ ]  파일 권한 확인/변경 실습 (파일 1개, 디렉토리 1개)
- [ ]  Docker 설치 확인 (`docker --version`)
- [ ]  Docker 데몬 동작 확인 (`docker info`)
- [ ]  Docker 기본 운영 명령 (`images`, `ps`, `ps -a`, `logs`, `stats`)
- [ ]  `hello-world` 컨테이너 실행
- [ ]  `ubuntu` 컨테이너 실행 + 내부 명령 수행 (`ls`, `echo` 등)
- [ ]  컨테이너 종료/유지(attach vs exec) 차이 관찰 및 정리
- [ ]  웹 서버 코드 작성
- [ ]  Dockerfile 작성 (기존 베이스 이미지 기반 커스텀 이미지)
- [ ]  이미지 빌드 및 컨테이너 실행 성공
- [ ]  포트 매핑 후 접속 성공
- [ ]  바인드 마운트 반영 확인 (호스트 변경 전/후 비교)
- [ ]  Docker 볼륨 생성/연결/영속성 검증 (컨테이너 삭제 전/후 비교)
- [ ]  Git 사용자 정보 및 기본 브랜치 설정
- [ ]  GitHub 로그인 및 VSCode 연동

---

---

## 3. 터미널 기본 조작 로그

<!-- pwd, ls -la, mkdir, cp, mv, rm, cat, touch 등 -->

```bash
# PWD 경로확인
/codyssey/firstMission main !2 ❯ pwd
/Users/swon0115728502/codyssey/firstMission

# 현재 디렉토리 내부 확인 ls
~/codyssey/firstMission main !2 ❯ ls -al
total 24
drwxr-xr-x   5 swon0115728502  swon0115728502   160  7 31 15:09 .
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 30 16:42 ..
drwxr-xr-x  15 swon0115728502  swon0115728502   480  7 31 14:25 .git
-rw-r--r--   1 swon0115728502  swon0115728502  1066  7 30 20:44 LICENSE
-rw-r--r--   1 swon0115728502  swon0115728502  8025  7 31 14:03 README.md

# 빈 파일 생성 touch(echo 파일 생성)
~/codyssey/firstMission main !2 ❯ touch test.txt
~/codyssey/firstMission main !2 ❯ echo '' > test2.txt
~/codyssey/firstMission main !2 ?1 ❯ ls -al
total 32
drwxr-xr-x   7 swon0115728502  swon0115728502   224  7 31 15:11 .
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 30 16:42 ..
drwxr-xr-x  15 swon0115728502  swon0115728502   480  7 31 14:25 .git
-rw-r--r--   1 swon0115728502  swon0115728502  1066  7 30 20:44 LICENSE
-rw-r--r--   1 swon0115728502  swon0115728502  8025  7 31 14:03 README.md
-rw-r--r--   1 swon0115728502  swon0115728502     0  7 31 15:11 test.txt
-rw-r--r--   1 swon0115728502  swon0115728502     1  7 31 15:11 test2.txt

# 파일복사 cp
~/codyssey/firstMission main !2 ?1 ❯ cp test2.txt copy.txt
~/codyssey/firstMission main !2 ?2 ❯ ls
copy.txt  LICENSE   README.md test.txt  test2.txt

# 파일이동 및 이름 변경 mv
~/codyssey/firstMission main !2 ?2 ❯ mv copy.txt rename.txt
~/codyssey/firstMission main !2 ?2 ❯ ls
LICENSE    README.md  rename.txt test.txt   test2.txt

# 디렉토리 생성 mkdir
~/codyssey/firstMission main !2 ?2 ❯ mkdir permission
~/codyssey/firstMission main !2 ?2 ❯ ls
LICENSE    permission README.md  rename.txt test.txt   test2.txt
~/codyssey/firstMission main !2 ?2 ❯ ls -al
total 40
drwxr-xr-x   9 swon0115728502  swon0115728502   288  7 31 15:18 .
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 30 16:42 ..
drwxr-xr-x  15 swon0115728502  swon0115728502   480  7 31 14:25 .git
-rw-r--r--   1 swon0115728502  swon0115728502  1066  7 30 20:44 LICENSE
drwxr-xr-x   2 swon0115728502  swon0115728502    64  7 31 15:18 permission
-rw-r--r--   1 swon0115728502  swon0115728502  8025  7 31 14:03 README.md
-rw-r--r--   1 swon0115728502  swon0115728502     1  7 31 15:17 rename.txt
-rw-r--r--   1 swon0115728502  swon0115728502     0  7 31 15:11 test.txt
-rw-r--r--   1 swon0115728502  swon0115728502     1  7 31 15:11 test2.txt
~/codyssey/firstMission main !2 ?2 ❯ mv rename.txt permission
~/codyssey/firstMission main !2 ?2 ❯ cd permission
~/c/firstMission/permission main !2 ?2 ❯ ls -al
total 8
drwxr-xr-x  3 swon0115728502  swon0115728502   96  7 31 15:18 .
drwxr-xr-x  8 swon0115728502  swon0115728502  256  7 31 15:18 ..
-rw-r--r--  1 swon0115728502  swon0115728502    1  7 31 15:17 rename.txt

~/c/firstMission/permission main !2 ?2 ❯ cd ..
~/codyssey/firstMission main !2 ?2 ❯ ls
LICENSE    permission README.md  test.txt   test2.txt

# 파일 내용 확인 cat
~/codyssey/firstMission main !2 ?2 ❯ cat test2.txt

~/codyssey/firstMission main !2 ?2 ❯ echo "hello codyssey" >> test2.txt
~/codyssey/firstMission main !2 ?2 ❯ cat test2.txt

hello codyssey
~/codyssey/firstMission main !2 ?2 ❯ ls -al
total 32
drwxr-xr-x   8 swon0115728502  swon0115728502   256  7 31 15:18 .
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 30 16:42 ..
drwxr-xr-x  15 swon0115728502  swon0115728502   480  7 31 14:25 .git
-rw-r--r--   1 swon0115728502  swon0115728502  1066  7 30 20:44 LICENSE
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 31 15:18 permission
-rw-r--r--   1 swon0115728502  swon0115728502  8025  7 31 14:03 README.md
-rw-r--r--   1 swon0115728502  swon0115728502     0  7 31 15:11 test.txt
-rw-r--r--   1 swon0115728502  swon0115728502    16  7 31 15:22 test2.txt

# 파일삭제 rm
~/codyssey/firstMission main !2 ?2 ❯ rm test2.txt
~/codyssey/firstMission main !2 ?1 ❯ ls -al
total 24
drwxr-xr-x   7 swon0115728502  swon0115728502   224  7 31 15:23 .
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 30 16:42 ..
drwxr-xr-x  15 swon0115728502  swon0115728502   480  7 31 14:25 .git
-rw-r--r--   1 swon0115728502  swon0115728502  1066  7 30 20:44 LICENSE
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 31 15:18 permission
-rw-r--r--   1 swon0115728502  swon0115728502  8025  7 31 14:03 README.md
-rw-r--r--   1 swon0115728502  swon0115728502     0  7 31 15:11 test.txt
```

### 3-1. 파일/디렉토리 권한 실습

```bash
# 파일/디렉토리 권한부여 chmod

~/codyssey/firstMission main !2 ?1 ❯ ls -al
total 24
drwxr-xr-x   7 swon0115728502  swon0115728502   224  7 31 15:23 .
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 30 16:42 ..
drwxr-xr-x  15 swon0115728502  swon0115728502   480  7 31 14:25 .git
-rw-r--r--   1 swon0115728502  swon0115728502  1066  7 30 20:44 LICENSE
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 31 15:18 permission
-rw-r--r--   1 swon0115728502  swon0115728502  8025  7 31 14:03 README.md
-rw-r--r--   1 swon0115728502  swon0115728502     0  7 31 15:11 test.txt

~/codyssey/firstMission main !2 ?1 ❯ chmod 744 test.txt
~/codyssey/firstMission main !2 ?1 ❯ ls -al
total 24
drwxr-xr-x   7 swon0115728502  swon0115728502   224  7 31 15:23 .
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 30 16:42 ..
drwxr-xr-x  15 swon0115728502  swon0115728502   480  7 31 14:25 .git
-rw-r--r--   1 swon0115728502  swon0115728502  1066  7 30 20:44 LICENSE
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 31 15:18 permission
-rw-r--r--   1 swon0115728502  swon0115728502  8025  7 31 14:03 README.md
-rwxr--r--   1 swon0115728502  swon0115728502     0  7 31 15:11 test.txt

~/codyssey/firstMission main !2 ?1 ❯ echo '#!/bin/zsh' > permission.txt

~/codyssey/firstMission main !2 ?1 ❯ ls -al
total 32
drwxr-xr-x   8 swon0115728502  swon0115728502   256  7 31 15:35 .
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 30 16:42 ..
drwxr-xr-x  15 swon0115728502  swon0115728502   480  7 31 14:25 .git
-rw-r--r--   1 swon0115728502  swon0115728502  1066  7 30 20:44 LICENSE
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 31 15:18 permission
-rw-r--r--   1 swon0115728502  swon0115728502    11  7 31 15:35 permission.txt
-rw-r--r--   1 swon0115728502  swon0115728502  8025  7 31 14:03 README.md
-rwxr--r--   1 swon0115728502  swon0115728502     0  7 31 15:11 test.txt
~/codyssey/firstMission main !2 ?1 ❯ echo 'echo "Permission Success"' >> permission.txt
~/codyssey/firstMission main !1 ?1 ❯ cat permission.txt
#!/bin/zsh
echo "Permission Success"

~/codyssey/firstMission main !1 ?1 ❯ chmod 744 permission.txt
~/codyssey/firstMission main !2 ?1 ❯ ./permission.txt
Permission Success
~/codyssey/firstMission main !2 ?1 ❯ ls -al
total 32
drwxr-xr-x   8 swon0115728502  swon0115728502   256  7 31 15:35 .
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 30 16:42 ..
drwxr-xr-x  15 swon0115728502  swon0115728502   480  7 31 14:25 .git
-rw-r--r--   1 swon0115728502  swon0115728502  1066  7 30 20:44 LICENSE
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 31 15:18 permission
-rwxr--r--   1 swon0115728502  swon0115728502    37  7 31 15:36 permission.txt
-rw-r--r--   1 swon0115728502  swon0115728502  8025  7 31 14:03 README.md
-rwxr--r--   1 swon0115728502  swon0115728502     0  7 31 15:11 test.txt

~/codyssey/firstMission main !2 ?1 ❯ chmod 644 permission
~/codyssey/firstMission main !2 ?1 ❯ ls -al
total 32
drwxr-xr-x   8 swon0115728502  swon0115728502   256  7 31 15:35 .
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 30 16:42 ..
drwxr-xr-x  15 swon0115728502  swon0115728502   480  7 31 14:25 .git
-rw-r--r--   1 swon0115728502  swon0115728502  1066  7 30 20:44 LICENSE
drw-r--r--   3 swon0115728502  swon0115728502    96  7 31 15:18 permission
-rwxr--r--   1 swon0115728502  swon0115728502    37  7 31 15:36 permission.txt
-rw-r--r--   1 swon0115728502  swon0115728502  8025  7 31 14:03 README.md
-rwxr--r--   1 swon0115728502  swon0115728502     0  7 31 15:11 test.txt
```

---

## 4. Docker 설치 및 기본 점검

```bash
# docker 버전확인 및 정보 확인
~/codyssey/firstMission main !2 ?1 ❯ docker --version
Docker version 28.5.2, build ecc6942
~/codyssey/firstMission main !2 ?1 ❯ docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/swon0115728502/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/swon0115728502/.docker/cli-plugins/docker-compose

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 28.5.2 // 도커 데몬 버전
 Storage Driver: overlay2
  Backing Filesystem: btrfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 1c4457e00facac03ce1d75f7b6777a7a851e5c41
 runc version: d842d7719497cc3b774fd71620278ac9e17710e0
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.17.8-orbstack-00308-g8f9c941121b1
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 15.67GiB
 Name: orbstack
 ID: c95326e8-7df8-4af3-9d33-4d1471403d88
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
 Default Address Pools:
   Base: 192.168.97.0/24, Size: 24
   Base: 192.168.107.0/24, Size: 24
   Base: 192.168.117.0/24, Size: 24
   Base: 192.168.147.0/24, Size: 24
   Base: 192.168.148.0/24, Size: 24
   Base: 192.168.155.0/24, Size: 24
   Base: 192.168.156.0/24, Size: 24
   Base: 192.168.158.0/24, Size: 24
   Base: 192.168.163.0/24, Size: 24
   Base: 192.168.164.0/24, Size: 24
   Base: 192.168.165.0/24, Size: 24
   Base: 192.168.166.0/24, Size: 24
   Base: 192.168.167.0/24, Size: 24
   Base: 192.168.171.0/24, Size: 24
   Base: 192.168.172.0/24, Size: 24
   Base: 192.168.181.0/24, Size: 24
   Base: 192.168.183.0/24, Size: 24
   Base: 192.168.186.0/24, Size: 24
   Base: 192.168.207.0/24, Size: 24
   Base: 192.168.214.0/24, Size: 24
   Base: 192.168.215.0/24, Size: 24
   Base: 192.168.216.0/24, Size: 24
   Base: 192.168.223.0/24, Size: 24
   Base: 192.168.227.0/24, Size: 24
   Base: 192.168.228.0/24, Size: 24
   Base: 192.168.229.0/24, Size: 24
   Base: 192.168.237.0/24, Size: 24
   Base: 192.168.239.0/24, Size: 24
   Base: 192.168.242.0/24, Size: 24
   Base: 192.168.247.0/24, Size: 24
   Base: fd07:b51a:cc66:d000::/56, Size: 64

WARNING: DOCKER_INSECURE_NO_IPTABLES_RAW is set
```

---

## 5. Docker 기본 운영 명령

```bash

~/codyssey/firstMission main !2 ?1 ❯ docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
ubuntu        latest    de7345b16e94   2 weeks ago    100MB
hello-world   latest    e2ac70e7319a   4 months ago   10.1kB

~/codyssey/firstMission main !2 ?1 ❯ docker rmi hello-world
Untagged: hello-world:latest
Untagged: hello-world@sha256:c3cbe1cc1aa588a64951ac6286e0df7b27fe2e6324b1001c619bb358770c0178
Deleted: sha256:e2ac70e7319a02c5a477f5825259bd118b94e8b02c279c67afa63adab6d8685b
Deleted: sha256:897b3f2a7c1bc2f3d02432f7892fe31c6272c521ad4d70257df624504a3238b4

~/codyssey/firstMission main !2 ?1 ❯ docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    de7345b16e94   2 weeks ago   100MB

~/codyssey/firstMission main !2 ?1 ❯ docker rmi ubuntu
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:3131b4cc82a783df6c9df078f86e01819a13594b865c2cad47bd1bca2b7063bb
Deleted: sha256:de7345b16e942e22044c6ba053020ec85ae879984860a9918517d54eb6cef851
Deleted: sha256:9384d7fbfc7805e1cf888304178b7ecf01f8f1e766e1798cac368238c41b3df1
Deleted: sha256:d6b1a90bccf18353db11e206211d1438050e64d14c56a48ae067131e0a3cc245

~/codyssey/firstMission main !2 ?1 ❯ docker pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete
Digest: sha256:c3cbe1cc1aa588a64951ac6286e0df7b27fe2e6324b1001c619bb358770c0178
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
~/c/firstMission main !2 ?1 ❯ docker images                                  4s
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    e2ac70e7319a   4 months ago   10.1kB
~/codyssey/firstMission main !2 ?1 ❯ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
~/codyssey/firstMission main !2 ?1 ❯ docker run -dit --name ubu1 unbuntu
Unable to find image 'unbuntu:latest' locally
docker: Error response from daemon: pull access denied for unbuntu, repository does not exist or may require 'docker login': denied: requested access to the resource is denied

Run 'docker run --help' for more information
~/codyssey/firstMission main !2 ?1 ❯ docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    e2ac70e7319a   4 months ago   10.1kB
~/codyssey/firstMission main !2 ?1 ❯ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
~/codyssey/firstMission main !2 ?1 ❯ docker run -dit --name ubu1 ubuntu
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
ed819469700f: Pull complete
a3679419df18: Pull complete
Digest: sha256:3131b4cc82a783df6c9df078f86e01819a13594b865c2cad47bd1bca2b7063bb
Status: Downloaded newer image for ubuntu:latest
77f90f3fd8ae7b6972ad159395cdeab54611aa25b6a2dc27e7b46e84a6804a7a
~/c/firstMission main !2 ?1 ❯ docker ps -a                                   8s
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
77f90f3fd8ae   ubuntu    "/bin/bash"   29 seconds ago   Up 28 seconds             ubu1
~/codyssey/firstMission main !2 ?1 ❯ docker run --name hi hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

~/codyssey/firstMission main !2 ?1 ❯ docker ps -a
CONTAINER ID   IMAGE         COMMAND       CREATED              STATUS                      PORTS     NAMES
c92b3609413c   hello-world   "/hello"      31 seconds ago       Exited (0) 30 seconds ago             hi
77f90f3fd8ae   ubuntu        "/bin/bash"   About a minute ago   Up About a minute
```

---

## 6. 컨테이너 실행 실습

### 6-1. hello-world

```bash
$ docker run hello-world
TODO
```

### 6-2. ubuntu 컨테이너 진입 및 명령 수행

```bash
TODO: docker run -it ubuntu 등으로 진입 후 ls, echo 결과
```

### 6-3. attach vs exec 관찰 정리

> TODO: 두 방식의 차이를 직접 관찰한 대로 2~3줄로 정리
(예: exec는 메인 프로세스 안 건드리고 새 프로세스 추가 실행이라 종료해도 컨테이너 유지,
attach는 메인 프로세스에 직접 연결이라 종료 시 컨테이너도 함께 종료)
> 

```bash
~/codyssey/firstMission main ❯ docker ps -a
CONTAINER ID   IMAGE         COMMAND       CREATED        STATUS                      PORTS     NAMES
c92b3609413c   hello-world   "/hello"      25 hours ago   Exited (0) 25 hours ago               hi
77f90f3fd8ae   ubuntu        "/bin/bash"   25 hours ago   Exited (137) 24 hours ago             ubu1
~/codyssey/firstMission main ❯ docker start ubu1
ubu1
~/codyssey/firstMission main ❯ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED        STATUS         PORTS     NAMES
77f90f3fd8ae   ubuntu    "/bin/bash"   25 hours ago   Up 5 seconds             ubu1
~/codyssey/firstMission main ❯ docker attach ubu1
root@77f90f3fd8ae:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@77f90f3fd8ae:/# ls -al
total 16
drwxr-xr-x   1 root root   6 Jul 31 07:01 .
drwxr-xr-x   1 root root   6 Jul 31 07:01 ..
-rwxr-xr-x   1 root root   0 Jul 31 07:01 .dockerenv
drwxr-xr-x   1 root root  26 Jul 13 16:06 .rock
lrwxrwxrwx   1 root root   7 Apr 20 08:46 bin -> usr/bin
drwxr-xr-x   1 root root   0 Apr 20 08:46 boot
drwxr-xr-x   5 root root 340 Aug  1 07:37 dev
drwxr-xr-x   1 root root  56 Jul 31 07:01 etc
drwxr-xr-x   1 root root  12 Jul 13 16:06 home
lrwxrwxrwx   1 root root   7 Apr 20 08:46 lib -> usr/lib
lrwxrwxrwx   1 root root   9 Apr 20 08:46 lib64 -> usr/lib64
drwxr-xr-x   1 root root   0 Jul 13 16:05 media
drwxr-xr-x   1 root root   0 Jul 13 16:05 mnt
drwxr-xr-x   1 root root   0 Jul 13 16:05 opt
dr-xr-xr-x 224 root root   0 Aug  1 07:37 proc
drwx------   1 root root  30 Jul 13 16:06 root
drwxr-xr-x   1 root root  22 Jul 13 16:06 run
lrwxrwxrwx   1 root root   8 Apr 20 08:46 sbin -> usr/sbin
drwxr-xr-x   1 root root   0 Jul 13 16:05 srv
dr-xr-xr-x  11 root root   0 Aug  1 07:37 sys
drwxrwxrwt   1 root root   0 Jul 13 16:06 tmp
drwxr-xr-x   1 root root  10 Jul 13 16:05 usr
drwxr-xr-x   1 root root  90 Jul 13 16:06 var

root@77f90f3fd8ae:/# exit
exit
~/codyssey/firstMission main ❯ docker ps -a                                                     1m 19s
CONTAINER ID   IMAGE         COMMAND       CREATED        STATUS                     PORTS     NAMES
c92b3609413c   hello-world   "/hello"      25 hours ago   Exited (0) 25 hours ago              hi
77f90f3fd8ae   ubuntu        "/bin/bash"   25 hours ago   Exited (0) 7 seconds ago             ubu1

~/codyssey/firstMission main ❯ docker start ubu1
ubu1
~/codyssey/firstMission main ❯ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED        STATUS         PORTS     NAMES
77f90f3fd8ae   ubuntu    "/bin/bash"   25 hours ago   Up 8 seconds             ubu1
~/codyssey/firstMission main ❯ docker exec -it ubu1 bash
root@77f90f3fd8ae:/# ls -al
total 16
drwxr-xr-x   1 root root  14 Jul 31 07:01 .
drwxr-xr-x   1 root root  14 Jul 31 07:01 ..
-rwxr-xr-x   1 root root   0 Jul 31 07:01 .dockerenv
drwxr-xr-x   1 root root  26 Jul 13 16:06 .rock
lrwxrwxrwx   1 root root   7 Apr 20 08:46 bin -> usr/bin
drwxr-xr-x   1 root root   0 Apr 20 08:46 boot
drwxr-xr-x   5 root root 340 Aug  1 07:40 dev
drwxr-xr-x   1 root root  56 Jul 31 07:01 etc
drwxr-xr-x   1 root root  12 Jul 13 16:06 home
lrwxrwxrwx   1 root root   7 Apr 20 08:46 lib -> usr/lib
lrwxrwxrwx   1 root root   9 Apr 20 08:46 lib64 -> usr/lib64
drwxr-xr-x   1 root root   0 Jul 13 16:05 media
drwxr-xr-x   1 root root   0 Jul 13 16:05 mnt
drwxr-xr-x   1 root root   0 Jul 13 16:05 opt
dr-xr-xr-x 229 root root   0 Aug  1 07:40 proc
drwx------   1 root root  26 Aug  1 07:39 root
drwxr-xr-x   1 root root  22 Jul 13 16:06 run
lrwxrwxrwx   1 root root   8 Apr 20 08:46 sbin -> usr/sbin
drwxr-xr-x   1 root root   0 Jul 13 16:05 srv
dr-xr-xr-x  11 root root   0 Aug  1 07:38 sys
drwxrwxrwt   1 root root   0 Jul 13 16:06 tmp
drwxr-xr-x   1 root root  10 Jul 13 16:05 usr
drwxr-xr-x   1 root root  90 Jul 13 16:06 var
root@77f90f3fd8ae:/# exit
exit
~/codyssey/firstMission main ❯ docker ps                                                           19s
CONTAINER ID   IMAGE     COMMAND       CREATED        STATUS              PORTS     NAMES
77f90f3fd8ae   ubuntu    "/bin/bash"   25 hours ago   Up About a minute             ubu1
```

---

## 7. Dockerfile 기반 커스텀 이미지

```bash
~/codyssey/firstMission main ❯ mkdir docker_workstation/site
mkdir: docker_workstation: No such file or directory
~/codyssey/firstMission main ❯ mkdir -p docker_workstation/site
~/codyssey/firstMission main ❯ mkdir -p docker_workstation/bind_site
~/codyssey/firstMission main ❯ cd docker_workstation
~/codyssey/firstMission/docker_workstation main ❯ ls
bind_site site
~/codyssey/firstMission/docker_workstation main ❯ pwd
/Users/swon0115728502/codyssey/firstMission/docker_workstation

# 파일구조 확인
~/codyssey/firstMission/docker_workstation main ❯ find . -maxdepth 2 -type f -o -type d
.
./site
./bind_site

# 파일 커스텀 html작성
~/codyssey/firstMission/docker_workstation main ❯ cat > site/index.html <<'EOF'
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Codyssey Docker Practice</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 720px;
      margin: 80px auto;
      padding: 20px;
      background: #f5f5f5;
    }

    main {
      padding: 32px;
      background: white;
      border-radius: 16px;
    }

    h1 {
      color: #1769aa;
    }
  </style>
</head>
<body>
  <main>
    <h1>Docker Custom Image</h1>
    <p>Nginx 기반 커스텀 이미지 실행에 성공했습니다.</p>
    <p>Version: 1.0</p>
  </main>
</body>
</html>
EOF
~/c/firstMission/docker_workstation main ?1 ❯ cat site/index.html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Codyssey Docker Practice</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 720px;
      margin: 80px auto;
      padding: 20px;
      background: #f5f5f5;
    }

    main {
      padding: 32px;
      background: white;
      border-radius: 16px;
    }

    h1 {
      color: #1769aa;
    }
  </style>
</head>
<body>
  <main>
    <h1>Docker Custom Image</h1>
    <p>Nginx 기반 커스텀 이미지 실행에 성공했습니다.</p>
    <p>Version: 1.0</p>
  </main>
</body>
</html>

~/c/firstMission/docker_workstation main ?1 ❯ pwd
/Users/swon0115728502/codyssey/firstMission/docker_workstation

~/c/firstMission/docker_workstation main ?1 ❯ cat > Dockerfile <<'EOF'
FROM nginx:alpine

LABEL org.opencontainers.image.title="codyssey-custom-nginx"
LABEL org.opencontainers.image.description="Codyssey Docker workstation practice"

COPY site/index.html /usr/share/nginx/html/index.html

EXPOSE 80
EOF

~/c/firstMission/docker_workstation main ?1 ❯ cat Dockerfile
FROM nginx:alpine

LABEL org.opencontainers.image.title="codyssey-custom-nginx"
LABEL org.opencontainers.image.description="Codyssey Docker workstation practice"

COPY site/index.html /usr/share/nginx/html/index.html

EXPOSE 80

~/c/firstMission/docker_workstation main ?1 ❯ ls -al
total 8
drwxr-xr-x  5 swon0115728502  swon0115728502  160  8  1 17:10 .
drwxr-xr-x  9 swon0115728502  swon0115728502  288  8  1 17:02 ..
drwxr-xr-x  2 swon0115728502  swon0115728502   64  8  1 17:03 bind_site
-rw-r--r--  1 swon0115728502  swon0115728502  228  8  1 17:10 Dockerfile
drwxr-xr-x  3 swon0115728502  swon0115728502   96  8  1 17:09 site

# 이미지 빌드

~/c/firstMission/docker_workstation main ?1 ❯ docker build -t dodyssey_web:1.0 .
[+] Building 6.4s (7/7) FINISHED                                                       docker:orbstack
 => [internal] load build definition from Dockerfile                                              0.2s
 => => transferring dockerfile: 267B                                                              0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                   2.3s
 => [internal] load .dockerignore                                                                 0.2s
 => => transferring context: 2B                                                                   0.0s
 => [internal] load build context                                                                 0.2s
 => => transferring context: 728B                                                                 0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3  2.9s
 => => resolve docker.io/library/nginx:alpine@sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3  0.2s
 => => sha256:3cd534fe98c64d68a1f4f1c83abb8d5cba7ecfd7be88e592389929d12e6253da 1.89MB / 1.89MB    0.3s
 => => sha256:4a73073bd557c65b759505da037898b61f1be6cbcc3c2c3aeac22d2a470c1752 10.33kB / 10.33kB  0.0s
 => => sha256:1d40e3eb3bf4f138de1d67193f2aa5309fcaf343eb5ffadbf5e9439de1eb1ebb 2.50kB / 2.50kB    0.0s
 => => sha256:f0ba77f796e57c6fa89ae7f4fdad1665d6fcbd8e3f211535120542b337f9959e 12.32kB / 12.32kB  0.0s
 => => sha256:1223f016b4e4a2c21f7c49d4837fbfd47a9da6436b511690ca1e582fc2810d59 627B / 627B        0.6s
 => => sha256:55afa1ecc21d2bb5e5045f32dafee56272ffd89860bac26f6c32123439af26a4 3.85MB / 3.85MB    0.5s
 => => sha256:62bec68d7c31c4c8a19d812d84da5f7748e54690c037979945b6c5b6c924b142 957B / 957B        0.7s
 => => extracting sha256:55afa1ecc21d2bb5e5045f32dafee56272ffd89860bac26f6c32123439af26a4         0.1s
 => => sha256:46f977ee452f4399c208714afa034868d6056864f8a0cf3c643ab143dd802c80 404B / 404B        0.8s
 => => extracting sha256:3cd534fe98c64d68a1f4f1c83abb8d5cba7ecfd7be88e592389929d12e6253da         0.1s
 => => sha256:d0008c891db48b5f526d914bce9e8d889fe1a9d1f08291ae03fe97f871726f38 1.21kB / 1.21kB    0.9s
 => => sha256:46519e7231d2eb5604df229beb44d59719a489eaa7aca52982535a010b07a9ed 20.31MB / 20.31MB  1.2s
 => => sha256:390dc935348d8070e695fbaae2a4bb114fb9e69c59f628e7576036ee9d5244c9 1.40kB / 1.40kB    1.0s
 => => extracting sha256:1223f016b4e4a2c21f7c49d4837fbfd47a9da6436b511690ca1e582fc2810d59         0.0s
 => => extracting sha256:62bec68d7c31c4c8a19d812d84da5f7748e54690c037979945b6c5b6c924b142         0.0s
 => => extracting sha256:46f977ee452f4399c208714afa034868d6056864f8a0cf3c643ab143dd802c80         0.0s
 => => extracting sha256:d0008c891db48b5f526d914bce9e8d889fe1a9d1f08291ae03fe97f871726f38         0.0s
 => => extracting sha256:390dc935348d8070e695fbaae2a4bb114fb9e69c59f628e7576036ee9d5244c9         0.0s
 => => extracting sha256:46519e7231d2eb5604df229beb44d59719a489eaa7aca52982535a010b07a9ed         0.4s
 => [2/2] COPY site/index.html /usr/share/nginx/html/index.html                                   0.2s
 => exporting to image                                                                            0.2s
 => => exporting layers                                                                           0.1s
 => => writing image sha256:6873943b2f57ebe2a21f54a5dce515d6918b4d0ec86264d9655fbe32cc6ea429      0.0s
 => => naming to docker.io/library/dodyssey_web:1.0                                               0.0s
 
# 도커 이미지 확인
~/c/firstMission/docker_workstation main ?1 ❯ docker images                                         7s
REPOSITORY     TAG       IMAGE ID       CREATED          SIZE
dodyssey_web   1.0       6873943b2f57   38 seconds ago   62.4MB
ubuntu         latest    de7345b16e94   2 weeks ago      100MB
hello-world    latest    e2ac70e7319a   4 months ago     10.1kB

~/c/firstMission/docker_workstation main ?1 ❯ docker images dodyssey_web
REPOSITORY     TAG       IMAGE ID       CREATED              SIZE
dodyssey_web   1.0       6873943b2f57   About a minute ago   62.4MB

# 포트매핑 하며 컨테이너 생성
~/c/firstMission/docker_workstation main ?1 ❯ docker run -d --name codyssey_web_8080 -p 8080:80 dodysse
y_web:1.0
17c5d0a240bd04fc043f2908fba55f503f48499f158bace122a99e1ca529264f

~/c/firstMission/docker_workstation main ?1 ❯ docker ps
CONTAINER ID   IMAGE              COMMAND                   CREATED         STATUS             PORTS                                     NAMES
17c5d0a240bd   dodyssey_web:1.0   "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes       0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   codyssey_web_8080
77f90f3fd8ae   ubuntu             "/bin/bash"               26 hours ago    Up About an hour                                             ubu1

# 생성된 컨테이너 파일 확인
~/c/firstMission/docker_workstation main ?1 ❯ docker exec codyssey_web_8080 \
  cat /usr/share/nginx/html/index.html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Codyssey Docker Practice</title>
  <style>
    body {
      font-family: sans-serif;
      max-width: 720px;
      margin: 80px auto;
      padding: 20px;
      background: #f5f5f5;
    }

    main {
      padding: 32px;
      background: white;
      border-radius: 16px;
    }

    h1 {
      color: #1769aa;
    }
  </style>
</head>
<body>
  <main>
    <h1>Docker Custom Image</h1>
    <p>Nginx 기반 커스텀 이미지 실행에 성공했습니다.</p>
    <p>Version: 1.0</p>
  </main>
</body>
</html>
```

---

---

## 8. 바인드 마운트 검증

```bash

# 바인드 마운트용 웹 페이지 

docker_workstation git:(main) ✗ cat bind_site/index.html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Bind Mount Practice</title>
</head>
<body>
  <h1>Bind Mount Version 1</h1>
  <p>이 파일은 이미지가 아니라 호스트 디렉토리에서 제공됩니다.</p>
</body>
</html>

# 다른 포트번호 부여 후 컨테이너 생성
docker_workstation git:(main) ✗ docker run -d \
  --name codyssey-bind \
  -p 8082:80 \
  -v "$PWD/bind_site:/usr/share/nginx/html:ro" \
  codyssey_web:1.0
6e9cc19de502238d005fcbd9e13c91268d7d8d816e8d2a47cb702529a6930c9d

➜  docker_workstation git:(main) ✗ docker ps
CONTAINER ID   IMAGE              COMMAND                   CREATED         STATUS         PORTS                                     NAMES
6e9cc19de502   codyssey_web:1.0   "/docker-entrypoint.…"   5 seconds ago   Up 4 seconds   0.0.0.0:8082->80/tcp, [::]:8082->80/tcp   codyssey-bind
89b00d469b10   codyssey_web:1.0   "/docker-entrypoint.…"   2 hours ago     Up 2 hours     0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   codyssey_web_8080

docker_workstation git:(main) ✗ cat > bind_site/index.html <<'EOF'
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Bind Mount Practice</title>
</head>
<body>
  <h1>Bind Mount Version 2</h1>
  <p>호스트 파일을 변경하자 실행 중인 컨테이너에 즉시 반영되었습니다.</p>
</body>
</html>
EOF

# 브라우저 확인
http://localhost:8082
# 터미널 확인
curl http://localhost:8082
```

---

## 9. Docker 볼륨 영속성 검증

```bash
# 도커 볼륨 생성
docker_workstation git:(main) ✗ docker volume create codyssey-data
codyssey-data

➜  docker_workstation git:(main) ✗ docker volume ls
DRIVER    VOLUME NAME
local     codyssey-data
local     mydata
local     n8n_data

docker_workstation git:(main) ✗ docker run -d \
  --name volume-test-1 \
  -v codyssey-data:/data \
  ubuntu \
  sleep infinity
1a5ffd2b54c1076dd076dde7c6b987ec897aa9f2899a43b5a0dd8845cab30b3f

➜  docker_workstation git:(main) ✗ docker ps
CONTAINER ID   IMAGE              COMMAND                   CREATED          STATUS          PORTS                                     NAMES
1a5ffd2b54c1   ubuntu             "sleep infinity"          13 seconds ago   Up 12 seconds                                             volume-test-1
6e9cc19de502   codyssey_web:1.0   "/docker-entrypoint.…"   35 minutes ago   Up 35 minutes   0.0.0.0:8082->80/tcp, [::]:8082->80/tcp   codyssey-bind
89b00d469b10   codyssey_web:1.0   "/docker-entrypoint.…"   3 hours ago      Up 3 hours      0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   codyssey_web_8080

# 컨테이너 실행 후 내용 변경
➜  docker_workstation git:(main) ✗ docker exec volume-test-1 \
  sh -c 'echo "Docker volume persistent data" > /data/result.txt'
  
  
➜  docker_workstation git:(main) ✗ docker exec volume-test-1 \
  cat /data/result.txt
Docker volume persistent data

# 컨테이너 실행 후 내용 넣은 파일 확인 
➜  docker_workstation git:(main) ✗ docker exec volume-test-1 \
  ls -la /data
total 12
drwxr-xr-x 2 root root 4096 Aug  3 08:56 .
drwxr-xr-x 1 root root 4096 Aug  3 08:56 ..
-rw-r--r-- 1 root root   30 Aug  3 08:56 result.txt

#컨테이너 삭제
➜  docker_workstation git:(main) ✗ docker rm -f volume-test-1
volume-test-1

➜  docker_workstation git:(main) ✗ docker ps -a
CONTAINER ID   IMAGE              COMMAND                   CREATED          STATUS          PORTS                                     NAMES
6e9cc19de502   codyssey_web:1.0   "/docker-entrypoint.…"   38 minutes ago   Up 38 minutes   0.0.0.0:8082->80/tcp, [::]:8082->80/tcp   codyssey-bind
89b00d469b10   codyssey_web:1.0   "/docker-entrypoint.…"   3 hours ago      Up 3 hours      0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   codyssey_web_8080

➜  docker_workstation git:(main) ✗ docker volume ls
DRIVER    VOLUME NAME
local     codyssey-data
local     mydata
local     n8n_data

# 해당 볼륨으로 새 컨테이너 생성
➜  docker_workstation git:(main) ✗ docker run -d \
  --name volume-test-2 \
  -v codyssey-data:/data \
  ubuntu \
  sleep infinity
1a1607e7a2f1cf2a84f1809d968c86f4d9bb4501794b38dcda08a387d75a4e27

# 전 컨테이너에서 만든 파일 확인
➜  docker_workstation git:(main) ✗ docker exec volume-test-2 \
  cat /data/result.txt
Docker volume persistent data

# 생성한 볼륨 json으로 정보 확인
➜  docker_workstation git:(main) ✗ docker inspect volume-test-2 \
  --format='{{json .Mounts}}'
[{"Type":"volume","Name":"codyssey-data","Source":"/var/lib/docker/volumes/codyssey-data/_data","Destination":"/data","Driver":"local","Mode":"z","RW":true,"Propagation":""}]
```

---

## 10. Git 설정 및 GitHub 연동

```bash
$ git config --list
core.repositoryformatversion=0   # 저장소 포맷 버전 (기본값)
core.filemode=true               # 파일 실행권한 변경 추적
core.bare=false                  # 일반 작업 저장소 (bare 아님)
core.logallrefupdates=true       # ref 변경 이력 기록 (reflog)
core.ignorecase=true             # 대소문자 구분 안 함 (macOS 특징)
core.precomposeunicode=true      # 한글 파일명 유니코드 정규화 (macOS 필수!)

remote.origin.url=https://github.com/k-sungwon/codyssey_misson1.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
→ origin이라는 이름으로 GitHub 저장소와 연결되어 있습니다.
→ 두 번째 줄은 "원격의 모든 브랜치를 가져온다"는 규칙입니다.

branch.main.remote=origin              # main은 origin과 연결
branch.main.merge=refs/heads/main      # origin/main과 병합
branch.main.vscode-merge-base=origin/main  # VSCode가 추가한 설정
→ 로컬 main 브랜치가 **원격 origin/main을 추적(track)**하고 있습니다.
→ 추적하고 있으므로 git pull, git push를 브랜치 이름 없이 사용할 수 있습니다.
---

## 11. 트러블슈팅

### 11-1.  로컬, 원격 브랜치 불일치로 push 실패

#### 문제

로컬 저장소의 기본 브랜치는 `master`였지만, GitHub 원격 저장소의 기본 브랜치는 `main`으로 생성되어 있었습니다.

로컬 `master`를 원격에 push하면서 GitHub에 `main`, `master` 브랜치가 동시에 존재하게 되었고, 로컬 브랜치 이름을 `main`으로 변경한 뒤 push하자 `non-fast-forward` 오류가 발생했습니다.

```bash
$ git push -u origin main
To https://github.com/k-sungwon/codyssey_misson1.git
 ! [rejected]        main -> main (non-fast-forward)

error: failed to push some refs
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart.
```

#### 확인

원격 정보를 가져온 뒤 모든 브랜치의 커밋 관계를 확인 했습니다.

```bash
$ git fetch origin

$ git log --oneline --graph --decorate --all
* c2e736f (HEAD -> main, origin/master) practice shell command
* 1a88688 (origin/main, origin/HEAD) Initial commit
```

출력 결과를 통해 다음 상태를 확인했습니다.

- 로컬 `main`과 원격 `master`는 `practice shell command` 커밋을 가리킴
- 원격 `main`은 별도로 생성된 `Initial commit`을 가리킴
- `origin/HEAD`가 `origin/main`을 가리키므로 원격 기본 브랜치는 `main`
- 로컬 `main`과 원격 `main`의 커밋 이력이 서로 합쳐지지 않은 상태

#### **해결/대안**

먼저 원격 `main`의 커밋을 현재 로컬 `main`에 병합했습니다.

두 저장소가 서로 별도로 초기화되어 공통 시작 커밋이 없었으므로 `--allow-unrelated-histories` 옵션을 사용했습니다.

```
$ git merge origin/main--allow-unrelated-histories
```

병합 과정에서 Git이 병합 커밋 메시지를 입력받기 위해 Vim을 실행했습니다. 기본 메시지를 저장하고 종료해 병합 커밋을 생성했습니다.

```
Merge remote-tracking branch 'origin/main'
```

병합 결과를 확인했습니다.

```
$ git log--oneline--graph--decorate--all
*   b738e75 (HEAD-> main) Merge remote-tracking branch'origin/main'
|\
| * 1a88688 (origin/main, origin/HEAD) Initial commit
* c2e736f (origin/master) practice shell command
```

로컬 실습 커밋과 원격 `Initial commit`이 하나의 `main` 이력으로 병합된 것을 확인한 뒤 원격 `main`에 push했습니다.

```
$ git push-u origin main
```

마지막으로 원격 `main`에 파일과 커밋이 정상적으로 반영된 것을 확인한 후, 중복된 원격 `master` 브랜치를 삭제했습니다.

```
$ git push origin--delete master$ git fetch--prune$ git branch-a
```

최종적으로 로컬과 원격 저장소의 기본 브랜치를 모두 `main`으로 통일했습니다.

```
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
```

### 11-2  GIT add 실패

#### 문제

git add 시 권한문제로 접근 거부

#### 확인

에러메세지를 통한 문제 확인

#### 해결방법

실행권한 부여하여 해결

```bash
## 디렉토리 실행권한 없으면 git add 안됨

~/codyssey/firstMission main !2 ?1 ❯ git add .                                          ✘ INT 23s
warning: 'permission/.gitignore'에 접근할 수 없습니다: Permission denied
fatal: unable to stat 'permission/rename.txt': Permission denied
~/codyssey/firstMission main !2 ?1 ❯ ls
LICENSE        permission     permission.txt README.md      test.txt
~/codyssey/firstMission main !2 ?1 ❯ ls -al
total 32
drwxr-xr-x   8 swon0115728502  swon0115728502   256  7 31 15:35 .
drwxr-xr-x   3 swon0115728502  swon0115728502    96  7 30 16:42 ..
drwxr-xr-x  15 swon0115728502  swon0115728502   480  7 31 16:06 .git
-rw-r--r--   1 swon0115728502  swon0115728502  1066  7 30 20:44 LICENSE
drw-r--r--   3 swon0115728502  swon0115728502    96  7 31 15:18 permission
-rwxr--r--   1 swon0115728502  swon0115728502    37  7 31 15:36 permission.txt
-rw-r--r--   1 swon0115728502  swon0115728502  8025  7 31 14:03 README.md
-rwxr--r--   1 swon0115728502  swon0115728502     0  7 31 15:11 test.txt
~/codyssey/firstMission main !2 ?1 ❯ chmod 744 permission
~/codyssey/firstMission main !2 ?1 ❯ git add .
```

### 11-3.  Docker 컨테이너 실행 후 즉시 종료되는 문제

#### **문제**:

Ubuntu 컨테이너를 실행했지만 `docker ps`에서 실행 중인 컨테이너가 보이지 않고 바로 종료되었습니다.

#### **원인 가설**:

 컨테이너 실행이 실패했거나 Docker에 문제가 있다고 판단하였습니다.

#### **확인**:

```
docker ps -a
docker logs <container_id>
```

`STATUS`가 `Exited`로 표시되는 것을 확인하였고, 컨테이너의 메인 프로세스(`bash`)가 종료되면서 컨테이너도 함께 종료되는 Docker의 동작 방식을 확인하였습니다.

#### **해결/대안**:

```
ocker run -dit --name ubuntu-test ubuntu sleep infinity
docker exec -it ubuntu-test bash
```

메인 프로세스가 계속 실행되도록 `sleep infinity`를 사용하여 컨테이너를 유지하고, `docker exec`로 내부에 접속하여 작업을 진행하였습니다.
