# 개발 워크스테이션 구축 실습

## 1. 프로젝트 개요

이 프로젝트는 macOS 환경에서 **리눅스 CLI 기초**, **Docker 컨테이너 실습**, **Git/GitHub 연동**을 직접 수행하고, 그 과정을 재현 가능한 형태로 문서화한 결과물이다.

이번 실습의 목표는 단순히 명령어를 한 번 실행해 보는 것이 아니라, 아래 내용을 **직접 수행하고 설명할 수 있는 수준으로 정리하는 것**이다.

- 터미널에서 파일/디렉토리 조작
- 파일 및 디렉토리 권한 확인과 변경
- OrbStack 기반 Docker 실행 환경 점검
- Docker 이미지/컨테이너 운영 명령 실습
- `hello-world`, `ubuntu` 컨테이너 실행
- 기존 베이스 이미지 기반 커스텀 Docker 이미지 제작
- 포트 매핑 후 브라우저 접속 확인
- 바인드 마운트와 Docker Volume 차이 검증
- Git 설정 및 GitHub/VSCode 연동

이 README는 평가자가 저장소 링크만 보고도 **무엇을 수행했는지, 어떤 명령어로 검증했는지, 어떤 문제가 있었고 어떻게 해결했는지**를 이해할 수 있도록 작성했다. :contentReference[oaicite:1]{index=1}

---

## 2. 실행 환경

| 항목 | 내용 |
|---|---|
| OS | macOS [버전 입력] |
| Shell | zsh |
| Terminal | macOS Terminal 또는 iTerm2 |
| Container Runtime | OrbStack |
| Docker Version | [실제 버전 입력] |
| Git Version | [실제 버전 입력] |
| VSCode Version | [실제 버전 입력] |

### 2-1. 환경 확인 명령

```bash
sw_vers
uname -a
echo $SHELL
docker --version
docker info
git --version
code --version
```

### 2-2. 실행 결과

```bash
[여기에 실제 출력 결과 붙여넣기]
```

> **참고**
>
> - 본 실습은 `macOS + OrbStack` 환경에서 진행했다.
> - OrbStack 앱이 실행 중이어야 `docker` 명령이 정상 동작한다.
> - `code --version` 명령이 동작하지 않는 경우, VSCode에서 `Shell Command: Install 'code' command in PATH`를 먼저 적용하면 된다.

---

## 3. 수행 체크리스트

- [ ] 작업 디렉토리 생성 및 이동
- [ ] 현재 위치 확인 및 숨김 파일 포함 목록 확인
- [ ] 파일 생성 / 복사 / 이동 / 이름 변경 / 삭제
- [ ] 파일 내용 확인 및 빈 파일 생성
- [ ] 파일 권한 변경 실습
- [ ] 디렉토리 권한 변경 실습
- [ ] Docker 버전 확인
- [ ] Docker 데몬 동작 확인
- [ ] `docker images` 확인
- [ ] `docker ps` / `docker ps -a` 확인
- [ ] `docker logs` 확인
- [ ] `docker stats` 확인
- [ ] `hello-world` 실행
- [ ] `ubuntu` 컨테이너 내부 진입 및 명령 실행
- [ ] 커스텀 Docker 이미지 빌드
- [ ] 포트 매핑 후 접속 확인
- [ ] 바인드 마운트 반영 확인
- [ ] Docker Volume 생성 및 영속성 검증
- [ ] Git 사용자 정보 및 기본 브랜치 설정
- [ ] GitHub 저장소 생성 및 연결
- [ ] VSCode GitHub 연동 확인
- [ ] 트러블슈팅 2건 이상 기록
- [ ] 민감정보 마스킹 확인

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
    ├── 02-permission-before-after.png
    ├── 03-docker-version-info.png
    ├── 04-hello-world.png
    ├── 05-ubuntu-container.png
    ├── 06-docker-build.png
    ├── 07-port-8080-browser.png
    ├── 08-bind-mount-before.png
    ├── 09-bind-mount-after.png
    ├── 10-volume-before-delete.png
    ├── 11-volume-after-recreate.png
    ├── 12-git-config.png
    └── 13-vscode-github-login.png
```

> 스크린샷 파일명은 예시이며, 실제 제출 시 본인이 저장한 이름에 맞게 수정했다.

---

## 5. 검증 방법

과제 요구사항상 단순 수행이 아니라, **어떤 명령으로 무엇을 검증했는지**가 중요하다. 아래 표는 항목별 검증 기준을 정리한 것이다. :contentReference[oaicite:2]{index=2}

| 검증 항목 | 사용 명령 / 방법 | 확인 포인트 | 증거 위치 |
|---|---|---|---|
| 터미널 기본 조작 | `pwd`, `ls -la`, `mkdir`, `cp`, `mv`, `rm` | 명령어와 출력 결과 | 본문 6장 / `logs/terminal-log.md` |
| 파일 권한 | `ls -l`, `chmod` | 변경 전/후 권한 차이 | 본문 7장 |
| Docker 점검 | `docker --version`, `docker info` | Docker 사용 가능 여부 | 본문 8장 |
| 이미지/컨테이너 운영 | `docker images`, `docker ps`, `docker ps -a`, `docker logs`, `docker stats --no-stream` | 이미지/컨테이너 상태 확인 | 본문 9장 |
| hello-world | `docker run hello-world` | 정상 실행 메시지 | 본문 10장 |
| ubuntu 컨테이너 | `docker run -it ubuntu bash` | 컨테이너 내부 명령 실행 | 본문 11장 |
| 커스텀 이미지 | `docker build`, `docker run` | 빌드 성공, 실행 성공 | 본문 12장 |
| 포트 매핑 | `docker run -p`, 브라우저 접속 또는 `curl` | 호스트 포트로 접근 성공 | 본문 13장 |
| 바인드 마운트 | `-v "$(pwd)/app:/usr/share/nginx/html"` | 호스트 수정 내용이 컨테이너에 반영됨 | 본문 14장 |
| 볼륨 영속성 | `docker volume create`, 컨테이너 삭제 후 재실행 | 데이터 유지 여부 | 본문 15장 |
| Git 설정 | `git config --list` | 사용자 정보 및 기본 브랜치 설정 | 본문 16장 |
| GitHub/VSCode 연동 | 로그인 화면, 저장소 연결 화면 | 연동 완료 증거 | 본문 17장 |

---

## 6. 터미널 기본 조작 로그

### 6-1. 수행 목표

- 현재 위치 확인
- 숨김 파일 포함 목록 확인
- 디렉토리 생성 및 이동
- 파일 생성 / 복사 / 이름 변경 / 삭제
- 파일 내용 확인

### 6-2. 수행 명령

```bash
pwd
ls
ls -la

mkdir -p ~/dev-workstation-practice
cd ~/dev-workstation-practice

touch memo.txt
echo "hello workstation" > memo.txt
cat memo.txt

cp memo.txt memo-copy.txt
mv memo-copy.txt renamed.txt
mkdir sample-dir
mv renamed.txt sample-dir/
ls -la
rm -rf sample-dir
ls -la
```

### 6-3. 실행 결과

```bash
[여기에 실제 출력 결과 붙여넣기]
```

### 6-4. 설명

- `pwd`: 현재 작업 중인 디렉토리의 절대경로를 확인한다.
- `ls -la`: 숨김 파일을 포함한 전체 목록과 권한 정보를 확인한다.
- `mkdir -p`: 중간 디렉토리가 없어도 한 번에 디렉토리를 생성한다.
- `touch`: 빈 파일을 만든다.
- `echo "..." > 파일명`: 파일 내용을 작성한다.
- `cat`: 파일 내용을 확인한다.
- `cp`: 파일을 복사한다.
- `mv`: 파일 이동 또는 이름 변경에 사용한다.
- `rm -rf`: 파일/디렉토리를 삭제한다.

### 6-5. 절대 경로와 상대 경로 설명

- **절대 경로**: 루트(`/`)부터 시작하는 전체 경로다.  
  예: `/Users/[사용자명]/dev-workstation-practice/memo.txt`
- **상대 경로**: 현재 위치를 기준으로 한 경로다.  
  예: `./memo.txt`, `sample-dir/renamed.txt`

---

## 7. 파일 권한 실습

과제 요구사항에 따라 **파일 1개와 디렉토리 1개에 대해 권한 변경 전/후를 비교**했다. :contentReference[oaicite:3]{index=3}

### 7-1. 파일 권한 변경

```bash
touch permission.txt
ls -l permission.txt
chmod 644 permission.txt
ls -l permission.txt
chmod 755 permission.txt
ls -l permission.txt
```

### 7-2. 디렉토리 권한 변경

```bash
mkdir permission-dir
ls -ld permission-dir
chmod 755 permission-dir
ls -ld permission-dir
chmod 700 permission-dir
ls -ld permission-dir
```

### 7-3. 실행 결과

```bash
[여기에 실제 출력 결과 붙여넣기]
```

### 7-4. 권한 해석

- `r`: read, 읽기 권한
- `w`: write, 쓰기 권한
- `x`: execute, 실행 권한

#### 숫자 권한 해석

- `r = 4`
- `w = 2`
- `x = 1`

따라서 다음과 같이 계산한다.

- `7 = 4 + 2 + 1 = rwx`
- `6 = 4 + 2 = rw-`
- `5 = 4 + 1 = r-x`
- `4 = 4 = r--`

#### 예시

- `755 = rwx r-x r-x`
  - 소유자: 읽기, 쓰기, 실행
  - 그룹: 읽기, 실행
  - 기타 사용자: 읽기, 실행
- `644 = rw- r-- r--`
  - 소유자: 읽기, 쓰기
  - 그룹: 읽기
  - 기타 사용자: 읽기

#### 파일과 디렉토리에서의 차이

- 파일의 `x`: 파일 자체를 실행할 수 있는지 여부
- 디렉토리의 `x`: 해당 디렉토리 안으로 들어가거나 내부 항목에 접근할 수 있는지 여부

---

## 8. Docker 설치 및 기본 점검

### 8-1. 점검 명령

```bash
docker --version
docker info
docker context ls
```

### 8-2. 실행 결과

```bash
[여기에 실제 출력 결과 붙여넣기]
```

### 8-3. 설명

- `docker --version`: 설치된 Docker CLI 버전을 확인한다.
- `docker info`: Docker 데몬이 정상 동작 중인지 확인한다.
- `docker context ls`: 현재 연결된 Docker context를 확인한다.

> OrbStack이 실행 중이지 않으면 `docker info`가 실패할 수 있다.

---

## 9. Docker 기본 운영 명령 실습

### 9-1. 수행 명령

```bash
docker pull nginx:alpine
docker images

docker run -d --name sample-nginx nginx:alpine
docker ps
docker logs sample-nginx
docker stats --no-stream
docker stop sample-nginx
docker ps -a
```

### 9-2. 실행 결과

```bash
[여기에 실제 출력 결과 붙여넣기]
```

### 9-3. 설명

- `docker images`: 로컬 이미지 목록 확인
- `docker ps`: 실행 중인 컨테이너 목록 확인
- `docker ps -a`: 종료된 컨테이너까지 포함한 전체 목록 확인
- `docker logs`: 컨테이너 로그 확인
- `docker stats --no-stream`: 현재 리소스 사용량 1회 확인

---

## 10. hello-world 실행

### 10-1. 수행 명령

```bash
docker run hello-world
```

### 10-2. 실행 결과

```bash
[여기에 실제 출력 결과 붙여넣기]
```

### 10-3. 검증 내용

`hello-world` 컨테이너는 Docker가 이미지를 내려받고, 컨테이너를 실행하고, 결과 메시지를 출력할 수 있는지 확인하는 가장 기본적인 테스트다.

---

## 11. Ubuntu 컨테이너 실행 및 내부 명령 수행

### 11-1. 수행 명령

```bash
docker run -it --name ubuntu-test ubuntu bash
```

컨테이너 내부에서 다음 명령을 실행했다.

```bash
ls
echo "inside container"
pwd
exit
```

컨테이너 종료 후 다시 시작하고 내부에 재진입했다.

```bash
docker ps -a
docker start ubuntu-test
docker exec -it ubuntu-test bash
```

### 11-2. 실행 결과

```bash
[여기에 실제 출력 결과 붙여넣기]
```

### 11-3. `attach` 와 `exec` 차이 정리

- `attach`: 실행 중인 메인 프로세스에 직접 연결한다.
- `exec`: 실행 중인 컨테이너 안에서 **새로운 셸이나 명령**을 실행한다.

실무에서는 내부 점검을 위해 보통 `docker exec -it 컨테이너명 bash`를 더 자주 사용한다.

---

## 12. Dockerfile 기반 커스텀 이미지 제작

이번 실습에서는 **웹 서버 베이스 이미지인 `nginx:alpine`** 을 활용해 정적 페이지를 제공하는 커스텀 이미지를 만들었다. 이는 과제에서 요구한 “기존 Dockerfile/이미지 기반의 커스텀 이미지 제작” 조건에 해당한다. :contentReference[oaicite:4]{index=4}

### 12-1. 선택한 베이스 이미지

- 베이스 이미지: `nginx:alpine`
- 선택 이유
  - 가볍고 빠르다.
  - 정적 웹 콘텐츠 배포가 쉽다.
  - 포트 매핑, 바인드 마운트, 볼륨 실습에 적합하다.

### 12-2. 커스텀 포인트

- 사용자 정의 HTML 파일 추가
- 제목과 본문을 실습 전용 페이지로 변경
- 브라우저 접속 확인이 가능하도록 구성

### 12-3. `Dockerfile`

```dockerfile
FROM nginx:alpine

LABEL org.opencontainers.image.title="dev-workstation-web"
LABEL org.opencontainers.image.version="1.0"

COPY app/index.html /usr/share/nginx/html/index.html
```

### 12-4. `app/index.html`

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Dev Workstation</title>
</head>
<body>
  <h1>Docker Custom Image Success</h1>
  <p>이 페이지는 macOS + OrbStack 환경에서 실행한 nginx 커스텀 이미지 결과다.</p>
</body>
</html>
```

### 12-5. 빌드 및 실행 명령

```bash
docker build -t my-web:1.0 .
docker run -d -p 8080:80 --name my-web my-web:1.0
docker ps
```

### 12-6. 실행 결과

```bash
[여기에 실제 출력 결과 붙여넣기]
```

---

## 13. 포트 매핑 및 접속 증거

### 13-1. 수행 명령

```bash
docker run -d -p 8080:80 --name my-web-8080 my-web:1.0
curl http://localhost:8080
```

또는 브라우저에서 다음 주소로 접속했다.

```text
http://localhost:8080
```

### 13-2. 확인 결과

- 호스트의 `8080` 포트 요청이 컨테이너의 `80` 포트로 전달되었다.
- 브라우저 또는 `curl` 응답으로 커스텀 페이지가 정상 출력되었다.

### 13-3. 접속 증거

![포트 매핑 접속 확인](./screenshots/07-port-8080-browser.png)

> 브라우저 캡처 시 주소창과 포트 번호가 함께 보이도록 포함했다.

### 13-4. 포트 매핑이 필요한 이유

컨테이너 내부 서비스는 기본적으로 격리되어 있으므로, macOS 호스트에서 접근하려면 `-p 호스트포트:컨테이너포트` 형태로 연결해야 한다.  
즉, 포트 매핑은 **호스트와 컨테이너를 연결하는 입구를 여는 작업**이다.

---

## 14. 바인드 마운트 반영 검증

### 14-1. 수행 목적

호스트의 파일을 수정했을 때 컨테이너 내부 서비스에도 즉시 반영되는지 확인하기 위해 바인드 마운트를 사용했다.

### 14-2. 수행 명령

```bash
docker run -d -p 8081:80 \
  --name bind-web \
  -v "$(pwd)/app:/usr/share/nginx/html" \
  nginx:alpine
```

그다음 macOS 호스트에서 `app/index.html` 파일 내용을 수정하고 다시 접속했다.

```bash
curl http://localhost:8081
```

### 14-3. 실행 결과

```bash
[여기에 실제 출력 결과 붙여넣기]
```

### 14-4. 증거

#### 변경 전

![바인드 마운트 변경 전](./screenshots/08-bind-mount-before.png)

#### 변경 후

![바인드 마운트 변경 후](./screenshots/09-bind-mount-after.png)

### 14-5. 설명

바인드 마운트는 macOS 호스트의 실제 디렉토리를 컨테이너에 직접 연결하는 방식이다.  
따라서 호스트 파일을 수정하면 컨테이너 안에서도 바로 같은 내용이 보인다.

---

## 15. Docker Volume 영속성 검증

### 15-1. 수행 목적

컨테이너를 삭제해도 데이터가 유지되는지 검증하기 위해 Docker Volume을 사용했다. 이는 과제의 “볼륨 영속성 검증” 요구사항에 해당한다. :contentReference[oaicite:5]{index=5}

### 15-2. 수행 명령

```bash
docker volume create mydata

docker run -d --name vol-test -v mydata:/data ubuntu sleep infinity
docker exec -it vol-test bash -lc "echo hi > /data/hello.txt && cat /data/hello.txt"

docker rm -f vol-test

docker run -d --name vol-test2 -v mydata:/data ubuntu sleep infinity
docker exec -it vol-test2 bash -lc "cat /data/hello.txt"
```

### 15-3. 실행 결과

```bash
[여기에 실제 출력 결과 붙여넣기]
```

### 15-4. 증거

#### 컨테이너 삭제 전

![볼륨 데이터 생성](./screenshots/10-volume-before-delete.png)

#### 컨테이너 재생성 후

![볼륨 데이터 유지 확인](./screenshots/11-volume-after-recreate.png)

### 15-5. 설명

Volume은 컨테이너와 분리된 저장공간이다.  
따라서 컨테이너를 삭제해도 Volume이 유지되는 한 데이터는 남아 있다.

---

## 16. Git 설정 및 GitHub 연동

### 16-1. Git 사용자 정보 설정

```bash
git config --global user.name "[사용자 이름]"
git config --global user.email "[사용자 이메일]"
git config --global init.defaultBranch main
git config --list
```

### 16-2. 실행 결과

```bash
[여기에 실제 출력 결과 붙여넣기]
```

### 16-3. 로컬 저장소 초기화 및 원격 연결

```bash
git init
git add .
git commit -m "docs: initialize dev workstation practice"
git remote add origin https://github.com/[github-id]/[repository-name].git
git branch -M main
git push -u origin main
```

### 16-4. Git과 GitHub의 역할 차이

- **Git**: 내 컴퓨터에서 버전 관리를 수행하는 도구다.
- **GitHub**: Git 저장소를 원격으로 올리고 공유·협업하는 플랫폼이다.

즉, Git은 버전 관리 엔진이고 GitHub는 그 결과물을 원격에서 관리하는 협업 공간이다.

### 16-5. 연동 증거

![git config 결과](./screenshots/12-git-config.png)

![VSCode GitHub 연동](./screenshots/13-vscode-github-login.png)

---

## 17. 트러블슈팅

과제 요구사항에 맞게 최소 2건 이상의 트러블슈팅을 정리했다. :contentReference[oaicite:6]{index=6}

### 트러블슈팅 1. `docker info` 가 동작하지 않음

#### 문제

`docker info` 실행 시 Docker 데몬에 연결할 수 없다는 메시지가 출력되었다.

#### 원인 가설

- OrbStack이 실행 중이지 않음
- Docker context 연결 문제
- 셸 세션이 아직 Docker 엔진과 연결되지 않음

#### 확인

```bash
docker --version
docker info
docker context ls
```

#### 해결

OrbStack 앱을 실행한 뒤 다시 `docker info`를 실행하니 정상 동작했다.

#### 배운 점

Docker CLI가 설치되어 있어도, 실제 Docker 엔진이 실행 중이 아니면 컨테이너를 사용할 수 없다.

---

### 트러블슈팅 2. `http://localhost:8080` 접속 실패

#### 문제

컨테이너는 실행 중인데 브라우저에서 `http://localhost:8080` 접속이 되지 않았다.

#### 원인 가설

- 포트 매핑이 잘못되었을 수 있음
- 컨테이너 내부 서비스 포트와 호스트 포트가 다를 수 있음
- 같은 포트를 다른 컨테이너가 이미 사용 중일 수 있음

#### 확인

```bash
docker ps
docker logs my-web
```

#### 해결

기존 컨테이너를 삭제하고 포트 매핑을 다시 지정하여 실행했다.

```bash
docker rm -f my-web
docker run -d -p 8080:80 --name my-web my-web:1.0
```

#### 배운 점

포트 매핑은 `호스트포트:컨테이너포트` 순서이며, 컨테이너 내부 서비스가 실제로 어떤 포트를 사용하는지 먼저 확인해야 한다.

---

### 트러블슈팅 3. 바인드 마운트 수정 내용이 반영되지 않음

#### 문제

`app/index.html`을 수정했는데 브라우저 내용이 바로 바뀌지 않았다.

#### 원인 가설

- 마운트 경로가 잘못되었을 수 있음
- 브라우저 캐시가 남아 있을 수 있음
- 다른 컨테이너를 보고 있었을 수 있음

#### 확인

```bash
docker inspect bind-web
cat app/index.html
curl http://localhost:8081
```

#### 해결

현재 작업 디렉토리를 다시 확인하고, 정확한 경로로 컨테이너를 재실행했다. 또한 브라우저 대신 `curl`로도 응답을 재검증했다.

#### 배운 점

바인드 마운트는 **호스트 경로와 컨테이너 경로를 정확히 연결하는 것**이 핵심이다.

---

## 18. 보안 및 개인정보 보호

문서 작성 시 아래 내용을 확인했다.

- GitHub 토큰, 비밀번호, 인증 코드, 개인키는 문서와 스크린샷에 포함하지 않았다.
- 이메일, 사용자명 등 개인 정보는 필요한 경우 일부 마스킹했다.
- `git config --list` 결과 캡처 시 민감정보 포함 여부를 다시 확인했다.
- 민감정보가 노출되었다면 문서 수정만이 아니라 필요한 경우 재발급도 진행해야 한다.

---

## 19. 재현 방법

과제 요구사항에 따라 README만 보고도 같은 절차를 따라 할 수 있도록 실행 순서를 정리했다. :contentReference[oaicite:7]{index=7}

### 19-1. 저장소 클론

```bash
git clone https://github.com/[github-id]/[repository-name].git
cd [repository-name]
```

### 19-2. 이미지 빌드

```bash
docker build -t my-web:1.0 .
```

### 19-3. 컨테이너 실행

```bash
docker run -d -p 8080:80 --name my-web my-web:1.0
```

### 19-4. 접속 확인

```bash
curl http://localhost:8080
```

또는 브라우저에서 다음 주소로 접속한다.

```text
http://localhost:8080
```

### 19-5. 바인드 마운트 확인

```bash
docker run -d -p 8081:80 \
  --name bind-web \
  -v "$(pwd)/app:/usr/share/nginx/html" \
  nginx:alpine
```

### 19-6. 볼륨 영속성 확인

```bash
docker volume create mydata
docker run -d --name vol-test -v mydata:/data ubuntu sleep infinity
docker exec -it vol-test bash -lc "echo hi > /data/hello.txt"
docker rm -f vol-test
docker run -d --name vol-test2 -v mydata:/data ubuntu sleep infinity
docker exec -it vol-test2 bash -lc "cat /data/hello.txt"
```

---

## 20. 최종 정리

이번 실습을 통해 다음 내용을 직접 수행하고 설명할 수 있게 되었다.

- 절대 경로와 상대 경로의 차이
- 파일/디렉토리 권한과 `755`, `644` 해석 방식
- Docker 이미지와 컨테이너의 차이
- 커스텀 Docker 이미지 제작 과정
- 포트 매핑이 필요한 이유
- 바인드 마운트와 Volume의 차이
- Docker Volume을 통한 데이터 영속성
- Git과 GitHub의 역할 차이
- 재현 가능한 개발 환경을 README로 문서화하는 방법
