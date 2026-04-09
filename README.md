# 🖥️ docker-cli-notes

리눅스 CLI와 Docker 기반의 개발 워크스테이션을 직접 구성하고,  
터미널 조작, 권한 설정, 컨테이너 실행/관리, Dockerfile 기반 웹 서버 실행,  
포트 매핑, 바인드 마운트, 볼륨 영속성, Git/GitHub 연동 과정을 기록한 저장소입니다.

---

## 1. 프로젝트 개요

이 저장소는 다음 내용을 직접 실습하고 검증하기 위해 만들었습니다.

- Linux CLI 기본 조작
- 파일/디렉토리 권한 확인 및 변경
- Docker 설치 상태 점검
- 컨테이너 실행 및 관리
- Dockerfile 기반 커스텀 이미지 제작
- 포트 매핑 검증
- 바인드 마운트 검증
- Docker 볼륨 영속성 검증
- Git / GitHub / VS Code 연동
- 트러블슈팅 기록

---

## 2. 실행 환경

- Host OS: Windows
- WSL: Ubuntu 24.04.4 LTS
- WSL Version: 2
- Linux User: user
- Working Directory: /home/user/docker-cli-notes
- Docker Version: 28.0.1
- Git Version: 2.43.0
- Editor: VS Code (WSL 환경)

---

## ✅ 3. 수행 체크리스트

- [x] WSL 2 Ubuntu 설치
- [x] Ubuntu 홈 디렉토리 작업 환경 구성
- [x] Docker Desktop 연동
- [x] Docker daemon 권한 문제 해결
- [x] 터미널 기본 조작 실습
- [x] 권한 변경 실습
- [x] hello-world 실행
- [x] Ubuntu 컨테이너 진입 실습
- [x] Docker 운영 명령 확인 (`images`, `ps -a`, `logs`, `stats`)
- [x] Dockerfile 기반 웹 서버 이미지 작성
- [x] 포트 매핑 접속 검증
- [x] 바인드 마운트 반영 검증
- [x] Docker 볼륨 영속성 검증
- [x] Git 설정 및 GitHub 연동
- [x] 트러블슈팅 기록
- [x] README 최종 정리

---

## 💻 4. 터미널 조작 로그

터미널 기본 조작을 통해 현재 위치 확인, 파일/디렉토리 목록 확인, 디렉토리 생성 및 이동, 빈 파일 생성, 파일 내용 확인, 파일 복사, 이름 변경, 삭제, 상위 디렉토리 복귀까지 순서대로 수행했습니다.

### 실행한 핵심 명령

- `pwd`로 현재 작업 경로 확인
- `ls -la`로 현재 디렉토리 구조 확인
- `mkdir practice`로 실습 디렉토리 생성
- `cd practice`로 디렉토리 이동
- `touch test.txt`로 빈 파일 생성
- `cat test.txt`로 파일 내용 확인
- `cp test.txt test-copy.txt`로 파일 복사
- `mv test-copy.txt renamed.txt`로 파일 이름 변경
- `rm renamed.txt`로 파일 삭제
- `cd ..` 및 `ls -la practice`로 상위 디렉토리 복귀 및 내부 상태 확인

추가로 `test.txt` 파일에 `Hello World!` 내용을 입력한 뒤, 복사본과 이름 변경 결과까지 확인했습니다.

자세한 명령과 결과는 아래 문서에 정리했습니다.

- [docs/terminal-log.md](docs/terminal-log.md)

---

## 🔐 5. 권한 실습

권한 실습은 기존 프로젝트 파일을 활용하여 진행했습니다.

- 대상 파일: `app/index.html`
- 대상 디렉토리: `app/`

초기 권한을 확인한 뒤, 임시로 더 제한적인 권한으로 변경하고 다시 최종 권한으로 복구했습니다.

- 초기 파일 권한: `-rw-r--r--` → `644`
- 초기 디렉토리 권한: `drwxr-xr-x` → `755`

임시 변경 후:

- 파일 권한: `-rw-------` → `600`
- 디렉토리 권한: `drwx------` → `700`

최종 복구 후:

- 파일 권한: `-rw-r--r--` → `644`
- 디렉토리 권한: `drwxr-xr-x` → `755`

권한 숫자의 의미는 다음과 같습니다.

- `r = 4` : 읽기
- `w = 2` : 쓰기
- `x = 1` : 실행

따라서,

- `755 = rwx r-x r-x`
- `644 = rw- r-- r--`

자세한 전체 로그는 아래 파일에 정리했습니다.

- [docs/permission-log.md](docs/permission-log.md)

---

## 🐳 6. Docker 기본 점검 및 운영

Docker 기본 점검 및 컨테이너 실행 실습에서는 다음 내용을 확인했습니다.

- `docker --version` 으로 Docker CLI 버전 확인
- `docker info` 로 Docker daemon 동작 여부 확인
- `docker run hello-world` 실행 성공 확인
- `docker run -it ubuntu bash` 로 Ubuntu 컨테이너 내부 진입
- 컨테이너 내부에서 `pwd`, `echo`, `cat /etc/os-release` 실행
- `docker images` 로 이미지 목록 확인
- `docker ps`, `docker ps -a` 로 실행/종료 컨테이너 상태 확인
- `docker logs` 로 hello-world 컨테이너 로그 확인
- `docker stats --no-stream` 로 현재 실행 중인 컨테이너 리소스 사용량 확인

추가로 `attach-exec-demo` 라는 이름의 실습용 Ubuntu 컨테이너를 사용해 `attach` 와 `exec` 의 차이도 확인했습니다.

- `docker attach attach-exec-demo` 는 컨테이너의 메인 프로세스에 직접 연결하는 방식이므로, 셸에서 `exit` 하면 컨테이너도 종료되어 `docker ps -a` 에서 `Exited` 상태로 확인됨
- `docker exec -it attach-exec-demo bash` 는 실행 중인 컨테이너 안에 새로운 셸을 여는 방식이므로, exec 셸에서 `exit` 해도 원래 컨테이너는 계속 실행 상태(`Up`)를 유지함

자세한 내용은 아래 파일에 정리했습니다.

- [docs/docker-log.md](docs/docker-log.md)

---

## 🌐 7. 커스텀 웹 서버 이미지

기존 웹 서버 베이스 이미지 방식으로 `nginx:alpine` 를 선택했습니다.  
정적 HTML 한 장을 가장 단순하고 안정적으로 서빙할 수 있어, **웹 서버 베이스 이미지 활용 + 정적 콘텐츠 교체** 으로 진행했습니다.

적용한 커스텀 포인트는 다음과 같습니다.

- `FROM nginx:alpine`
  - 경량 NGINX 웹 서버 이미지를 베이스로 사용
- `COPY app/index.html /usr/share/nginx/html/index.html`
  - 기본 index 페이지를 내가 만든 정적 HTML로 교체

### 사용한 Dockerfile:

```dockerfile
FROM nginx:alpine

COPY app/index.html /usr/share/nginx/html/index.html
```

### 빌드 및 실행 명령

```bash
$ docker build -t docker-cli-web:1.0 .
$ docker run -d --name docker-cli-web -p 8080:80 docker-cli-web:1.0
$ docker ps
```

### 핵심 결과:

- `docker-cli-web:1.0` 이미지 빌드 성공
- `docker-cli-web` 컨테이너 실행 성공
- `0.0.0.0:8080->80/tcp` 포트 매핑 확인

자세한 로그는 아래 문서에 정리했습니다.

- [docs/docker-log.md](docs/docker-log.md)

---

## 🔌 8. 포트 매핑 검증

브라우저와 curl을 사용해 포트 매핑 결과를 확인했습니다.

```bash
$ curl http://localhost:8080
```

확인 결과, 컨테이너 내부의 NGINX 웹 서버가 호스트의 8080 포트로 정상 노출되었고, 작성한 HTML 페이지가 응답으로 반환되었습니다.

### 브라우저 접속 화면:

- [8080 접속 화면](docs/screenshots/browser-8080.jpg)

---

## 🔗 9. 바인드 마운트 검증

호스트의 app/ 디렉토리를 컨테이너의 웹 루트에 읽기 전용으로 바인드 마운트하여, 호스트 파일 수정이 컨테이너에 즉시 반영되는지 확인했습니다.

### 실행 명령:

```bash
$ docker run -d --name docker-cli-bind -p 8081:80 -v "$(pwd)/app:/usr/share/nginx/html:ro" nginx:alpine
$ docker ps --filter "name=docker-cli-bind"
$ curl http://localhost:8081
```

이후 `app/index.html`에 아래 문장을 추가했습니다.

```HTML
<p>Live content update through bind mount.</p>
```

컨테이너를 재생성하지 않고 다시 확인했습니다.

```bash
$ curl http://localhost:8081
```

확인 결과, 호스트 파일 변경이 컨테이너 웹 페이지에 즉시 반영되었습니다.

### 브라우저 접속 화면:

- [8081 수정 전](docs/screenshots/browser-8081-before.jpg)
- [8081 수정 후](docs/screenshots/browser-8081-after.jpg)

---

## 10. 💾 볼륨 영속성 검증

named volume docker-cli-data를 생성하고, 첫 번째 컨테이너에서 파일을 만든 뒤 컨테이너를 삭제했습니다.
이후 같은 volume을 두 번째 컨테이너에 다시 연결해 동일한 파일 내용을 읽어, 데이터가 유지됨을 확인했습니다.

### 실행 명령:

```bash
$ docker volume create docker-cli-data
$ docker run -d --name volume-write -v docker-cli-data:/data ubuntu sleep infinity
$ docker exec volume-write bash -lc 'echo "persistent data check" > /data/note.txt && cat /data/note.txt'
$ docker rm -f volume-write
$ docker run -d --name volume-read -v docker-cli-data:/data ubuntu sleep infinity
$ docker exec volume-read bash -lc 'cat /data/note.txt'
```

확인 결과, 첫 번째 컨테이너 삭제 후에도 note.txt 내용이 그대로 유지되었습니다.

자세한 로그는 아래 문서에 정리했습니다.

- [docs/docker-log.md](docs/docker-log.md)

---

## 🔄 11. Git / GitHub / VS Code 연동

Git 설정, 현재 브랜치, 원격 저장소 연결 상태를 확인했습니다.

### 실행 명령:

```bash
$ git config --list
$ git branch --show-current
$ git remote -v
$ git status
```

### 핵심 결과

- 현재 브랜치: `main`
- 원격 저장소 `origin` 연결 확인
- VS Code Source Control에서 현재 저장소 변경 사항 확인
- 문서에는 이메일 등 민감정보를 마스킹하여 기록

### VS Code 화면:

- [VS Code 저장소 화면](docs/screenshots/vscode-source-control.jpg)

---

## 🐳 12. Docker Compose 기초 및 운영 명령어

`docker-compose.yml` 을 루트 디렉토리에 추가하고, 단일 웹 서비스를 Compose로 실행했습니다.  
이번 구성은 기존 `Dockerfile` 을 그대로 재사용하면서 실행 설정을 파일로 문서화하는 방식입니다.

### 사용한 docker-compose.yml

```yaml
services:
  web:
    build: .
    container_name: docker-cli-compose
    ports:
      - '8082:80'
```

### 실행 명령

```bash
$ docker compose up -d
$ docker compose ps
$ docker compose logs
$ curl http://localhost:8082
$ docker compose down
```

### 핵심 결과

- `docker compose up -d` 로 이미지 빌드와 컨테이너 실행을 한 번에 수행
- `docker compose ps` 에서 `docker-cli-compose` 컨테이너가 `0.0.0.0:8082->80/tcp` 로 실행 중임을 확인
- `docker compose logs` 로 NGINX 시작 로그 확인
- `curl http://localhost:8082` 로 페이지 응답 확인
- `docker compose down` 으로 컨테이너와 네트워크 정리 완료

자세한 로그는 아래 문서에 정리했습니다.

- [docs/docker-log.md](docs/docker-log.md)

---

## 🚨 13. 트러블슈팅

실습 중 실제로 발생했거나, 진행 중 핵심적으로 확인한 문제를 정리했습니다.

- Docker daemon 연결/권한 문제
- `docker attach` 후 `exit` 시 컨테이너가 종료되는 문제

각 문제에 대해 원인 가설, 확인 과정, 해결 방법, 재발 방지 포인트를 아래 문서에 정리했습니다.

- [docs/trouble-shooting.md](docs/trouble-shooting.md)

---

## 💡 14. 배운 점

이번 미션을 통해 아래 내용을 직접 확인할 수 있었습니다.

- 이미지와 컨테이너는 분리된 개념이며, 같은 이미지로 여러 컨테이너를 실행할 수 있다.
- 포트 매핑은 컨테이너 내부 서비스를 호스트에서 접근 가능하게 연결하는 과정이다.
- 바인드 마운트는 호스트 파일 변경을 즉시 반영하는 데 유리하고, Docker volume은 컨테이너 삭제 이후에도 데이터를 유지하는 데 적합하다.
- Docker Compose를 사용하면 실행 명령을 파일로 관리할 수 있어 재현성과 문서화 측면에서 유리하다.
