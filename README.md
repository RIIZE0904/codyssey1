

````markdown
# Codyssey 1 실습 보고서

## 1. 프로젝트 개요

본 문서는 리눅스 CLI, Docker, Git/GitHub, VSCode 연동까지의 기초 개발 환경을 직접 실습한 내용을 정리한 README이다.  
단순히 명령어를 따라 치는 것에 그치지 않고, 각 명령어와 개념의 의미를 이해하고 설명할 수 있도록 실습 과정과 결과를 함께 기록하였다.

이번 실습은 다음과 같은 흐름으로 진행하였다.

**CLI 기초 → Docker 기본 → Docker 심화 → Git/GitHub → 문서화**

이 순서로 진행한 이유는, 개발자가 실제로 작업할 때 운영체제 기본 조작 능력 위에 컨테이너 실행 능력이 쌓이고, 그 위에 버전 관리와 문서화가 이어지기 때문이다.

---

## 2. 실습 환경

- OS: Windows / macOS / Linux
- Terminal: Git Bash / PowerShell / Linux Terminal
- Editor: Visual Studio Code
- Docker: Docker Desktop
- Git: Git
- GitHub: 원격 저장소 사용

---

## 3. 전체 체크리스트

### 3-1. 전체 학습/실습 순서 체크리스트

- [x] 터미널 기본기
- [x] 경로
- [x] 파일/폴더 조작
- [x] 내용 확인
- [x] 숨김 파일 보기
- [x] 파일 권한
- [x] r / w / x 의미
- [x] 파일과 디렉토리에서 권한 차이
- [x] 755, 644 해석
- [x] Docker 기본기
- [x] 이미지와 컨테이너 차이
- [x] Docker 실행 확인
- [x] hello-world 실행
- [x] ubuntu 진입 및 종료
- [x] Docker 운영 명령
- [x] images 확인
- [x] ps / ps -a 확인
- [x] logs 확인
- [x] stats 확인
- [x] rm / stop 실행
- [x] Dockerfile 커스텀 이미지
- [x] 베이스 이미지 선택
- [x] Dockerfile 작성
- [x] build 실행
- [x] run 실행
- [x] 포트 매핑 접속 확인
- [x] 마운트와 볼륨
- [x] 바인드 마운트
- [x] Docker volume
- [x] 삭제 후 데이터 유지 확인
- [x] Git / GitHub / VSCode 연동
- [x] git config
- [x] 저장소 초기화
- [x] commit
- [x] remote 연결
- [x] push
- [x] VSCode 연동 증거 확인
- [x] README 정리
- [x] 환경 작성
- [x] 수행 로그 작성
- [x] 검증 결과 작성
- [x] 트러블슈팅 작성
- [x] 설명용 정리 작성

### 3-2. 최종 검증 기준 체크리스트

- [x] 작업 디렉토리를 생성하고 이동할 수 있다.
- [x] 파일 생성, 복사, 이동, 삭제를 할 수 있다.
- [x] 절대경로와 상대경로를 구분하여 설명할 수 있다.
- [x] `ls -l` 결과를 보고 파일 권한을 해석할 수 있다.
- [x] `755`와 `644`가 왜 그런 숫자인지 설명할 수 있다.
- [x] Docker 이미지와 컨테이너의 차이를 설명할 수 있다.
- [x] `docker run hello-world`로 Docker 동작을 확인할 수 있다.
- [x] Ubuntu 컨테이너에 진입하고 종료할 수 있다.
- [x] `docker images`, `docker ps`, `docker ps -a`, `docker logs`, `docker stats`를 사용할 수 있다.
- [x] Dockerfile을 작성하여 커스텀 이미지를 만들 수 있다.
- [x] 포트 매핑 후 브라우저에서 접속 확인을 할 수 있다.
- [x] 바인드 마운트와 볼륨의 차이를 설명할 수 있다.
- [x] 볼륨 사용 시 컨테이너 삭제 후에도 데이터가 유지되는 것을 확인할 수 있다.
- [x] Git 저장소를 초기화하고 add / commit / push 흐름을 수행할 수 있다.
- [x] GitHub 원격 저장소와 연결할 수 있다.
- [x] VSCode에서 저장소를 열고 작업할 수 있다.

---

## 4. 리눅스 CLI 기초 실습

### 4-1. 학습 목표

- 경로 개념 이해
- 파일 시스템 구조 이해
- 파일 및 디렉토리 조작
- 숨김 파일 확인
- 파일 권한 확인 및 변경
- 절대경로 / 상대경로 구분

### 4-2. 실습 명령어

```bash
pwd
mkdir -p ~/codyssey1/linux-cli
cd ~/codyssey1/linux-cli

touch practice.txt
mkdir sample_dir

ls
ls -al

echo "hello codyssey" > practice.txt
cat practice.txt

cp practice.txt practice_copy.txt
mv practice_copy.txt moved.txt

ls -al

chmod 644 practice.txt
ls -l practice.txt

chmod 755 practice.txt
ls -l practice.txt
````

### 4-3. 절대경로 / 상대경로 확인

```bash
pwd
cat practice.txt
cat ./practice.txt
cat ~/codyssey1/linux-cli/practice.txt
```

### 4-4. 삭제 실습

```bash
rm moved.txt
rm practice.txt
rmdir sample_dir
ls -al
```

### 4-5. 개념 정리

#### 1) 절대경로와 상대경로

* **절대경로**: 루트(`/`) 또는 홈(`~`)부터 시작하는 전체 경로
* **상대경로**: 현재 위치를 기준으로 표현하는 경로

예시:

* 절대경로: `~/codyssey1/linux-cli/practice.txt`
* 상대경로: `./practice.txt`

#### 2) 파일 권한: r / w / x

* `r` = read = 읽기
* `w` = write = 쓰기
* `x` = execute = 실행

#### 3) 파일과 디렉토리에서 권한 의미 차이

| 권한 | 파일에서 의미         | 디렉토리에서 의미         |
| -- | --------------- | ----------------- |
| r  | 파일 내용을 읽을 수 있음  | 내부 목록을 볼 수 있음     |
| w  | 파일 내용을 수정할 수 있음 | 내부 파일 생성/삭제 가능    |
| x  | 파일을 실행할 수 있음    | 디렉토리 안으로 들어갈 수 있음 |

#### 4) 755 / 644 해석

숫자 권한은 다음 규칙으로 계산한다.

* `r = 4`
* `w = 2`
* `x = 1`

따라서,

* `7 = 4 + 2 + 1 = rwx`
* `6 = 4 + 2 = rw-`
* `5 = 4 + 1 = r-x`
* `4 = r--`

즉,

* `755 = rwxr-xr-x`
* `644 = rw-r--r--`

### 4-6. CLI 실습 결과

* 작업 디렉토리 생성 완료
* 파일 생성 / 복사 / 이동 / 삭제 완료
* `ls -al`로 숨김 파일 포함 목록 확인 완료
* `chmod 644`, `chmod 755` 권한 변경 확인 완료
* 절대경로 / 상대경로 접근 확인 완료

---

## 5. Docker 기본 실습

### 5-1. 학습 목표

* Docker가 정상 설치되었는지 확인
* 이미지와 컨테이너 차이 이해
* hello-world 실행
* Ubuntu 컨테이너 진입 및 종료
* 주요 운영 명령어 사용

### 5-2. Docker 동작 확인

```bash
docker --version
docker run hello-world
docker images
docker ps
docker ps -a
```

### 5-3. Ubuntu 컨테이너 실행 및 종료

```bash
docker run -it --name ubuntu-test ubuntu bash
pwd
ls
exit
```

### 5-4. 로그 및 삭제 확인

```bash
docker ps -a
docker logs ubuntu-test
docker rm ubuntu-test
docker ps -a
```

### 5-5. 개념 정리

#### 1) 이미지(Image)

이미지는 컨테이너를 만들기 위한 **설계도**이다.
실행에 필요한 파일, 라이브러리, 환경설정이 포함된 읽기 전용 템플릿이다.

#### 2) 컨테이너(Container)

컨테이너는 이미지를 실제로 실행한 **동작 중인 인스턴스**이다.
즉, 이미지는 틀이고, 컨테이너는 그 틀로 실제 실행된 결과물이다.

### 5-6. Docker 실습 결과

* Docker 버전 확인 완료
* `hello-world` 실행 성공
* `docker images`로 이미지 확인 완료
* `docker ps`, `docker ps -a` 차이 확인 완료
* Ubuntu 컨테이너 진입 및 종료 완료
* `docker logs` 확인 완료

---

## 6. Docker 운영 명령 실습

### 6-1. Nginx 컨테이너 실행

```bash
docker run -d --name web-test -p 8080:80 nginx
docker ps
docker logs web-test
docker stats web-test
```

브라우저 접속:

```text
http://localhost:8080
```

### 6-2. 중지 및 삭제

```bash
docker stop web-test
docker rm web-test
docker ps -a
```

### 6-3. 개념 정리

* `-d`: 백그라운드 실행
* `--name`: 컨테이너 이름 지정
* `-p 8080:80`: 호스트 8080 포트를 컨테이너 80 포트와 연결
* `docker stats`: CPU, 메모리 등 자원 사용량 확인

### 6-4. 운영 명령 실습 결과

* Nginx 컨테이너 실행 완료
* 포트 매핑 완료
* 브라우저 접속 확인 완료
* `logs`, `stats`, `stop`, `rm` 실행 완료

---

## 7. Dockerfile 커스텀 이미지 실습

### 7-1. 실습 목표

* 베이스 이미지를 선택할 수 있다.
* Dockerfile을 작성할 수 있다.
* 커스텀 이미지를 빌드할 수 있다.
* 생성한 이미지를 컨테이너로 실행할 수 있다.

### 7-2. 실습 폴더 생성

```bash
mkdir -p ~/codyssey1/dockerfile-practice
cd ~/codyssey1/dockerfile-practice
```

### 7-3. HTML 파일 생성

```bash
echo '<h1>Hello Codyssey</h1><p>Custom Docker Image Test</p>' > index.html
cat index.html
```

### 7-4. Dockerfile 작성

```bash
printf 'FROM nginx:alpine\nCOPY index.html /usr/share/nginx/html/index.html\n' > Dockerfile
cat Dockerfile
```

### 7-5. 이미지 빌드 및 실행

```bash
docker build -t codyssey-web .
docker run -d --name codyssey-web-container -p 8081:80 codyssey-web
docker ps
```

브라우저 접속:

```text
http://localhost:8081
```

### 7-6. 종료 및 삭제

```bash
docker stop codyssey-web-container
docker rm codyssey-web-container
```

### 7-7. 개념 정리

#### Dockerfile이란?

Dockerfile은 이미지를 만들기 위한 **설명서**이다.
어떤 베이스 이미지를 사용할지, 어떤 파일을 복사할지, 어떤 명령을 실행할지 등을 작성한다.

#### 베이스 이미지란?

직접 모든 환경을 처음부터 만들지 않고, 기존에 만들어진 기본 이미지를 출발점으로 사용하는 것이다.

예시:

* `nginx:alpine`
* `ubuntu`
* `python:3.11`

### 7-8. Dockerfile 실습 결과

* `FROM nginx:alpine` 사용 완료
* `COPY`를 통해 HTML 파일 복사 완료
* `docker build` 성공
* `docker run` 성공
* 포트 매핑 후 웹 페이지 접속 성공

---

## 8. 바인드 마운트(bind mount) 실습

### 8-1. 실습 목표

* 호스트 파일과 컨테이너 파일이 연결되는 방식을 이해한다.
* 호스트에서 수정한 파일이 컨테이너에 반영되는 것을 확인한다.

### 8-2. 실습 명령어

```bash
mkdir -p ~/codyssey1/bind-mount-test
cd ~/codyssey1/bind-mount-test

echo '<h1>Bind Mount Test</h1>' > index.html

docker run -d --name bind-web -p 8082:80 -v "$(pwd):/usr/share/nginx/html" nginx:alpine
docker ps
```

브라우저 접속:

```text
http://localhost:8082
```

### 8-3. 파일 수정 후 반영 확인

```bash
echo '<h1>Bind Mount Changed</h1>' > index.html
cat index.html
```

브라우저 새로고침 후 변경 여부 확인

### 8-4. 종료 및 삭제

```bash
docker stop bind-web
docker rm bind-web
```

### 8-5. 개념 정리

#### 바인드 마운트란?

호스트 컴퓨터의 실제 폴더를 컨테이너 내부 경로와 직접 연결하는 방식이다.
즉, 내 컴퓨터의 파일을 컨테이너 안에서도 그대로 사용하는 것이다.

#### 장점

* 파일 수정 결과를 바로 확인할 수 있다.
* 개발 중 빠른 테스트가 가능하다.

#### 주의점

* 호스트의 실제 파일이 직접 연결되므로 실수로 수정/삭제하면 바로 반영된다.

### 8-6. 바인드 마운트 실습 결과

* 호스트 폴더 연결 성공
* 컨테이너 내부 웹 페이지에 호스트 파일 반영 확인
* 수정 내용 즉시 반영 확인 완료

---

## 9. Docker Volume 실습

### 9-1. 실습 목표

* 볼륨을 생성하고 사용할 수 있다.
* 컨테이너를 삭제해도 데이터가 유지되는지 확인할 수 있다.
* 바인드 마운트와 볼륨의 차이를 설명할 수 있다.

### 9-2. 볼륨 생성 및 사용

```bash
docker volume create my-volume
docker volume ls

docker run -it --name volume-test -v my-volume:/data ubuntu bash
```

컨테이너 내부에서:

```bash
echo "volume saved data" > /data/test.txt
cat /data/test.txt
exit
```

### 9-3. 컨테이너 삭제 후 데이터 유지 확인

```bash
docker rm volume-test
docker run -it --name volume-test-2 -v my-volume:/data ubuntu bash
```

컨테이너 내부에서:

```bash
cat /data/test.txt
exit
```

### 9-4. 정리

```bash
docker rm volume-test-2
docker volume ls
```

### 9-5. 개념 정리

#### Volume이란?

Volume은 Docker가 관리하는 별도 저장공간이다.
컨테이너가 삭제되어도 데이터가 유지될 수 있다.

#### 바인드 마운트와 볼륨 차이

| 항목     | 바인드 마운트       | 볼륨                 |
| ------ | ------------- | ------------------ |
| 저장 위치  | 호스트의 실제 경로    | Docker가 관리하는 저장 공간 |
| 사용 목적  | 개발 중 빠른 파일 반영 | 안정적인 데이터 저장        |
| 데이터 관리 | 사용자가 직접 관리    | Docker가 관리         |

### 9-6. Volume 실습 결과

* Volume 생성 완료
* 컨테이너 내부 파일 저장 완료
* 기존 컨테이너 삭제 후 새 컨테이너에서 동일 데이터 확인 완료
* 데이터 유지 확인 성공

---

## 10. Git / GitHub / VSCode 연동 실습

### 10-1. 실습 목표

* Git 사용자 정보를 설정할 수 있다.
* 로컬 저장소를 초기화할 수 있다.
* 파일을 add / commit 할 수 있다.
* GitHub 원격 저장소와 연결할 수 있다.
* push를 수행할 수 있다.
* VSCode에서 저장소를 열 수 있다.

### 10-2. Git 설정 및 저장소 초기화

```bash
mkdir -p ~/codyssey1/git-practice
cd ~/codyssey1/git-practice

git config --global user.name "본인이름"
git config --global user.email "본인이메일@example.com"

git init
git status
```

### 10-3. README 파일 생성 및 첫 커밋

```bash
echo "# Codyssey 1 Practice" > README.md
git status

git add README.md
git status

git commit -m "docs: first commit"
```

### 10-4. 브랜치명 변경 및 원격 저장소 연결

```bash
git branch -M main
git remote add origin https://github.com/본인아이디/저장소이름.git
git remote -v
```

### 10-5. GitHub로 push

```bash
git push -u origin main
```

### 10-6. VSCode에서 열기

```bash
code .
```

### 10-7. 개념 정리

#### Git의 3가지 영역

1. **Working Directory**
   실제로 내가 작업 중인 파일들이 있는 공간

2. **Staging Area**
   다음 commit에 포함할 파일을 올려두는 공간

3. **Repository**
   commit 기록이 저장되는 공간

#### 기본 흐름

```text
파일 수정 → git add → git commit → git push
```

### 10-8. Git 실습 결과

* Git 사용자 정보 설정 완료
* 로컬 저장소 초기화 완료
* 파일 staging 및 commit 완료
* GitHub 원격 저장소 연결 완료
* push 완료
* VSCode 연동 확인 완료

---

## 11. 수행 로그 요약

| 단계         | 수행 내용                           | 결과 |
| ---------- | ------------------------------- | -- |
| CLI        | 파일/폴더 생성, 이동, 삭제, 권한 변경         | 성공 |
| Docker 기본  | hello-world, ubuntu 컨테이너 실행     | 성공 |
| Docker 운영  | logs, stats, stop, rm           | 성공 |
| Dockerfile | 커스텀 이미지 빌드 및 실행                 | 성공 |
| 마운트        | bind mount 실습                   | 성공 |
| 볼륨         | volume 생성 및 데이터 유지 확인           | 성공 |
| Git        | init, add, commit, remote, push | 성공 |
| VSCode     | 저장소 열기 및 확인                     | 성공 |

---

## 12. 검증 결과

이번 실습을 통해 다음 사항을 직접 확인하였다.

* 터미널에서 경로 이동과 파일 조작이 가능함
* 절대경로와 상대경로의 차이를 설명할 수 있음
* `r`, `w`, `x`의 의미와 `755`, `644`의 숫자 해석이 가능함
* Docker 이미지와 컨테이너의 차이를 설명할 수 있음
* `hello-world`와 Ubuntu 컨테이너 실행이 가능함
* `docker images`, `docker ps`, `docker ps -a`, `docker logs`, `docker stats`를 사용할 수 있음
* Dockerfile 작성 및 빌드가 가능함
* 포트 매핑 후 웹 페이지 접속을 확인함
* 바인드 마운트와 볼륨의 차이를 확인함
* 볼륨 사용 시 데이터 유지가 가능함
* Git 로컬 저장소 초기화부터 GitHub push까지 전체 흐름을 수행함
* VSCode에서 프로젝트를 열어 개발 환경과 연동함

---

## 13. 트러블슈팅

### 13-1. Docker 명령어가 실행되지 않는 경우

**문제**
`docker: command not found` 또는 Docker 실행 불가

**원인**
Docker Desktop 미설치 또는 실행되지 않음

**해결**

* Docker Desktop 설치
* Docker Desktop 실행 확인
* 터미널 재시작

### 13-2. 포트 충돌이 발생하는 경우

**문제**
`Bind for 0.0.0.0:8080 failed`

**원인**
이미 다른 프로그램이 같은 포트를 사용 중임

**해결**

```bash
docker run -d --name web-test -p 8081:80 nginx
```

### 13-3. Git push 실패

**문제**
원격 저장소 push 실패

**원인**

* 원격 저장소 주소 오류
* GitHub 인증 문제
* 저장소 미생성

**해결**

```bash
git remote -v
git remote remove origin
git remote add origin https://github.com/본인아이디/저장소이름.git
git push -u origin main
```

---
