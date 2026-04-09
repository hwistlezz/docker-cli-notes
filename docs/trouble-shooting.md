# 🚨 Trouble Shooting

실습 과정에서 실제로 발생했거나, 진행 중 핵심적으로 확인한 문제 상황과 해결 과정을 기록했습니다.

## 🐳 1. Docker daemon 연결/권한 문제

### ❗ 문제

WSL Ubuntu 환경에서 Docker 명령은 입력되지만, Docker daemon에 연결되지 않거나 권한 문제로 인해 컨테이너 실행이 되지 않는 상황이 발생했다.

### 🧩 원인 가설

- Docker Desktop(또는 대체 Docker 엔진)이 실행되지 않았을 수 있다.
- WSL과 Docker 엔진 연동이 정상적으로 되지 않았을 수 있다.
- 현재 사용자가 Docker 소켓에 접근할 권한이 없을 수 있다.

### 🔍 확인 과정

- `docker --version` 으로 CLI 자체는 설치되어 있는지 확인했다.
- `docker info` 로 Docker daemon(Server) 정보가 정상적으로 조회되는지 확인했다.
- 필요 시 Docker 엔진 실행 여부, WSL 연동 여부, 사용자 권한 상태를 함께 점검했다.

### ✅ 해결 방법

- Docker Desktop을 실행하고 WSL 연동 상태를 다시 확인했다.
- 사용자 권한 문제인 경우 Docker 그룹 권한을 반영해 접근 가능하도록 조치했다.
- 실습 환경에서 `sudo` 사용 제약이 있는 경우, 미션 안내에 따라 제공된 실행 환경을 활용할 수 있음을 확인했다.
- 최종적으로 `docker info` 에서 Client뿐 아니라 Server 정보까지 정상 출력되는 것을 확인했다.

### 🛡️ 대안 또는 재발 방지 포인트

- Docker 작업 전 가장 먼저 `docker info` 로 daemon 연결 상태를 확인한다.
- WSL 환경에서는 Docker Desktop 실행 여부와 WSL integration 상태를 먼저 점검한다.
- 권한 문제를 해결한 뒤에는 터미널 세션을 다시 열어 권한 반영 여부를 확인한다.
- 제한된 실습 환경에서는 일반적인 설치 방식보다 제공된 Docker 실행 환경을 우선 확인한다.

---

## 2.🔌 docker attach 후 exit 시 컨테이너가 종료되는 문제

### ❗ 문제

실행 중인 Ubuntu 컨테이너에 들어가 작업한 뒤 `exit` 했더니, 셸만 종료될 것이라고 생각했지만 컨테이너 자체가 종료되었다.

### 🧩 원인 가설

- `docker attach` 와 `docker exec` 의 동작 차이를 정확히 구분하지 못했다.
- `attach` 는 실행 중인 컨테이너의 메인 프로세스에 직접 연결되므로, 메인 프로세스가 종료되면 컨테이너도 함께 종료된다.

### 🔍 확인 과정

1. `docker run -dit --name attach-exec-demo ubuntu bash` 로 실습용 컨테이너를 실행했다.
2. `docker attach attach-exec-demo` 로 메인 bash 프로세스에 직접 접속했다.
3. 컨테이너 내부에서 `exit` 한 뒤 `docker ps -a --filter "name=attach-exec-demo"` 로 상태를 확인했다.
4. 컨테이너가 `Exited` 상태로 바뀐 것을 확인했다.
5. 이후 `docker start attach-exec-demo` 로 컨테이너를 다시 시작했다.
6. 이번에는 `docker exec -it attach-exec-demo bash` 로 새 셸을 열어 들어갔다.
7. `exit` 후 `docker ps --filter "name=attach-exec-demo"` 로 상태를 확인했다.
8. 이번에는 컨테이너가 계속 `Up` 상태를 유지하는 것을 확인했다.

### ✅ 해결 방법

- 컨테이너를 유지한 채 내부에서 작업만 하고 싶을 때는 `docker exec -it <container> bash` 를 사용한다.
- `docker attach` 는 컨테이너의 메인 프로세스 자체에 연결해야 할 특별한 이유가 있을 때만 사용한다.

### 🛡️ 대안 또는 재발 방지 포인트

- 장시간 유지할 컨테이너에 들어갈 때는 기본적으로 `exec` 를 우선 사용한다.
- `attach + exit` 는 컨테이너 종료로 이어질 수 있다는 점을 기억한다.
- 실습용 컨테이너는 `docker run -dit ...` 로 백그라운드 실행 후 `exec` 로 접근하는 패턴을 습관화하면 안전하다.

---
