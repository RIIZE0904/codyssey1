# Codyssey Mission 1 - 개발 워크스테이션 구축 기록

## 한눈에 보기

이 문서는 macOS 환경에서 터미널, Docker, Git/GitHub를 이용해 개발 워크스테이션을 구성하고 검증한 과정을 정리한 기록이다.  
이번 실습에서는 단순히 명령어를 실행하는 데서 끝나지 않고, **각 도구가 왜 필요한지**, **어떤 방식으로 동작하는지**, **어떤 결과를 통해 검증했는지**까지 확인하는 데 초점을 두었다.

### 이번 실습에서 직접 확인한 것

- 터미널에서 파일과 디렉토리를 생성, 이동, 복사, 삭제할 수 있는지
- 파일과 디렉토리의 권한이 어떻게 다르게 동작하는지
- OrbStack 환경에서 Docker가 정상적으로 동작하는지
- 이미지와 컨테이너를 구분해서 설명할 수 있는지
- 포트 매핑을 통해 호스트에서 컨테이너 웹 서버로 접근할 수 있는지
- 바인드 마운트로 호스트 변경사항이 즉시 반영되는지
- Docker Volume으로 컨테이너 삭제 후에도 데이터가 유지되는지
- Git과 GitHub의 역할 차이를 설명할 수 있는지

---

## 1. 실습 환경

| 항목 | 내용 |
|---|---|
| OS | macOS [버전 입력] |
| Shell | zsh |
| Terminal | macOS Terminal / iTerm2 |
| Docker Runtime | OrbStack |
| Docker Version | [실제 버전 입력] |
| Git Version | [실제 버전 입력] |
| VSCode Version | [실제 버전 입력] |

### 환경 확인 명령

```bash
sw_vers
echo $SHELL
docker --version
docker info
git --version
code --version
```

### 실행 결과

```bash
[실제 출력 붙여넣기]
```

> OrbStack이 실행 중이어야 `docker info`가 정상 출력된다.

---

## 2. 수행 체크리스트

- [ ] 작업 디렉토리 생성 및 이동
- [ ] 현재 위치 및 숨김 파일 포함 목록 확인
- [ ] 파일 생성 / 내용 작성 / 내용 확인
- [ ] 파일 복사 / 이동 / 이름 변경 / 삭제
- [ ] 디렉토리 생성 / 이동 / 이름 변경 / 삭제
- [ ] 파일 권한 변경 실습
- [ ] 디렉토리 권한 변경 실습
- [ ] Docker 설치 및 동작 여부 확인
- [ ] Docker 이미지 다운로드 및 목록 확인
- [ ] 컨테이너 실행 / 중지 / 전체 목록 확인
- [ ] `docker logs`, `docker stats --no-stream` 실행
- [ ] `hello-world` 실행
- [ ] `ubuntu` 컨테이너 내부 진입 및 명령 실행
- [ ] `attach` 와 `exec` 차이 확인
- [ ] Dockerfile 기반 커스텀 이미지 제작
- [ ] 포트 매핑 후 접속 확인
- [ ] 바인드 마운트 실습
- [ ] Docker Volume 영속성 검증
- [ ] Git 사용자 정보 및 기본 브랜치 설정
- [ ] GitHub 저장소 연동 확인
- [ ] VSCode GitHub 연동 증거 첨부
- [ ] 트러블슈팅 2건 이상 정리
- [ ] 민감정보 마스킹 확인

---

## 3. 프로젝트 구조

```text
.
├── README.md
├── Dockerfile
├── .gitignore
├── app
│   └── index.html
├── logs
│   ├── terminal-log.md
│   └── docker-log.md
└── screenshots
    ├── 01-terminal-basic.png
    ├── 02-permission-test.png
    ├── 03-docker-info.png
    ├── 04-hello-world.png
    ├── 05-ubuntu-container.png
    ├── 06-custom-image-build.png
    ├── 07-port-8080-browser.png
    ├── 08-bind-mount-before.png
    ├── 09-bind-mount-after.png
    ├── 10-volume-before-delete.png
    ├── 11-volume-after-recreate.png
    ├── 12-git-config.png
    └── 13-vscode-github.png
```

---

## 4. 검증 기준

| 검증 대상 | 확인 방법 | 핵심 확인 포인트 |
|---|---|---|
| 터미널 기본 조작 | `pwd`, `ls -la`, `touch`, `cp`, `mv`, `rm` | 파일/디렉토리 생명주기 조작 가능 여부 |
| 권한 | `ls -l`, `chmod` | 파일과 디렉토리에서 권한 차이 확인 |
| Docker 설치 | `docker --version`, `docker info` | CLI 및 Docker 엔진 사용 가능 여부 |
| Docker 운영 | `docker images`, `docker ps -a`, `docker logs`, `docker stats --no-stream` | 이미지/컨테이너 상태와 리소스 사용량 확인 |
| 컨테이너 기본 실습 | `docker run hello-world`, `docker run -it ubuntu bash` | Docker 실행 구조와 컨테이너 내부 진입 확인 |
| 커스텀 이미지 | `docker build`, `docker run` | 기존 이미지 기반 커스텀 이미지 생성 성공 여부 |
| 포트 매핑 | `curl http://localhost:8080` 또는 브라우저 접속 | 호스트에서 컨테이너 접근 가능 여부 |
| 바인드 마운트 | `-v "$(pwd)/app:/usr/share/nginx/html"` | 호스트 파일 변경이 실시간 반영되는지 |
| 볼륨 영속성 | `docker volume create`, 컨테이너 삭제 후 재실행 | 데이터 유지 여부 |
| Git/GitHub | `git config --list`, `git remote -v` | 로컬 Git 설정 및 원격 저장소 연결 여부 |

---

## 5. 터미널 기본 조작

이번 실습에서 먼저 확인한 것은 **현재 위치를 알고, 파일과 디렉토리를 직접 관리할 수 있는지**였다.  
개발자는 터미널 안에서 작업 환경을 만들고 정리해야 하므로, 이 단계는 전체 실습의 출발점이 된다.

### 5-1. 현재 위치 및 목록 확인

```bash
pwd
ls -la
```

```bash
[실제 출력 붙여넣기]
```

### 5-2. 작업 디렉토리 생성 및 이동

```bash
mkdir -p ~/dev-workstation-practice
cd ~/dev-workstation-practice
pwd
```

```bash
[실제 출력 붙여넣기]
```

### 5-3. 파일 생성 / 내용 작성 / 내용 확인

```bash
touch memo.txt
cat memo.txt
echo "hello workstation" > memo.txt
cat memo.txt
```

```bash
[실제 출력 붙여넣기]
```

### 5-4. 파일 복사 / 이동 / 이름 변경

```bash
cp memo.txt copy.txt
mv copy.txt renamed.txt
ls
```

```bash
[실제 출력 붙여넣기]
```

### 5-5. 디렉토리 생성 / 이동 / 삭제

```bash
mkdir sample-dir
mv renamed.txt sample-dir
ls sample-dir
rm -rf sample-dir
ls
```

```bash
[실제 출력 붙여넣기]
```

### 5-6. 절대 경로와 상대 경로

- **절대 경로**: 루트(`/`)부터 시작하는 전체 경로  
  예: `/Users/[사용자명]/dev-workstation-practice/memo.txt`
- **상대 경로**: 현재 위치를 기준으로 표현하는 경로  
  예: `./memo.txt`, `sample-dir/renamed.txt`

절대 경로는 어디서 실행하든 같은 위치를 가리키고, 상대 경로는 현재 위치가 기준이 되기 때문에 내부 구조를 다룰 때 더 간단하다.

---

## 6. 권한 실습

권한 실습에서는 **파일의 실행 권한**과 **디렉토리의 접근 권한**이 다르게 동작한다는 점을 확인했다.

### 6-1. 파일 권한 확인 및 변경

```bash
echo 'echo hello' > script.sh
ls -l script.sh
./script.sh
chmod 744 script.sh
ls -l script.sh
./script.sh
```

```bash
[실제 출력 붙여넣기]
```

### 6-2. 디렉토리 권한 확인 및 변경

```bash
mkdir permission-dir
touch permission-dir/test.txt
ls -ld permission-dir
chmod 655 permission-dir
cd permission-dir
```

```bash
[실제 출력 붙여넣기]
```

### 6-3. 권한 숫자 규칙

- `r = 4`
- `w = 2`
- `x = 1`

따라서

- `7 = rwx`
- `6 = rw-`
- `5 = r-x`
- `4 = r--`

예를 들어

- `744 = rwx r-- r--`
- `755 = rwx r-x r-x`
- `644 = rw- r-- r--`

### 6-4. 파일과 디렉토리의 차이

- 파일에서 `x`는 **실행 가능 여부**
- 디렉토리에서 `x`는 **해당 디렉토리 안으로 들어가거나 내부에 접근 가능한지 여부**

즉, 파일과 디렉토리는 같은 권한 문자(r/w/x)를 사용해도 의미가 완전히 같지는 않다.

---

## 7. Docker 설치 및 기본 점검

macOS에서는 직접 Docker 데몬을 다루기보다 OrbStack을 통해 Docker 엔진을 실행했다.  
이 단계에서는 Docker CLI가 설치되어 있는지, 그리고 실제로 엔진과 통신 가능한지 확인했다.

### 7-1. 점검 명령

```bash
docker --version
docker info
docker context ls
```

```bash
[실제 출력 붙여넣기]
```

### 7-2. 확인한 점

- `docker --version` 으로 Docker CLI 버전을 확인했다.
- `docker info` 에서 Client / Server 정보가 모두 출력되는지 확인했다.
- `Context` 가 `orbstack` 으로 연결되어 있는지 확인했다.

---

## 8. Docker 기본 운영 명령

### 8-1. 이미지 다운로드 및 목록 확인

```bash
docker pull nginx:latest
docker images
```

```bash
[실제 출력 붙여넣기]
```

### 8-2. 컨테이너 실행 / 중지 / 목록 확인

```bash
docker run -d --name web-test1 -p 8080:80 nginx
docker ps
docker stop web-test1
docker ps -a
```

```bash
[실제 출력 붙여넣기]
```

### 8-3. 로그 및 리소스 사용량 확인

```bash
docker logs web-test1
docker stats --no-stream
```

```bash
[실제 출력 붙여넣기]
```

### 8-4. 이미지와 컨테이너 차이

- **이미지(Image)**: 실행을 위한 설계도, 변경되지 않는 기준 파일
- **컨테이너(Container)**: 이미지를 실제로 실행한 인스턴스, 실행 중 상태와 변경사항을 가질 수 있음

즉, 이미지는 “틀”이고 컨테이너는 “그 틀을 실제로 실행한 결과”라고 볼 수 있다.

---

## 9. 컨테이너 실행 실습

### 9-1. hello-world 실행

```bash
docker run hello-world
```

```bash
[실제 출력 붙여넣기]
```

`hello-world` 는 Docker가 이미지를 내려받고, 컨테이너를 만들고, 실행 결과를 터미널에 출력할 수 있는지 확인하는 가장 기본적인 테스트다.

### 9-2. ubuntu 컨테이너 진입

```bash
docker run -it ubuntu bash
```

컨테이너 내부에서 다음 명령을 실행했다.

```bash
ls
touch test.txt
echo "hello" > test.txt
cat test.txt
pwd
exit
```

```bash
[실제 출력 붙여넣기]
```

### 9-3. attach 와 exec 차이

#### exec

```bash
docker run -d -it --name ubuntu-test1 ubuntu
docker exec -it ubuntu-test1 /bin/bash
```

#### attach

```bash
docker attach ubuntu-test1
```

### 9-4. 차이 정리

- `exec`: 실행 중인 컨테이너 안에 **새로운 프로세스**로 들어감
- `attach`: 현재 실행 중인 컨테이너의 **메인 프로세스에 직접 연결**됨

따라서 `bash` 가 메인 프로세스인 컨테이너에서 `attach` 후 `exit` 하면, 메인 프로세스가 종료되면서 컨테이너도 꺼질 수 있다.

---

## 10. Dockerfile 기반 커스텀 이미지 제작

이번 실습에서는 `nginx` 이미지를 베이스로 사용해 정적 HTML 파일을 보여주는 웹 서버 이미지를 만들었다.

### 10-1. 선택한 베이스 이미지

- 베이스 이미지: `nginx:latest`
- 선택 이유
  - 웹 서버 동작 확인이 쉽다.
  - 포트 매핑 실습에 적합하다.
  - 정적 파일 교체만으로 커스텀 결과를 쉽게 확인할 수 있다.

### 10-2. Dockerfile

```dockerfile
FROM nginx:latest
COPY app/index.html /usr/share/nginx/html/index.html
```

### 10-3. HTML 파일

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Mission 1 Custom Web</title>
</head>
<body>
  <h1>기존 Docker 이미지 기반 커스텀 웹 서버</h1>
  <p>macOS + OrbStack 환경에서 실행한 결과다.</p>
</body>
</html>
```

### 10-4. 빌드 및 실행

```bash
docker build -t my-web .
docker images
docker run -d -p 8080:80 --name my-web-app my-web
```

```bash
[실제 출력 붙여넣기]
```

### 10-5. 접속 결과

```bash
curl http://localhost:8080
```

```bash
[실제 출력 붙여넣기]
```

![커스텀 이미지 실행 화면](./screenshots/07-port-8080-browser.png)

---

## 11. 바인드 마운트 실습

바인드 마운트는 **호스트 파일을 컨테이너 내부 경로에 직접 연결**하는 방식이다.  
이번 실습에서는 호스트의 `index.html` 을 수정하면 컨테이너 웹 화면도 바로 바뀌는지 확인했다.

### 11-1. 실행 명령

```bash
docker run -d -p 8081:80 \
  --name bind-test \
  -v "$(pwd)/app/index.html:/usr/share/nginx/html/index.html" \
  my-web
```

### 11-2. 변경 전 확인

```bash
curl http://localhost:8081
```

```bash
[실제 출력 붙여넣기]
```

![변경 전 화면](./screenshots/08-bind-mount-before.png)

### 11-3. 호스트 파일 수정 후 재확인

```bash
echo "bind mount success" > app/index.html
curl http://localhost:8081
```

```bash
[실제 출력 붙여넣기]
```

![변경 후 화면](./screenshots/09-bind-mount-after.png)

### 11-4. 확인한 점

이미지를 다시 빌드하지 않아도, 호스트 파일을 수정하자 컨테이너 내부 웹 페이지도 즉시 바뀌었다.  
즉, 바인드 마운트는 “개발 중 빠른 반영”에 유리한 방식임을 확인했다.

---

## 12. Docker Volume 영속성 검증

컨테이너는 기본적으로 휘발성이므로, 컨테이너 내부에 저장한 데이터는 컨테이너 삭제 시 사라질 수 있다.  
이를 해결하기 위해 Docker Volume을 사용해 데이터가 컨테이너 밖에 유지되는지 검증했다.

### 12-1. 볼륨 생성

```bash
docker volume create my-data
```

### 12-2. 볼륨 연결 후 데이터 생성

```bash
docker run -d --name volume-test1 -v my-data:/data nginx
docker exec volume-test1 sh -c "echo 'volume data' > /data/test.txt"
docker exec volume-test1 cat /data/test.txt
```

```bash
[실제 출력 붙여넣기]
```

### 12-3. 컨테이너 삭제 후 새 컨테이너에서 확인

```bash
docker stop volume-test1
docker rm volume-test1
docker run -d --name volume-test2 -v my-data:/data nginx
docker exec volume-test2 cat /data/test.txt
```

```bash
[실제 출력 붙여넣기]
```

![삭제 전 데이터 생성](./screenshots/10-volume-before-delete.png)
![재생성 후 데이터 유지](./screenshots/11-volume-after-recreate.png)

### 12-4. 확인한 점

컨테이너는 삭제되었지만, `my-data` 볼륨은 남아 있었고 새 컨테이너에서도 이전 데이터가 그대로 확인되었다.  
따라서 Volume은 컨테이너 생명주기와 분리된 저장공간이라는 점을 직접 검증했다.

---

## 13. Git 설정 및 GitHub 연동

### 13-1. Git 설정

```bash
git config --global init.defaultBranch main
git config --global user.name "[사용자명]"
git config --global user.email "[이메일]"
git config --list
```

```bash
[실제 출력 붙여넣기]
```

### 13-2. 원격 저장소 연결 확인

```bash
git remote -v
```

```bash
[실제 출력 붙여넣기]
```

### 13-3. Git과 GitHub 차이

- **Git**: 로컬에서 소스 코드 변경 이력을 관리하는 도구
- **GitHub**: Git 저장소를 원격으로 올리고 협업하는 플랫폼

즉, Git은 버전 관리 자체이고 GitHub는 그 기록을 공유하는 온라인 저장소라고 정리할 수 있다.

### 13-4. 연동 증거

![Git 설정 확인](./screenshots/12-git-config.png)
![VSCode GitHub 연동](./screenshots/13-vscode-github.png)

---

## 14. 트러블슈팅

### 14-1. ubuntu 컨테이너가 바로 종료되는 문제

#### 문제
`docker run -d --name ubuntu-test1 ubuntu` 실행 후 컨테이너가 곧바로 종료되었다.

#### 원인
Ubuntu 이미지는 기본적으로 `/bin/bash` 를 실행하는데, 백그라운드 모드에서 입력을 받을 수 있는 터미널이 없으면 메인 프로세스가 바로 끝난다. Docker 컨테이너는 메인 프로세스가 종료되면 컨테이너도 종료된다.

#### 해결

```bash
docker run -d -it --name ubuntu-test1 ubuntu
docker ps -a
```

#### 배운 점
백그라운드에서 셸을 유지하려면 `-it` 옵션이 함께 필요하다.

---

### 14-2. 포트 충돌 후 컨테이너 이름 중복이 발생한 문제

#### 문제
이미 `8080` 포트를 사용 중인 상태에서 새 컨테이너를 실행하려 했고, 이후 포트를 바꿔 다시 실행했는데도 이름 충돌이 발생했다.

#### 원인
포트 충돌로 컨테이너 실행은 실패했지만, 생성 단계에서 컨테이너 객체가 남아 이름을 점유하고 있었다.

#### 확인

```bash
docker ps -a
```

#### 해결

```bash
docker rm bind-test
docker run -d -p 8081:80 --name bind-test \
  -v "$(pwd)/app/index.html:/usr/share/nginx/html/index.html" \
  my-web
```

#### 배운 점
`docker run` 실패 후에도 컨테이너 껍데기가 남을 수 있으므로, 문제가 생기면 `docker ps -a` 로 전체 목록을 먼저 확인하는 습관이 중요하다.

---

## 15. 재현 방법

같은 결과를 다시 확인하려면 아래 순서로 진행하면 된다.

### 15-1. 저장소 클론

```bash
git clone https://github.com/[github-id]/[repository-name].git
cd [repository-name]
```

### 15-2. 커스텀 이미지 빌드

```bash
docker build -t my-web .
```

### 15-3. 컨테이너 실행

```bash
docker run -d -p 8080:80 --name my-web-app my-web
```

### 15-4. 접속 확인

```bash
curl http://localhost:8080
```

### 15-5. 바인드 마운트 확인

```bash
docker run -d -p 8081:80 \
  --name bind-test \
  -v "$(pwd)/app/index.html:/usr/share/nginx/html/index.html" \
  my-web
```

### 15-6. 볼륨 영속성 확인

```bash
docker volume create my-data
docker run -d --name volume-test1 -v my-data:/data nginx
docker exec volume-test1 sh -c "echo 'volume data' > /data/test.txt"
docker rm -f volume-test1
docker run -d --name volume-test2 -v my-data:/data nginx
docker exec volume-test2 cat /data/test.txt
```

---

## 16. 최종 정리

이번 실습을 통해 아래 내용을 직접 수행하고 설명할 수 있게 되었다.

- 절대 경로와 상대 경로의 차이
- 파일과 디렉토리 권한의 차이
- `755`, `744`, `644` 같은 숫자 권한 해석 방식
- Docker 이미지와 컨테이너의 차이
- 포트 매핑이 필요한 이유
- 바인드 마운트와 Volume의 차이
- 컨테이너는 휘발적이지만 Volume은 영속적이라는 점
- Git과 GitHub의 역할 차이
- README만으로 실습 결과를 재현 가능하게 정리하는 방법
