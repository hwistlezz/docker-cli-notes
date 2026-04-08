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

## 3. 수행 체크리스트

- [x] WSL 2 Ubuntu 설치
- [x] Ubuntu 홈 디렉토리 작업 환경 구성
- [x] Docker Desktop 연동
- [x] Docker daemon 권한 문제 해결
- [x] 터미널 기본 조작 실습
- [x] 권한 변경 실습
- [x] hello-world 실행
- [x] Ubuntu 컨테이너 진입 실습
- [x] Docker 운영 명령 확인 (`images`, `ps -a`, `logs`, `stats`)
- [ ] Dockerfile 기반 웹 서버 이미지 작성
- [ ] 포트 매핑 접속 검증
- [ ] 바인드 마운트 반영 검증
- [ ] Docker 볼륨 영속성 검증
- [ ] Git 설정 및 GitHub 연동
- [ ] 트러블슈팅 기록
- [ ] README 최종 정리

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

## 7. 커스텀 웹 서버 이미지

> 진행 후 기록

---

## 8. 포트 매핑 검증

> 진행 후 기록

---

## 9. 바인드 마운트 검증

> 진행 후 기록

---

## 10. 볼륨 영속성 검증

> 진행 후 기록

---

## 11. Git / GitHub / VS Code 연동

> 진행 후 기록

---

## 12. 트러블슈팅

> 진행 후 기록

- [docs/trouble-shooting.md](docs/trouble-shooting.md)

---

## 13. 배운 점

> 진행 후 기록
