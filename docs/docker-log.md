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
$ docker --version
Docker version 28.0.1, build 068a01e
```

### 👉 설명

- Docker CLI가 정상 설치되어 있음을 확인

---

## ⚙️ 2. Docker daemon 동작 확인

```bash
$ docker info
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
$ docker run hello-world
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
$ docker run -it ubuntu bash
```

컨테이너 내부에서 아래 명령을 실행했습니다.

```bash
$ pwd
/
$ echo "hello from ubuntu container"
hello from ubuntu container

$ cat /etc/os-release
PRETTY_NAME="Ubuntu 24.04.4 LTS"
...
$ exit
```

### 👉 설명

- `ubuntu` 이미지를 다운로드한 후, 컨테이너 내부 셸에 진입
- 컨테이너 내부에서 현재 위치 확인, 문자열 출력, OS 정보 확인을 수행
- `exit` 후 컨테이너 셸을 종료

---

## 🖼️ 5. 이미지 목록 확인

```bash
$ docker images
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
$ docker ps -a
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
$ docker ps
```

### 핵심 출력 일부

```text
CONTAINER ID   IMAGE    COMMAND   STATUS   PORTS   NAMES
...            ...      ...       Up ...           ...
```

---

## 📜 7. 컨테이너 로그 확인

```bash
$ docker logs $(docker ps -aq --filter "ancestor=hello-world" | head -n 1)
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
$ docker stats --no-stream
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
$ docker run -dit --name attach-exec-demo ubuntu bash
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
$ docker attach attach-exec-demo
```

컨테이너 내부에서:

```bash
$ echo "attached to main bash"
$ pwd
$ exit
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
$ docker ps -a --filter "name=attach-exec-demo"
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
$ docker start attach-exec-demo
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
$ docker exec -it attach-exec-demo bash
```

컨테이너 내부에서:

```bash
$ echo "inside exec shell"
$ pwd
$ exit
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
$ docker ps --filter "name=attach-exec-demo"
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

## 🌐 10. Dockerfile 기반 커스텀 웹 서버 이미지 제작

### 10-1. 선택한 베이스 이미지와 커스텀 포인트

이번 실습에서는 기존 웹 서버 베이스 이미지 방식으로 `nginx:alpine` 를 선택했습니다.

적용한 커스텀 포인트:

- `FROM nginx:alpine`
  - 경량 웹 서버 이미지를 베이스로 사용
- `COPY app/index.html /usr/share/nginx/html/index.html`
  - 기본 index 페이지를 내가 만든 정적 HTML로 교체

### 사용한 Dockerfile:

```dockerfile
FROM nginx:alpine

COPY app/index.html /usr/share/nginx/html/index.html
```

### 10-2. 이미지 빌드

```bash
$ docker build -t docker-cli-web:1.0 .
```

### 핵심 출력 일부

```text
[+] Building ... FINISHED
=> naming to docker.io/library/docker-cli-web:1.0
=> unpacking to docker.io/library/docker-cli-web:1.0
```

### 👉 설명

- `docker-cli-web:1.0` 이름의 커스텀 이미지가 정상적으로 빌드됨

---

### 10-3. 이미지 목록 확인

```bash
$ docker images
```

### 핵심 출력 일부

```text
REPOSITORY       TAG      IMAGE ID       CREATED         SIZE
docker-cli-web   1.0      1537bb123393   2 minutes ago   92.6MB
ubuntu           latest   ...
hello-world      latest   ...
nginx            latest   ...
```

### 👉 설명

- 커스텀 이미지 `docker-cli-web:1.0` 이 로컬 이미지 목록에 생성된 것을 확인

---

### 10-4. 커스텀 웹 서버 컨테이너 실행

```bash
$ docker run -d --name docker-cli-web -p 8080:80 docker-cli-web:1.0
$ docker ps
```

### 핵심 출력 일부

```text
<container_id>

CONTAINER ID   IMAGE                COMMAND                  STATUS         PORTS                  NAMES
...            docker-cli-web:1.0   "/docker-entrypoint.…"   Up ...         0.0.0.0:8080->80/tcp   docker-cli-web
```

### 👉 설명

- 커스텀 이미지로 컨테이너를 실행했고, 호스트 `8080` 포트가 컨테이너 `80` 포트에 연결됨

---

### 10-5. 포트 매핑 응답 확인

```bash
$ curl http://localhost:8080
```

### 핵심 출력 일부

```text
<!doctype html>
<html lang="ko">
  <head>
    <title>docker-cli-notes</title>
  </head>
  <body>
    <h1>Docker CLI Notes</h1>
    <p>nginx 기반 커스텀 웹 서버 컨테이너 실행 확인 페이지입니다.</p>
    <p>Custom Web Server Image</p>
  </body>
</html>
```

### 👉 설명

- 호스트 `8080` 포트로 접속했을 때, 컨테이너 내부의 NGINX가 작성한 HTML 페이지를 정상 응답함

---

## 🔗 11. 바인드 마운트 반영 검증

### 11-1. 바인드 마운트 컨테이너 실행

```bash
$ docker run -d --name docker-cli-bind -p 8081:80 -v "$(pwd)/app:/usr/share/nginx/html:ro" nginx:alpine
$ docker ps --filter "name=docker-cli-bind"
```

### 핵심 출력 일부

```text
<container_id>

CONTAINER ID   IMAGE          COMMAND                  STATUS         PORTS                  NAMES
...            nginx:alpine   "/docker-entrypoint.…"   Up ...         0.0.0.0:8081->80/tcp   docker-cli-bind
```

### 👉 설명

- 호스트의 `app/` 디렉토리를 컨테이너 웹 루트에 읽기 전용으로 연결함
- `8081` 포트로 바인드 마운트 검증용 컨테이너를 별도로 실행함

---

### 11-2. 수정 전 응답 확인

```bash
$ curl http://localhost:8081
```

### 핵심 출력 일부

```HTML
<!doctype html>
<html lang="ko">
  <head>
    <title>docker-cli-notes</title>
  </head>
  <body>
    <h1>Docker CLI Notes</h1>
    <p>nginx 기반 커스텀 웹 서버 컨테이너 실행 확인 페이지입니다.</p>
    <p>Custom Web Server Image</p>
  </body>
</html>
```

### 👉 설명

- 바인드 마운트 직후에는 기존 `index.html` 내용이 그대로 응답됨

---

### 11-3. 호스트 파일 수정 후 다시 확인

`app/index.html` 에 아래 문장을 추가한 뒤, 컨테이너 재실행 없이 다시 확인했습니다.

```HTML
<p>Live content update through bind mount.</p>
```

```bash
<p>Live content update through bind mount.</p>
```

### 출력

```HTML
<!doctype html>
<html lang="ko">
  <head>
    <title>docker-cli-notes</title>
  </head>
  <body>
    <h1>Docker CLI Notes</h1>
    <p>nginx 기반 커스텀 웹 서버 컨테이너 실행 확인 페이지입니다.</p>
    <p>Custom Web Server Image</p>
    <p>Live content update through bind mount.</p>
  </body>
</html>
```

### 👉 설명

- 이미지를 다시 빌드하거나 컨테이너를 다시 만들지 않아도, 호스트 파일 수정 내용이 즉시 반영됨
- 바인드 마운트가 컨테이너 내부와 호스트 파일을 직접 연결한다는 점을 확인함

---

## 💾 12. Docker 볼륨 영속성 검증

### 12-1. named volume 생성

```bash
$ docker volume create docker-cli-data
```

### 핵심 출력 일부

```text
docker-cli-data
```

### 👉 설명

- `docker-cli-data` 라는 이름의 Docker volume을 생성함

---

### 12-2. 첫 번째 컨테이너에서 파일 생성

```bash
$ docker run -d --name volume-write -v docker-cli-data:/data ubuntu sleep infinity
$ docker exec volume-write bash -lc 'echo "persistent data check" > /data/note.txt && cat /data/note.txt'
```

### 핵심 출력 일부

```text
<container_id>
persistent data check
```

### 👉 설명

- 첫 번째 컨테이너에서 `/data/note.txt` 파일을 생성하고 내용을 기록함

---

### 12-3. 첫 번째 컨테이너 삭제 후 두 번째 컨테이너에서 재확인

```bash
$ docker rm -f volume-write
$ docker run -d --name volume-read -v docker-cli-data:/data ubuntu sleep infinity
$ docker exec volume-read bash -lc 'cat /data/note.txt'
```

### 핵심 출력 일부

```text
volume-write
<container_id>
persistent data check
```

### 👉 설명

- 첫 번째 컨테이너를 삭제한 뒤에도, 같은 volume을 연결한 두 번째 컨테이너에서 동일한 파일 내용을 다시 읽을 수 있었음
- volume 데이터가 컨테이너 생명주기와 분리되어 유지됨을 확인함

---

### 12-4. volume 목록 확인

```bash
$ docker volume ls
```

### 핵심 출력 일부

```text
DRIVER    VOLUME NAME
local     docker-cli-data
```

### 👉 설명

- 생성한 named volume `docker-cli-data` 가 로컬에 존재함을 확인
  과제와 무관한 다른 volume 이름은 기록에서 제외함

---

## ⚙️ 13. Docker Compose 기초 및 운영 명령어

### 13-1. compose 파일 작성

루트 디렉토리에 `docker-compose.yml` 을 두고 단일 웹 서비스를 정의했습니다.

```yaml
services:
  web:
    build: .
    container_name: docker-cli-compose
    ports:
      - '8082:80'
```

### 👉 설명

- 기존 `Dockerfile` 을 그대로 재사용하도록 `build: .` 로 설정
- 컨테이너 이름을 `docker-cli-compose` 로 설정
- 호스트 `8082` 포트를 컨테이너 `80` 포트에 연결

---

### 13-2 compose로 서비스 실행

```bash
$ docker compose up -d
```

### 핵심 출력 일부

```text
[+] Running 3/3
✔ web                               Built
✔ Network docker-cli-notes_default  Created
✔ Container docker-cli-compose      Started
```

### 👉 설명

- `docker compose up -d` 한 번으로 이미지 빌드, 네트워크 생성, 컨테이너 실행까지 함께 수행됨

---

### 13-3. 실행 상태 확인

```bash
$ docker compose ps
```

### 핵심 출력 일부

```text
NAME                 IMAGE                  SERVICE   STATUS         PORTS
docker-cli-compose   docker-cli-notes-web   web       Up 6 seconds   0.0.0.0:8082->80/tcp
```

### 👉 설명

- Compose 서비스 `web` 이 정상 실행 중이며, `8082:80` 포트 매핑이 적용됨을 확인

---

### 13-4. 로그 확인

```bash
$ docker compose logs
```

### 핵심 출력 일부

```text
docker-cli-compose  | /docker-entrypoint.sh: Configuration complete; ready for start up
docker-cli-compose  | 2026/04/09 06:58:47 [notice] 1#1: nginx/1.29.8
```

### 👉 설명

- Compose 환경에서도 NGINX가 정상적으로 시작되었음을 로그로 확인

---

### 13-5. 응답 확인

```bash
$ curl http://localhost:8082
```

### 핵심 출력 일부

```HTML
<!doctype html>
<html lang="ko">
  <head>
    <title>docker-cli-notes</title>
  </head>
  <body>
    <h1>Docker CLI Notes</h1>
    <p>nginx 기반 커스텀 웹 서버 컨테이너 실행 확인 페이지입니다.</p>
    <p>Custom Web Server Image</p>
    <p>Live content update through bind mount.</p>
  </body>
</html>
```

### 👉 설명

- Compose로 실행한 컨테이너에서도 동일한 웹 페이지가 정상 응답됨을 확인

---

### 13-4. compose 환경 정리

```bash
$ docker compose down
```

### 핵심 출력 일부

```text
✔ Container docker-cli-compose      Removed
✔ Network docker-cli-notes_default  Removed
```

### 👉 설명

- `docker compose down` 으로 실행한 컨테이너와 네트워크를 정리함
- `up` / `ps` / `logs` / `down` 를 사용해 실행·상태 확인·로그 확인·종료 및 정리를 수행함

---
