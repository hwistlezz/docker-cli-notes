# 💻 Terminal Log

이 파일에는 리눅스 CLI 기본 조작 과정에서 실행한 명령어와 출력 결과를 순서대로 기록합니다.

## 기록 원칙

- 명령어와 출력 결과를 함께 남긴다.
- 필요한 경우 짧은 설명을 덧붙인다.
- README 4번 섹션에는 이 로그의 핵심 부분을 정리해서 반영한다.

---

## 📂 1. 작업 위치 및 사용자 확인

```bash
$ pwd
/home/user/docker-cli-notes

$ ls -la
total 24
drwxr-xr-x 5 user user   4096 Apr  8 19:00 .
drwxr-x--- 6 user user   4096 Apr  8 17:57 ..
drwxr-xr-x 8 user docker 4096 Apr  8 19:23 .git
-rw-r--r-- 1 user docker    0 Apr  8 18:05 Dockerfile
-rw-r--r-- 1 user docker 2544 Apr  8 18:17 README.md
drwxr-xr-x 2 user docker 4096 Apr  8 18:05 app
drwxr-xr-x 3 user docker 4096 Apr  8 18:05 docs
```

### 👉 설명

- 현재 작업 위치가 `/home/user/docker-cli-notes `임을 확인
- 숨김 파일을 포함한 현재 디렉토리 목록을 확인

---

## 📁 2. practice 디렉토리 생성 및 이동

```bash
$ mkdir practice
$ cd practice
$ pwd
/home/user/docker-cli-notes/practice
```

### 👉 설명

- `practice` 디렉토리 생성
- 생성한 디렉토리로 이동한 후, 현재 위치를 확인

---

## 📄 3. 빈 파일 생성 및 파일 내용 확인

```bash
$ touch test.txt

$ ls -la
total 8
drwxr-xr-x 2 user docker 4096 Apr  8 19:56 .
drwxr-xr-x 6 user user   4096 Apr  8 19:56 ..
-rw-r--r-- 1 user docker    0 Apr  8 19:56 test.txt

$ cat test.txt
```

### 👉 설명

- `touch` 명령으로 빈 파일 `test.txt` 를 생성
- `cat` 명령으로 생성 직후 파일 내용이 비어 있음을 확인

---

## 📄 4. 파일에 내용 입력 후 다시 확인

```bash
$ vim test.txt
$ cat test.txt
Hello World!
```

### 👉 설명

- 편집기를 사용해 파일에 `Hello World!` 내용을 입력
- `cat` 명령으로 실제 내용이 저장되었는지 다시 확인

---

## 📄 5. 파일 복사 및 복사본 확인

```bash
$ cp test.txt test-copy.txt

$ ls -la
total 16
drwxr-xr-x 2 user docker 4096 Apr  8 20:01 .
drwxr-xr-x 6 user user   4096 Apr  8 19:56 ..
-rw-r--r-- 1 user docker   13 Apr  8 20:01 test-copy.txt
-rw-r--r-- 1 user docker   13 Apr  8 20:00 test.txt

$ cat test-copy.txt
Hello World!
```

### 👉 설명

- `cp` 명령으로 `test.txt` 를 `test-copy.txt` 로 복사했다
- `cat` 명령으로 복사된 파일에도 동일한 내용이 있는지 확인

---

## 🔄 6. 파일 이름 변경 및 결과 확인

```bash
$ mv test-copy.txt renamed.txt

$ ls -la
total 16
drwxr-xr-x 2 user docker 4096 Apr  8 20:02 .
drwxr-xr-x 6 user user   4096 Apr  8 19:56 ..
-rw-r--r-- 1 user docker   13 Apr  8 20:01 renamed.txt
-rw-r--r-- 1 user docker   13 Apr  8 20:00 test.txt

$ cat renamed.txt
Hello World!
```

### 👉 설명

- `mv` 명령으로 `test-copy.txt` 의 이름을 `renamed.txt` 로 변경
- 이름이 바뀐 뒤에도 파일 내용이 그대로 유지되는지 확인

---

## 🗑️ 7. 파일 삭제 후 확인

```bash
$ rm renamed.txt

$ ls -la
total 12
drwxr-xr-x 2 user docker 4096 Apr  8 20:03 .
drwxr-xr-x 6 user user   4096 Apr  8 19:56 ..
-rw-r--r-- 1 user docker   13 Apr  8 20:00 test.txt
```

### 👉 설명

- `rm` 명령으로 `renamed.txt` 를 삭제
- 삭제 후 목록에서 파일이 사라졌는지 확인

---

## 📁 8. 상위 디렉토리로 이동 후 하위 디렉토리 확인

```bash
$ cd ..
$ ls -la practice
total 12
drwxr-xr-x 2 user docker 4096 Apr  8 20:03 .
drwxr-xr-x 6 user user   4096 Apr  8 19:56 ..
-rw-r--r-- 1 user docker   13 Apr  8 20:00 test.txt
```

### 👉 설명

- `cd ..` 명령으로 상위 디렉토리로 이동 후, `ls -la` 명령으로 `practice` 디렉토리 상태를 다시 확인
- 최종적으로 `test.txt` 파일만 남아 있음을 확인

---

## 📁 9. 추가 복사 실습

```bash
$ cd practice
$ cp test.txt test-copy.txt
$ mv test-copy.txt ~/docker-cli-notes/
$ ls -la
total 12
drwxr-xr-x 2 user docker 4096 Apr  8 20:06 .
drwxr-xr-x 6 user user   4096 Apr  8 20:06 ..
-rw-r--r-- 1 user docker   13 Apr  8 20:00 test.txt

$ cd ..
$ ls -la
total 32
drwxr-xr-x 6 user user   4096 Apr  8 20:06 .
drwxr-x--- 6 user user   4096 Apr  8 20:00 ..
drwxr-xr-x 8 user docker 4096 Apr  8 19:23 .git
-rw-r--r-- 1 user docker    0 Apr  8 18:05 Dockerfile
-rw-r--r-- 1 user docker 2544 Apr  8 18:17 README.md
drwxr-xr-x 2 user docker 4096 Apr  8 18:05 app
drwxr-xr-x 3 user docker 4096 Apr  8 18:05 docs
drwxr-xr-x 2 user docker 4096 Apr  8 20:06 practice
-rw-r--r-- 1 user docker   13 Apr  8 20:05 test-copy.txt
```

### 👉 설명

- 추가로 파일을 상위 디렉토리로 이동시키는 실습도 수행함
- 상대 경로와 홈 디렉토리 절대 경로를 함께 다뤄봄

---
