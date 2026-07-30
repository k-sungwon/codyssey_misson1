# Codyssey 개발 워크스테이션 미션

## 0. 프로젝트 개요

<!-- 미션 목표를 2~4줄로 요약. 예: 터미널/Docker/Git을 직접 세팅하고, 웹 서버를 컨테이너화해서
포트 매핑·바인드 마운트·볼륨 영속성을 직접 검증한 개발 워크스테이션 구축 기록. -->

TODO: 미션 목표 요약 작성

---

## 1. 실행 환경

| 항목               | 값                                                               |
| ------------------ | ---------------------------------------------------------------- |
| OS                 | TODO (예: macOS 14.x)                                            |
| Shell              | TODO (예: zsh)                                                   |
| Docker             | TODO (`docker --version` 결과)                                   |
| Git                | TODO (`git --version` 결과)                                      |
| 컨테이너 실행 도구 | TODO (Docker Desktop 또는 OrbStack — 아래 "환경 관련 참고" 참고) |

**환경 관련 참고:**

> 과제 안내에는 서울캠퍼스 sudo 제한 때문에 OrbStack 사용을 권장하고 있으나, 본인 iMac 환경에는
> Docker Desktop이 이미 설치되어 있고 sudo 없이 정상 동작하여(OrbStack이 필요한 이유와 동일한 조건을
> 만족) Docker Desktop을 그대로 사용함.
> TODO: 실제 사용한 도구에 맞게 이 문단 수정/삭제

---

## 2. 수행 항목 체크리스트

- [ ] 터미널 기본 조작 (이동/생성/복사/이동·이름변경/삭제/내용확인/빈파일생성)
- [ ] 파일 권한 확인/변경 실습 (파일 1개, 디렉토리 1개)
- [ ] Docker 설치 확인 (`docker --version`)
- [ ] Docker 데몬 동작 확인 (`docker info`)
- [ ] Docker 기본 운영 명령 (`images`, `ps`, `ps -a`, `logs`, `stats`)
- [ ] `hello-world` 컨테이너 실행
- [ ] `ubuntu` 컨테이너 실행 + 내부 명령 수행 (`ls`, `echo` 등)
- [ ] 컨테이너 종료/유지(attach vs exec) 차이 관찰 및 정리
- [ ] 웹 서버 코드 작성
- [ ] Dockerfile 작성 (기존 베이스 이미지 기반 커스텀 이미지)
- [ ] 이미지 빌드 및 컨테이너 실행 성공
- [ ] 포트 매핑 후 접속 성공 (2회 이상, 서로 다른 호스트 포트)
- [ ] 바인드 마운트 반영 확인 (호스트 변경 전/후 비교)
- [ ] Docker 볼륨 생성/연결/영속성 검증 (컨테이너 삭제 전/후 비교)
- [ ] Git 사용자 정보 및 기본 브랜치 설정
- [ ] GitHub 로그인 및 VSCode 연동

---

## 3. 검증 방법 요약

| 검증 대상            | 사용한 명령                               | 결과 위치                                                           |
| -------------------- | ----------------------------------------- | ------------------------------------------------------------------- |
| Docker 설치/데몬     | `docker --version`, `docker info`         | [2절](#2-수행-항목-체크리스트) / [5절](#5-docker-설치-및-기본-점검) |
| 이미지/컨테이너 목록 | `docker images`, `docker ps -a`           | [6절](#6-docker-기본-운영-명령)                                     |
| 커스텀 이미지 빌드   | `docker build -t ...`                     | [8절](#8-dockerfile-기반-커스텀-이미지)                             |
| 포트 매핑            | `curl localhost:<port>` (2개 포트)        | [9절](#9-포트-매핑-및-접속-증거)                                    |
| 바인드 마운트        | 호스트 파일 수정 → `docker exec ... cat`  | [10절](#10-바인드-마운트-검증)                                      |
| 볼륨 영속성          | 컨테이너 삭제 전/후 `docker exec ... cat` | [11절](#11-docker-볼륨-영속성-검증)                                 |
| Git/GitHub 연동      | `git config --list`, VSCode 스크린샷      | [12절](#12-git-설정-및-github-연동)                                 |

---

## 4. 터미널 기본 조작 로그

<!-- pwd, ls -la, mkdir, cp, mv, rm, cat, touch 등 -->

```bash
TODO: 명령어 + 출력 붙여넣기
```

### 4-1. 파일/디렉토리 권한 실습

```bash
TODO: 변경 전 ls -l 결과
TODO: chmod 명령
TODO: 변경 후 ls -l 결과
```

권한 표기(예: 755, 644) 해석:

> TODO: 자릿수별 의미(소유자/그룹/전체 × r/w/x) 한두 줄로 정리

---

## 5. Docker 설치 및 기본 점검

```bash
$ docker --version
TODO

$ docker info
TODO
```

---

## 6. Docker 기본 운영 명령

```bash
$ docker images
TODO

$ docker ps -a
TODO

$ docker logs <container>
TODO

$ docker stats
TODO
```

---

## 7. 컨테이너 실행 실습

### 7-1. hello-world

```bash
$ docker run hello-world
TODO
```

### 7-2. ubuntu 컨테이너 진입 및 명령 수행

```bash
TODO: docker run -it ubuntu 등으로 진입 후 ls, echo 결과
```

### 7-3. attach vs exec 관찰 정리

> TODO: 두 방식의 차이를 직접 관찰한 대로 2~3줄로 정리
> (예: exec는 메인 프로세스 안 건드리고 새 프로세스 추가 실행이라 종료해도 컨테이너 유지,
> attach는 메인 프로세스에 직접 연결이라 종료 시 컨테이너도 함께 종료)

---

## 8. Dockerfile 기반 커스텀 이미지

**선택한 베이스**: TODO (예: `node:20-alpine`)

**적용한 커스텀 포인트**:
| 항목 | 목적 |
|---|---|
| TODO | TODO |

### 8-1. 웹 서버 소스코드

```javascript
TODO: server.js 내용
```

### 8-2. Dockerfile

```dockerfile
TODO: Dockerfile 내용
```

### 8-3. 빌드 및 실행

```bash
$ docker build -t my-web:1.0 .
TODO

$ docker run -d -p 8080:5050 --name my-web-8080 my-web:1.0
TODO
```

---

## 9. 포트 매핑 및 접속 증거

```bash
$ docker run -d -p 8080:5050 --name my-web-8080 my-web:1.0
$ curl localhost:8080
TODO

$ docker run -d -p 8081:5050 --name my-web-8081 my-web:1.0
$ curl localhost:8081
TODO
```

**브라우저 접속 스크린샷** (주소창 + 응답 화면):

> TODO: 이미지 삽입 `![포트매핑 접속1](경로)` (2개, 서로 다른 포트)

---

## 10. 바인드 마운트 검증

```bash
# 마운트 없이 실행 중인 컨테이너에서 파일 확인
$ docker exec my-web-8080 cat /app/server.js
TODO (변경 전)

# 호스트 파일 수정
$ TODO: 파일 수정 명령

# 마운트 없는 컨테이너는 반영 안 됨
$ docker exec my-web-8080 cat /app/server.js
TODO (반영 안 됨 확인)

# 바인드 마운트로 새 컨테이너 실행
$ docker run -d -p 8090:5050 --name my-web-mount -v "$(pwd):/app" my-web:1.0
$ docker exec my-web-mount cat /app/server.js
TODO (반영됨 확인)
```

---

## 11. Docker 볼륨 영속성 검증

```bash
$ docker volume create mydata
$ docker run -d --name vol-test -v mydata:/data ubuntu sleep infinity
$ docker exec vol-test bash -c "echo hi > /data/hello.txt && cat /data/hello.txt"
TODO

$ docker rm -f vol-test
$ docker run -d --name vol-test2 -v mydata:/data ubuntu sleep infinity
$ docker exec vol-test2 cat /data/hello.txt
TODO (컨테이너 삭제 후에도 데이터 유지 확인)
```

---

## 12. Git 설정 및 GitHub 연동

```bash
$ git config --list
TODO
```

**VSCode ↔ GitHub 연동 스크린샷**:

> TODO: 이미지 삽입 (로그인 상태 + 저장소 연동 화면)

> 민감정보(토큰/비밀번호 등)는 스크린샷에서 마스킹 처리했습니다.

---

## 13. 트러블슈팅

### 13-1. TODO: 문제 제목 (예: 포트 충돌)

- **문제**: TODO
- **원인 가설**: TODO
- **확인**: TODO (사용한 명령 + 결과)
- **해결/대안**: TODO

### 13-2. TODO: 문제 제목

- **문제**: TODO
- **원인 가설**: TODO
- **확인**: TODO
- **해결/대안**: TODO

---

## 14. 배운 점 / 개념 정리 (과제 목표 대응)

- **절대경로 vs 상대경로**: TODO (예시 포함)
- **파일 권한(r/w/x, 755/644)**: TODO
- **기존 Dockerfile 기반 커스텀 이미지 제작**: TODO
- **포트 매핑이 필요한 이유**: TODO
- **Docker 볼륨(영속 데이터)**: TODO
- **Git vs GitHub 역할 차이**: TODO
