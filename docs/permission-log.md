# 🔐 Permission Log

이 파일에는 파일과 디렉토리의 권한 변경 과정을 기록합니다.

## 기록 원칙

- 명령어와 출력 결과를 함께 남긴다.
- 파일 1개, 디렉토리 1개를 대상으로 변경 전/후를 비교한다.
- README 5번 섹션에는 핵심 결과만 요약해서 반영한다.

---

## 🔍 1. 변경 전 권한 확인

대상:

- 📄 파일: `app/index.html`
- 📂 디렉토리: `app/`

```bash
$ ls -l app/index.html
-rw-r--r-- 1 user docker 0 Apr  8 18:05 app/index.html

$ ls -ld app
drwxr-xr-x 2 user docker 4096 Apr  8 18:05 app
```

### 👉 설명

- 📄 `app/index.html `파일의 초기 권한은 `644`
- 📂 `app/` 디렉토리의 초기 권한은 `755`

---

## 🔒 2. 임시로 제한적인 권한으로 변경

```bash
$ ls -l app/index.html
-rw------- 1 user docker 0 Apr  8 18:05 app/index.html

$ ls -ld app
drwx------ 2 user docker 4096 Apr  8 18:05 app
```

### 👉 설명

- 파일 권한이 `600` 으로 변경됨을 확인
- 디렉토리 권한이 `700` 으로 변경됨을 확인

---

## ✔️ 3. 변경 후 권한 확인

```bash
$ ls -l app/index.html
-rw------- 1 user docker 0 Apr  8 18:05 app/index.html

$ ls -ld app
drwx------ 2 user docker 4096 Apr  8 18:05 app
```

### 👉 설명

- 파일 권한이 `600`으로 반영되었음을 확인
- 디렉토리 권한이 `700` 으로 변경됨을 확인

---

## 🔓 4. 최종 권한으로 복구

```bash
$ chmod 644 app/index.html
$ chmod 755 app
```

### 👉 설명

- 파일 권한을 최종 목표 값인 `644` 로 복구
- 디렉토리 권한이 `700` 으로 변경됨을 확인

---

## 🔍 5. 최종 권한 확인

```bash
$ ls -l app/index.html
-rw-r--r-- 1 user docker 0 Apr  8 18:05 app/index.html

$ ls -ld app
drwxr-xr-x 2 user docker 4096 Apr  8 18:05 app
```

### 👉 설명

- 파일 권한이 다시 `644` 로 복구되었음을 확인
- 디렉토리 권한이 다시 `755` 로 복구되었음을 확인

---

## 6. 권한 숫자 해석

- `r = 4`
- `w = 2`
- `x = 1`

### 예시

- `644 = rw- r-- r--`
- `755 = rwx r-x r-x`

### 의미 정리

- `644` 파일: 소유자는 읽기/쓰기 가능, 나머지는 읽기만 가능
- `755` 디렉토리: 소유자는 읽기/쓰기/실행 가능, 나머지는 읽기/실행 가능

---
