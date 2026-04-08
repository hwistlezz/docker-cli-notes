# 🐳 Docker Log

이 파일에는 Docker 설치 확인, 컨테이너 실행/관리, 이미지/로그/리소스 확인 결과를 기록합니다.

## 기록 원칙

- 명령어와 출력 결과를 함께 남긴다.
- 불필요한 민감 정보(프록시, 내부 레지스트리, 개인 식별 정보 등)는 제외한다.
- 브라우저 확인은 스크린샷과 함께 README에 반영한다.
- README 6번 섹션에는 이 로그의 핵심 결과를 정리해서 반영한다.

---

## 🔍 1. Docker 버전 확인

```bash
docker --version
Docker version 28.0.1, build 068a01e
```

### 👉 설명

- Docker CLI가 정상 설치되어 있음을 확인

---

## ⚙️ 2. Docker daemon 동작 확인

```bash
docker info
```

### 핵심 출력 일부

```text
Client:
 Version:    28.0.1

Server:
 Containers: ...
 Images: ...
 Server Version: 28.0.1
 Operating System: Docker Desktop
 OSType: linux
```

### 👉 설명

- `docker info` 결과에서 Client와 Server가 모두 출력되어 Docker daemon이 정상 동작함을 확인

---

## 👋 3. hello-world 컨테이너 실행

```bash
docker run hello-world
```

### 핵심 출력

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### 👉 설명

- `hello-world` 이미지를 다운로드하고 컨테이너를 실행했다.
- Docker 설치 및 기본 실행 흐름이 정상이라는 것을 확인했다.

---

## 💻 4. Ubuntu 컨테이너 내부 진입

```bash
docker run -it ubuntu bash
```

컨테이너 내부에서 아래 명령을 실행했습니다.

```bash
pwd
/
echo "hello from ubuntu container"
hello from ubuntu container

cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04.4 LTS"
...
exit
```

### 👉 설명

- `ubuntu` 이미지를 다운로드한 후, 컨테이너 내부 셸에 진입
- 컨테이너 내부에서 현재 위치 확인, 문자열 출력, OS 정보 확인을 수행
- `exit` 후 컨테이너 셸을 종료

---

## 🖼️ 5. 이미지 목록 확인

```bash
docker images
```

### 핵심 출력 일부

```text
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
ubuntu        latest    ...
hello-world   latest    ...
nginx         latest    ...
```

### 👉 설명

- 이번 실습에서 사용한 ubuntu, hello-world 이미지가 로컬에 존재함을 확인함
- 기존에 다른 프로젝트에서 사용하던 이미지들도 함께 존재했지만, 본 로그에는 핵심 이미지 위주로만 정리함

---

## 📦 6. 컨테이너 목록 확인

```bash
docker ps -a
```

### 핵심 출력 일부

```text
CONTAINER ID   IMAGE         COMMAND    STATUS                     NAMES
...            ubuntu        "bash"     Exited (0) ...             ...
...            hello-world   "/hello"   Exited (0) ...             ...
```

### 👉 설명

- docker ps -a 로 종료된 컨테이너까지 포함한 전체 컨테이너 상태를 확인함
- hello-world 컨테이너는 실행 후 자동 종료되었고, ubuntu 컨테이너는 셸에서 exit 후 종료된 상태로 확인됨

추가로 실행 중인 컨테이너만 보기 위해 아래 명령을 사용했습니다.

```bash
docker ps
```

### 핵심 출력 일부

```text
CONTAINER ID   IMAGE    COMMAND   STATUS   PORTS   NAMES
...            ...      ...       Up ...           ...
```

---

## 📜 7. 컨테이너 로그 확인

```bash
docker logs $(docker ps -aq --filter "ancestor=hello-world" | head -n 1)
```

### 핵심 출력

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### 👉 설명

- `hello-world` 컨테이너의 로그를 다시 확인했다.
- 실행 당시 터미널에 출력된 메시지를 `docker logs` 로도 조회할 수 있음을 확인

---

## 📊 8. 리소스 사용량 확인

```bash
docker stats --no-stream
```

### 핵심 출력 일부

```text
CONTAINER ID   NAME         CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS
...            nginx        ...       ...                 ...       ...       ...         ...
...            app-container ...      ...                 ...       ...       ...         ...
```

### 👉 설명

- 현재 실행 중인 컨테이너의 CPU, 메모리, 네트워크 I/O 등을 한 번만 출력하도록 확인함
- 과제와 직접 관련 없는 기존 개발용 컨테이너 이름 및 세부 식별 정보는 일부 비식별화하여 기록함

---

## 🖥️ 9. 컨테이너 종료/유지 차이 관찰

`attach` 와 `exec` 의 차이를 확인하기 위해 `attach-exec-demo` 라는 이름의 Ubuntu 컨테이너를 사용해 실습했습니다.

### 9-1. 실습용 컨테이너 생성

```bash
docker run -dit --name attach-exec-demo ubuntu bash
```

### 핵심 출력

```text
<container_id>
```

### 👉 설명

- `ubuntu` 이미지를 기반으로 `attach-exec-demo` 컨테이너를 생성하고 백그라운드에서 실행함

---

### 9-2. attach로 메인 프로세스에 접속

```bash
docker attach attach-exec-demo
```

컨테이너 내부에서:

```bash
echo "attached to main bash"
pwd
exit
```

### 핵심 출력

```bash
attached to main bash
/
```

### 👉 설명

- `docker attach` 는 컨테이너의 메인 프로세스에 직접 연결하는 방식임
- 메인 bash 셸에서 `exit` 하면 컨테이너도 종료됨

---

### 9-3. attach 종료 후 컨테이너 상태 확인

```bash
docker ps -a --filter "name=attach-exec-demo"
```

### 핵심 출력

```bash
CONTAINER ID   IMAGE    COMMAND   CREATED   STATUS          PORTS   NAMES
...            ubuntu   "bash"    ...       Exited (0) ...          attach-exec-demo
```

### 👉 설명

- `attach` 상태에서 `exit` 했기 때문에 컨테이너가 `Exited` 상태로 바뀜

---

### 9-4. 컨테이너 다시 시작

```bash
docker start attach-exec-demo
```

### 핵심 출력

```text
attach-exec-demo
```

### 👉 설명

- 종료된 컨테이너를 다시 실행 상태로 변경함

---

### 9-5. exec로 실행 중인 컨테이너 내부 진입

```bash
docker exec -it attach-exec-demo bash
```

컨테이너 내부에서:

```bash
echo "inside exec shell"
pwd
exit
```

### 핵심 출력

```bash
inside exec shell
/
```

### 👉 설명

- `docker exec -it` 는 이미 실행 중인 컨테이너 안에 새로운 셸을 여는 방식임
- 이 셸에서 `exit` 하더라도, 원래 컨테이너는 종료되지 않음

---

### 9-6. exec 종료 후 컨테이너 상태 확인

```bash
docker ps --filter "name=attach-exec-demo"
```

### 핵심 출력

```bash
CONTAINER ID   IMAGE    COMMAND   CREATED   STATUS   PORTS   NAMES
...            ubuntu   "bash"    ...       Up ...           attach-exec-demo
```

### 👉 설명

- `exec` 셸을 종료한 뒤에도 원래 컨테이너는 계속 Up 상태를 유지함

---

### 9-7. attach 와 exec 차이 정리

- `docker attach` 는 컨테이너의 메인 프로세스에 직접 연결함
- 따라서 메인 셸에서 `exit` 하면 컨테이너도 함께 종료된다.

---

- `docker exec -it` 는 이미 실행 중인 컨테이너 안에 새로운 셸을 여는 방식임
- 따라서 `exec` 셸에서 `exit` 하더라도, 원래 컨테이너는 계속 실행 상태를 유지함

### 정리

- `attach + exit` → 컨테이너 종료
- `exec + exit` → exec 셸만 종료, 원래 컨테이너는 유지

---
