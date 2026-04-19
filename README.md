# Codyssey Mission 1 - 개발 워크스테이션 구축 기록

## 1. 실습 개요

이 문서는 macOS 환경에서 터미널, Docker, Git/GitHub를 이용해 개발 워크스테이션을 구성하고 검증한 과정을 정리한 기록이다.  
이번 실습에서는 단순히 명령어를 실행하는 데서 끝나지 않고, 각 도구가 실제로 왜 필요한지, 어떤 방식으로 동작하는지, 어떤 결과를 통해 정상 동작을 확인했는지까지 검증하는 데 초점을 두었다.

이번 실습에서 직접 확인한 핵심 내용은 다음과 같다.

- 터미널에서 파일과 디렉토리를 생성, 이동, 복사, 삭제할 수 있는지
- 파일과 디렉토리의 권한이 서로 다르게 동작하는지
- OrbStack 환경에서 Docker CLI와 Docker Engine이 정상적으로 연결되는지
- 이미지와 컨테이너를 구분해서 설명할 수 있는지
- 포트 매핑을 통해 호스트에서 컨테이너 웹 서버로 접근할 수 있는지
- 바인드 마운트로 호스트 변경사항이 즉시 반영되는지
- Docker Volume으로 컨테이너 삭제 후에도 데이터가 유지되는지
- Git과 GitHub의 역할 차이를 설명할 수 있는지

---

## 2. 실습 환경

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

```text
[실제 출력 붙여넣기]
```

정상 동작 기준은 다음과 같다.

- `sw_vers`에서 현재 macOS 버전이 출력된다.
- `echo $SHELL`에서 현재 셸이 `zsh`로 확인된다.
- `docker --version`에서 Docker CLI 버전이 출력된다.
- `docker info`에서 Client와 Server 정보가 모두 출력된다.
- `git --version`에서 Git 버전이 출력된다.
- `code --version`에서 VSCode CLI가 정상 동작한다.

---

## 3. 평가 항목별 검증 요약

| 평가 항목 | 실행 명령 | 확인 결과 | 의미 |
|---|---|---|---|
| 터미널 기본 조작 | `mkdir`, `touch`, `cp`, `mv`, `rm` | 파일/디렉토리 생성·이동·삭제 확인 | 기본 CLI 조작 가능 |
| 파일 권한 | `ls -l`, `chmod 744` | 실행 권한 추가 전후 차이 확인 | 파일에서 x 권한 의미 이해 |
| 디렉토리 권한 | `chmod 655` 후 `cd` 시도 | 접근 실패 확인 | 디렉토리에서 x 권한 의미 이해 |
| Docker 동작 확인 | `docker --version`, `docker info`, `docker context ls` | CLI/Engine 정상 응답 | Docker 사용 가능 |
| 이미지/컨테이너 실습 | `docker pull`, `docker run`, `docker ps -a` | 이미지 다운로드 및 컨테이너 실행 확인 | 이미지와 컨테이너 차이 이해 |
| hello-world 실행 | `docker run hello-world` | 테스트 메시지 출력 | Docker 실행 구조 확인 |
| Ubuntu 컨테이너 진입 | `docker run -it ubuntu bash` | 컨테이너 내부 명령 수행 가능 | 독립 실행 환경 확인 |
| 커스텀 이미지 | `docker build`, `docker run` | 커스텀 웹 서버 실행 성공 | Dockerfile 기반 이미지 제작 가능 |
| 포트 매핑 | `-p 8080:80`, `curl http://localhost:8080` | 호스트에서 접속 성공 | 호스트-컨테이너 연결 확인 |
| 바인드 마운트 | `-v "$(pwd)/app/index.html:..."` | 파일 수정 즉시 반영 | 개발 중 빠른 반영 가능 |
| 볼륨 영속성 | `docker volume create`, 컨테이너 재생성 | 데이터 유지 확인 | 컨테이너와 분리된 저장공간 이해 |
| Git/GitHub | `git config --list`, `git remote -v` | 로컬 설정 및 원격 연결 확인 | 버전 관리와 원격 저장소 역할 구분 가능 |

---

## 4. 프로젝트 구조

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

## 5. 터미널 기본 조작

이번 실습에서 먼저 확인한 것은 현재 위치를 알고, 파일과 디렉토리를 직접 관리할 수 있는지였다.  
개발자는 터미널 안에서 작업 환경을 만들고 정리해야 하므로, 이 단계는 전체 실습의 출발점이 된다.

### 5-1. 현재 위치 및 목록 확인

#### 목적
현재 작업 위치를 확인하고, 숨김 파일을 포함한 전체 목록을 볼 수 있는지 확인한다.

#### 실행 명령

```bash
pwd
ls -la
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
`pwd`는 현재 작업 디렉토리의 절대 경로를 보여준다.  
`ls -la`는 일반 파일뿐 아니라 숨김 파일까지 포함해 목록을 확인할 수 있게 해준다.

---

### 5-2. 작업 디렉토리 생성 및 이동

#### 목적
새로운 작업 공간을 만들고, 해당 경로로 정상적으로 이동할 수 있는지 확인한다.

#### 실행 명령

```bash
mkdir -p ~/dev-workstation-practice
cd ~/dev-workstation-practice
pwd
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
`mkdir -p`를 사용하면 중간 디렉토리가 없어도 한 번에 생성할 수 있다.  
이후 `cd`와 `pwd`를 통해 실제로 원하는 작업 디렉토리로 이동했는지 확인했다.

---

### 5-3. 파일 생성 / 내용 작성 / 내용 확인

#### 목적
파일을 생성하고, 내용을 기록하고, 다시 확인하는 기본 흐름을 검증한다.

#### 실행 명령

```bash
touch memo.txt
cat memo.txt
echo "hello workstation" > memo.txt
cat memo.txt
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
처음 생성한 빈 파일은 내용이 없고, `echo ... > 파일명`을 사용하면 내용을 기록할 수 있다.  
`cat`을 통해 실제 파일 내용이 저장되었는지 바로 검증했다.

---

### 5-4. 파일 복사 / 이동 / 이름 변경

#### 목적
파일의 복사, 이동, 이름 변경이 정상적으로 가능한지 확인한다.

#### 실행 명령

```bash
cp memo.txt copy.txt
mv copy.txt renamed.txt
ls
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
`cp`는 파일 복사, `mv`는 파일 이동 또는 이름 변경에 사용된다.  
이번 실습에서는 복사본을 만든 뒤 이름을 바꾸는 흐름으로 동작을 확인했다.

---

### 5-5. 디렉토리 생성 / 이동 / 삭제

#### 목적
디렉토리를 만들고 내부로 파일을 이동한 뒤, 삭제까지 가능한지 확인한다.

#### 실행 명령

```bash
mkdir sample-dir
mv renamed.txt sample-dir
ls sample-dir
rm -rf sample-dir
ls
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
디렉토리는 파일과 별도로 생성·이동·삭제할 수 있다.  
`rm -rf`는 디렉토리와 내부 내용을 함께 삭제하므로 주의해서 사용해야 한다.

---

### 5-6. 절대 경로와 상대 경로

절대 경로는 루트(`/`)부터 시작하는 전체 경로이고, 상대 경로는 현재 작업 위치를 기준으로 한다.

예시:

- 절대 경로: `/Users/[사용자명]/dev-workstation-practice/memo.txt`
- 상대 경로: `./memo.txt`, `sample-dir/renamed.txt`

이번 실습에서는 두 방식 모두를 사용해 경로 기준이 어떻게 달라지는지 확인했다.

---

## 6. 파일 및 디렉토리 권한 검증

권한 실습에서는 파일의 실행 권한과 디렉토리의 접근 권한이 다르게 동작한다는 점을 직접 확인했다.

### 6-1. 파일 권한 확인 및 변경

#### 목적
실행 권한이 없는 파일은 바로 실행되지 않으며, `chmod`를 통해 권한을 바꾸면 실행 가능해지는지 확인한다.

#### 실행 명령

```bash
echo 'echo hello' > script.sh
ls -l script.sh
./script.sh
chmod 744 script.sh
ls -l script.sh
./script.sh
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
처음에는 실행 권한이 없어 `Permission denied`가 발생할 수 있다.  
이후 `chmod 744`를 적용하면 소유자에게 실행 권한이 추가되어 스크립트가 정상 실행된다.  
이를 통해 파일에서 `x` 권한은 **실행 가능 여부**를 의미한다는 점을 확인했다.

---

### 6-2. 디렉토리 권한 확인 및 변경

#### 목적
디렉토리에서 `x` 권한이 실행이 아니라 접근 가능 여부에 해당한다는 점을 확인한다.

#### 실행 명령

```bash
mkdir permission-dir
touch permission-dir/test.txt
ls -ld permission-dir
chmod 655 permission-dir
cd permission-dir
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
디렉토리에서 `x` 권한이 없으면 내부로 들어가는 동작(`cd`)이 제한된다.  
즉, 디렉토리의 `x`는 파일처럼 실행이 아니라 **해당 디렉토리 안으로 접근할 수 있는지**를 결정한다.

---

### 6-3. 권한 숫자 규칙

권한 숫자는 다음 값을 더해서 표현한다.

- `r = 4`
- `w = 2`
- `x = 1`

예를 들어:

- `7 = rwx`
- `6 = rw-`
- `5 = r-x`
- `4 = r--`

대표적인 예시는 다음과 같다.

- `744 = rwx r-- r--`
- `755 = rwx r-x r-x`
- `644 = rw- r-- r--`

이번 실습에서는 숫자 권한이 실제 권한 문자열로 어떻게 반영되는지 `ls -l`로 함께 확인했다.

---

## 7. Docker 동작 확인

macOS에서는 직접 Linux 커널 기반 Docker Engine을 다루기보다 OrbStack을 통해 Docker 실행 환경을 구성했다.  
이 단계에서는 Docker CLI가 설치되어 있는지, 그리고 실제로 엔진과 통신 가능한지 확인했다.

### 7-1. 점검 명령

#### 목적
Docker CLI, Engine, Context가 모두 정상적으로 연결되어 있는지 확인한다.

#### 실행 명령

```bash
docker --version
docker info
docker context ls
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
`docker --version`은 CLI 설치 여부와 버전을 확인하는 명령이다.  
`docker info`는 Docker Engine과의 통신 여부를 확인하는 핵심 명령이다.  
`docker context ls`에서는 현재 컨텍스트가 `orbstack`으로 연결되어 있는지 확인했다.

---

## 8. 이미지와 컨테이너 실행 실습

이번 단계에서는 Docker 이미지 다운로드, 컨테이너 실행, 상태 확인, 기본 예제 실행을 통해 이미지와 컨테이너의 차이를 실제로 확인했다.

### 8-1. 이미지 다운로드 및 목록 확인

#### 목적
로컬 환경에 이미지가 정상적으로 다운로드되는지 확인한다.

#### 실행 명령

```bash
docker pull nginx:latest
docker images
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
`docker pull`은 레지스트리에서 이미지를 로컬로 내려받는 명령이다.  
`docker images`를 통해 로컬에 저장된 이미지 목록을 확인했다.

---

### 8-2. 컨테이너 실행 / 중지 / 목록 확인

#### 목적
이미지를 기반으로 컨테이너를 생성하고, 실행 상태와 종료 상태를 확인한다.

#### 실행 명령

```bash
docker run -d --name web-test1 -p 8080:80 nginx
docker ps
docker stop web-test1
docker ps -a
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
`docker run`은 이미지를 기반으로 컨테이너를 생성하고 실행한다.  
`docker ps`는 현재 실행 중인 컨테이너, `docker ps -a`는 종료된 컨테이너까지 포함한 전체 목록을 보여준다.

---

### 8-3. 로그 및 리소스 사용량 확인

#### 목적
실행 중이거나 종료된 컨테이너의 로그와 자원 사용량을 확인한다.

#### 실행 명령

```bash
docker logs web-test1
docker stats --no-stream
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
`docker logs`는 컨테이너 실행 중 발생한 로그를 확인하는 데 사용한다.  
`docker stats --no-stream`은 CPU, 메모리 등 리소스 사용량을 1회성으로 확인할 수 있다.

---

### 8-4. 이미지와 컨테이너 차이

- **이미지(Image)**: 실행을 위한 설계도, 변경되지 않는 기준 파일
- **컨테이너(Container)**: 이미지를 실제로 실행한 인스턴스, 실행 중 상태와 변경사항을 가질 수 있음

즉, 이미지는 **실행 전 템플릿**, 컨테이너는 **그 이미지를 실제로 실행한 결과물**이라고 정리할 수 있다.

---

## 9. 컨테이너 기본 실습

### 9-1. hello-world 실행

#### 목적
Docker가 이미지를 내려받고, 컨테이너를 만들고, 실행 결과를 출력하는 전체 흐름이 정상 동작하는지 확인한다.

#### 실행 명령

```bash
docker run hello-world
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
`hello-world`는 Docker 설치와 실행이 정상 동작하는지 검증하는 가장 기본적인 테스트다.  
이 명령을 통해 이미지 다운로드, 컨테이너 생성, 실행, 출력까지의 흐름을 한 번에 확인할 수 있다.

---

### 9-2. ubuntu 컨테이너 내부 진입

#### 목적
컨테이너 내부에 들어가 독립된 환경에서 직접 명령어를 실행할 수 있는지 확인한다.

#### 실행 명령

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

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
`-it` 옵션을 사용하면 컨테이너 내부 쉘과 상호작용할 수 있다.  
이번 실습에서는 Ubuntu 컨테이너 안에서 파일을 만들고 내용을 작성해, 컨테이너가 호스트와 분리된 별도 환경이라는 점을 확인했다.

---

### 9-3. attach 와 exec 차이 확인

#### 실행 명령

```bash
docker run -d -it --name ubuntu-test1 ubuntu
docker exec -it ubuntu-test1 /bin/bash
docker attach ubuntu-test1
```

#### 해석
- `exec`: 실행 중인 컨테이너 안에 **새로운 프로세스**로 들어간다.
- `attach`: 실행 중인 컨테이너의 **메인 프로세스에 직접 연결**된다.

따라서 `bash`가 메인 프로세스인 컨테이너에서 `attach` 후 `exit` 하면, 메인 프로세스가 종료되면서 컨테이너도 함께 종료될 수 있다.

---

## 10. Dockerfile 기반 커스텀 웹 서버 구축

이번 실습에서는 `nginx` 이미지를 베이스로 사용해 정적 HTML 파일을 보여주는 웹 서버 이미지를 만들었다.

### 10-1. 베이스 이미지 선택

- 베이스 이미지: `nginx:latest`

선택 이유는 다음과 같다.

- 웹 서버 동작 확인이 쉽다.
- 포트 매핑 실습에 적합하다.
- 정적 파일 교체만으로 커스텀 결과를 쉽게 확인할 수 있다.

---

### 10-2. Dockerfile

```dockerfile
FROM nginx:latest
COPY app/index.html /usr/share/nginx/html/index.html
```

---

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

---

### 10-4. 빌드 및 실행

#### 목적
Dockerfile을 기반으로 새 이미지를 만들고, 실제 컨테이너로 실행할 수 있는지 확인한다.

#### 실행 명령

```bash
docker build -t my-web .
docker images
docker run -d -p 8080:80 --name my-web-app my-web
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
`docker build`는 현재 디렉토리의 Dockerfile을 기준으로 이미지를 생성한다.  
생성한 `my-web` 이미지를 실행한 뒤, 포트 매핑을 통해 웹 서버에 접근할 수 있도록 구성했다.

---

### 10-5. 접속 결과

#### 실행 명령

```bash
curl http://localhost:8080
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
호스트의 `8080` 포트와 컨테이너 내부의 `80` 포트를 연결했기 때문에, `localhost:8080`으로 접속할 수 있다.  
이를 통해 포트 매핑이 정상적으로 동작하는지 확인했다.

---

## 11. 바인드 마운트와 Volume 검증

### 11-1. 바인드 마운트 실습

바인드 마운트는 호스트 파일을 컨테이너 내부 경로에 직접 연결하는 방식이다.  
이번 실습에서는 호스트의 `index.html`을 수정하면 컨테이너 웹 화면도 바로 바뀌는지 확인했다.

#### 목적
이미지를 다시 빌드하지 않아도 호스트 파일 변경이 컨테이너에 바로 반영되는지 검증한다.

#### 실행 명령

```bash
docker run -d -p 8081:80 \
  --name bind-test \
  -v "$(pwd)/app/index.html:/usr/share/nginx/html/index.html" \
  my-web
```

#### 변경 전 확인

```bash
curl http://localhost:8081
```

```text
[실제 출력 붙여넣기]
```

#### 호스트 파일 수정 후 재확인

```bash
echo "bind mount success" > app/index.html
curl http://localhost:8081
```

```text
[실제 출력 붙여넣기]
```

#### 해석
이미지를 다시 빌드하지 않아도, 호스트 파일을 수정하자 컨테이너 내부 웹 페이지도 즉시 바뀌었다.  
이를 통해 바인드 마운트는 **개발 중 빠른 반영**에 유리한 방식임을 확인했다.

---

### 11-2. Docker Volume 영속성 검증

컨테이너는 기본적으로 휘발성이므로, 컨테이너 내부 데이터는 삭제 시 사라질 수 있다.  
이번 실습에서는 Docker Volume을 사용해 데이터가 컨테이너 밖에 유지되는지 검증했다.

#### 목적
컨테이너를 삭제한 뒤에도 데이터가 유지되는지 확인해 Volume의 영속성을 검증한다.

#### 실행 명령

```bash
docker volume create my-data
docker run -d --name volume-test1 -v my-data:/data nginx
docker exec volume-test1 sh -c "echo 'volume data' > /data/test.txt"
docker exec volume-test1 cat /data/test.txt
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 컨테이너 삭제 후 재확인

```bash
docker stop volume-test1
docker rm volume-test1
docker run -d --name volume-test2 -v my-data:/data nginx
docker exec volume-test2 cat /data/test.txt
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
컨테이너는 삭제되었지만 `my-data` 볼륨은 유지되었고, 새 컨테이너에서도 이전 데이터가 그대로 확인되었다.  
이를 통해 Volume은 **컨테이너 생명주기와 분리된 저장공간**이라는 점을 직접 검증했다.

---

## 12. Git 설정 및 GitHub 연동

### 12-1. Git 설정

#### 목적
로컬 Git 환경에서 사용자 정보와 기본 브랜치 설정이 정상 반영되는지 확인한다.

#### 실행 명령

```bash
git config --global init.defaultBranch main
git config --global user.name "[사용자명]"
git config --global user.email "[이메일]"
git config --list
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
Git은 로컬 환경에서 버전 관리를 담당하므로, 사용자 정보와 기본 브랜치 설정이 먼저 필요하다.  
`git config --list`를 통해 설정이 실제로 반영되었는지 확인했다.

---

### 12-2. 원격 저장소 연결 확인

#### 목적
현재 로컬 저장소가 어떤 원격 저장소와 연결되어 있는지 확인한다.

#### 실행 명령

```bash
git remote -v
```

#### 실행 결과

```text
[실제 출력 붙여넣기]
```

#### 해석
`git remote -v`는 연결된 원격 저장소의 fetch/push 주소를 보여준다.  
이를 통해 GitHub 저장소가 정상적으로 연결되어 있는지 확인할 수 있다.

---

### 12-3. Git과 GitHub 차이

- **Git**: 로컬에서 소스 코드 변경 이력을 관리하는 버전 관리 도구
- **GitHub**: Git 저장소를 원격으로 올리고 공유·협업하는 플랫폼

즉, Git은 **버전 관리 자체**, GitHub는 **그 기록을 원격에서 저장하고 협업하는 공간**이라고 정리할 수 있다.

---

### 12-4. 연동 증거

- Git 설정 확인 화면 첨부
- VSCode GitHub 연동 화면 첨부

---

## 13. 트러블슈팅

### 13-1. Ubuntu 컨테이너가 바로 종료되는 문제

#### 문제
`docker run -d --name ubuntu-test1 ubuntu` 실행 후 컨테이너가 곧바로 종료되었다.

#### 원인
Ubuntu 이미지는 기본적으로 셸을 실행하지만, 백그라운드 모드에서 입력을 받을 수 있는 터미널이 없으면 메인 프로세스가 바로 종료된다.  
Docker 컨테이너는 메인 프로세스가 종료되면 함께 종료된다.

#### 확인 명령

```bash
docker ps -a
```

#### 해결 방법

```bash
docker run -d -it --name ubuntu-test1 ubuntu
docker ps -a
```

#### 배운 점
백그라운드에서 셸을 유지하려면 단순히 `-d`만이 아니라 `-it`까지 함께 고려해야 한다.  
컨테이너는 결국 **메인 프로세스가 살아 있어야 유지된다**는 점을 이해하게 되었다.

---

### 13-2. 포트 충돌 후 컨테이너 이름 중복이 발생한 문제

#### 문제
이미 `8080` 포트를 사용 중인 상태에서 새 컨테이너를 실행하려 했고, 이후 포트를 바꿔 다시 실행했는데도 이름 충돌이 발생했다.

#### 원인
포트 충돌로 실행은 실패했지만, 생성 단계에서 컨테이너 객체가 남아 이름을 점유하고 있었다.

#### 확인 명령

```bash
docker ps -a
```

#### 해결 방법

```bash
docker rm bind-test
docker run -d -p 8081:80 --name bind-test \
  -v "$(pwd)/app/index.html:/usr/share/nginx/html/index.html" \
  my-web
```

#### 배운 점
`docker run`이 실패한 뒤에도 컨테이너 껍데기가 남을 수 있다.  
문제가 생기면 먼저 `docker ps -a`로 전체 목록을 확인하는 습관이 중요하다.

---

## 14. 재현 방법

같은 결과를 다시 확인하려면 아래 순서로 진행하면 된다.

### 14-1. 저장소 클론

```bash
git clone https://github.com/[github-id]/[repository-name].git
cd [repository-name]
```

### 14-2. 커스텀 이미지 빌드

```bash
docker build -t my-web .
```

### 14-3. 컨테이너 실행

```bash
docker run -d -p 8080:80 --name my-web-app my-web
```

### 14-4. 접속 확인

```bash
curl http://localhost:8080
```

### 14-5. 바인드 마운트 확인

```bash
docker run -d -p 8081:80 \
  --name bind-test \
  -v "$(pwd)/app/index.html:/usr/share/nginx/html/index.html" \
  my-web
```

### 14-6. 볼륨 영속성 확인

```bash
docker volume create my-data
docker run -d --name volume-test1 -v my-data:/data nginx
docker exec volume-test1 sh -c "echo 'volume data' > /data/test.txt"
docker rm -f volume-test1
docker run -d --name volume-test2 -v my-data:/data nginx
docker exec volume-test2 cat /data/test.txt
```

---

## 15. 최종 정리

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

이번 기록은 단순한 명령어 나열이 아니라, 실제 개발 환경을 구성하고 검증한 과정을 재현 가능하게 남기는 데 목적이 있다.  
따라서 각 실습 단계는 **무엇을 했는지**, **왜 했는지**, **어떤 결과로 확인했는지**가 드러나도록 구성했다.

---

## 16. 부록

### 16-1. 스크린샷 목록

- `screenshots/01-terminal-basic.png`
- `screenshots/02-permission-test.png`
- `screenshots/03-docker-info.png`
- `screenshots/04-hello-world.png`
- `screenshots/05-ubuntu-container.png`
- `screenshots/06-custom-image-build.png`
- `screenshots/07-port-8080-browser.png`
- `screenshots/08-bind-mount-before.png`
- `screenshots/09-bind-mount-after.png`
- `screenshots/10-volume-before-delete.png`
- `screenshots/11-volume-after-recreate.png`
- `screenshots/12-git-config.png`
- `screenshots/13-vscode-github.png`

### 16-2. 민감정보 마스킹 항목

제출 전 아래 정보는 반드시 마스킹한다.

- 사용자명
- 이메일 주소
- GitHub 저장소 주소 일부
- 로컬 경로 중 개인 식별 정보
- 토큰, 키, 인증 정보
