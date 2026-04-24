# Codyssey Mission 1 - 개발 워크스테이션 구축 기록

> 이 문서는 macOS + OrbStack 환경에서 터미널, Docker, Git/GitHub를 직접 세팅하고  
> 동작을 검증한 과정을 재현 가능하게 정리한 기록이다.

---

## 목차

1. [실습 환경](#1-실습-환경)
2. [수행 체크리스트](#2-수행-체크리스트)
3. [프로젝트 구조](#3-프로젝트-구조)
4. [평가 항목별 검증 요약](#4-평가-항목별-검증-요약)
5. [터미널 기본 조작](#5-터미널-기본-조작)
6. [파일 및 디렉토리 권한 검증](#6-파일-및-디렉토리-권한-검증)
7. [Docker 동작 확인](#7-docker-동작-확인)
8. [Docker 기본 운영 명령](#8-docker-기본-운영-명령)
9. [컨테이너 기본 실습](#9-컨테이너-기본-실습)
10. [Dockerfile 기반 커스텀 웹 서버 구축](#10-dockerfile-기반-커스텀-웹-서버-구축)
11. [바인드 마운트와 Volume 검증](#11-바인드-마운트와-volume-검증)
12. [Git 설정 및 GitHub 연동](#12-git-설정-및-github-연동)
13. [트러블슈팅](#13-트러블슈팅)
14. [재현 방법](#14-재현-방법)
15. [심층 질문 대비 정리](#15-심층-질문-대비-정리)

---

## 1. 실습 환경

| 항목 | 내용 |
|------|------|
| OS | macOS [15.7.4] |
| Shell | zsh |
| Terminal | macOS Terminal / iTerm2 |
| Docker Runtime | OrbStack |
| Docker Version | 28.5.2 |
| Git Version | 2.53.0 |
| VSCode Version | 1.112.0 |

**환경 확인 명령**

```bash
sw_vers
echo $SHELL
docker --version
docker info
git --version
code --version
```

**실행 결과**

```
tangled01257696@c5r4s1 ~ % sw_vers
ProductName:		macOS
ProductVersion:		15.7.4
BuildVersion:		24G517
tangled01257696@c5r4s1 ~ % echo $SHELL
/bin/zsh
tangled01257696@c5r4s1 ~ % docker --version
Docker version 28.5.2, build ecc6942
tangled01257696@c5r4s1 ~ % docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/tangled01257696/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/tangled01257696/.docker/cli-plugins/docker-compose

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 28.5.2
 Storage Driver: overlay2
  Backing Filesystem: btrfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 1c4457e00facac03ce1d75f7b6777a7a851e5c41
 runc version: d842d7719497cc3b774fd71620278ac9e17710e0
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.17.8-orbstack-00308-g8f9c941121b1
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 15.67GiB
 Name: orbstack
 ID: 30dabf87-4a8d-4e7e-8b58-b44d7bb76137
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
 Default Address Pools:
   Base: 192.168.97.0/24, Size: 24
   Base: 192.168.107.0/24, Size: 24
   Base: 192.168.117.0/24, Size: 24
   Base: 192.168.147.0/24, Size: 24
   Base: 192.168.148.0/24, Size: 24
   Base: 192.168.155.0/24, Size: 24
   Base: 192.168.156.0/24, Size: 24
   Base: 192.168.158.0/24, Size: 24
   Base: 192.168.163.0/24, Size: 24
   Base: 192.168.164.0/24, Size: 24
   Base: 192.168.165.0/24, Size: 24
   Base: 192.168.166.0/24, Size: 24
   Base: 192.168.167.0/24, Size: 24
   Base: 192.168.171.0/24, Size: 24
   Base: 192.168.172.0/24, Size: 24
   Base: 192.168.181.0/24, Size: 24
   Base: 192.168.183.0/24, Size: 24
   Base: 192.168.186.0/24, Size: 24
   Base: 192.168.207.0/24, Size: 24
   Base: 192.168.214.0/24, Size: 24
   Base: 192.168.215.0/24, Size: 24
   Base: 192.168.216.0/24, Size: 24
   Base: 192.168.223.0/24, Size: 24
   Base: 192.168.227.0/24, Size: 24
   Base: 192.168.228.0/24, Size: 24
   Base: 192.168.229.0/24, Size: 24
   Base: 192.168.237.0/24, Size: 24
   Base: 192.168.239.0/24, Size: 24
   Base: 192.168.242.0/24, Size: 24
   Base: 192.168.247.0/24, Size: 24
   Base: fd07:b51a:cc66:d000::/56, Size: 64

WARNING: DOCKER_INSECURE_NO_IPTABLES_RAW is set
tangled01257696@c5r4s1 ~ % git version
git version 2.53.0
tangled01257696@c5r4s1 ~ % code --version
1.112.0
07ff9d6178ede9a1bd12ad3399074d726ebe6e43
x64

```

> OrbStack이 실행 중이어야 `docker info`가 정상 출력된다.  
> `docker info`에서 Client / Server 정보가 모두 보이면 Docker 엔진과 정상 연결된 상태다.

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
- [ ] hello-world 실행
- [ ] ubuntu 컨테이너 내부 진입 및 명령 실행
- [ ] attach 와 exec 차이 확인
- [ ] Dockerfile 기반 커스텀 이미지 제작
- [ ] 포트 매핑 후 접속 확인
- [ ] 바인드 마운트 실습 (변경 전/후 비교)
- [ ] Docker Volume 영속성 검증 (삭제 전/후 비교)
- [ ] Git 사용자 정보 및 기본 브랜치 설정
- [ ] GitHub 저장소 연동 확인
- [ ] VSCode GitHub 연동 증거 첨부
- [ ] 트러블슈팅 2건 이상 정리
- [ ] 민감정보 마스킹 확인

---

## 3. 프로젝트 구조

```
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

**구조 선택 이유**

- `app/` : 웹 서버에 올릴 소스 파일만 모아둠. Dockerfile의 COPY 경로와 바인드 마운트 경로를 일치시켜 관리하기 쉽게 했다.
- `logs/` : 터미널 조작 로그와 Docker 운영 로그를 분리해서, README가 너무 길어지지 않도록 했다.
- `screenshots/` : 각 단계 번호를 파일명에 넣어 순서대로 확인할 수 있게 했다.

---

## 4. 평가 항목별 검증 요약

| 평가 항목 | 실행 명령 | 확인 결과 | 의미 |
|-----------|-----------|-----------|------|
| 터미널 기본 조작 | `mkdir`, `touch`, `cp`, `mv`, `rm` | 파일/디렉토리 생성·이동·삭제 확인 | 기본 CLI 조작 가능 |
| 파일 권한 | `ls -l`, `chmod 744` | 실행 권한 추가 전후 차이 확인 | 파일에서 x 권한 의미 이해 |
| 디렉토리 권한 | `chmod 655` 후 `cd` 시도 | 접근 실패 확인 | 디렉토리에서 x 권한 의미 이해 |
| Docker 동작 확인 | `docker --version`, `docker info`, `docker context ls` | CLI/Engine 정상 응답 | Docker 사용 가능 |
| 이미지/컨테이너 실습 | `docker pull`, `docker run`, `docker ps -a` | 이미지 다운로드 및 컨테이너 실행 확인 | 이미지와 컨테이너 차이 이해 |
| hello-world 실행 | `docker run hello-world` | 테스트 메시지 출력 | Docker 실행 구조 확인 |
| ubuntu 컨테이너 진입 | `docker run -it ubuntu bash` | 컨테이너 내부 명령 수행 가능 | 독립 실행 환경 확인 |
| 커스텀 이미지 | `docker build`, `docker run` | 커스텀 웹 서버 실행 성공 | Dockerfile 기반 이미지 제작 가능 |
| 포트 매핑 | `-p 8080:80`, `curl http://localhost:8080` | 호스트에서 접속 성공 | 호스트-컨테이너 연결 확인 |
| 바인드 마운트 | `-v "$(pwd)/app/index.html:..."` | 파일 수정 즉시 반영 | 개발 중 빠른 반영 가능 |
| 볼륨 영속성 | `docker volume create`, 컨테이너 재생성 | 데이터 유지 확인 | 컨테이너와 분리된 저장공간 이해 |
| Git/GitHub | `git config --list`, `git remote -v` | 로컬 설정 및 원격 연결 확인 | 버전 관리와 원격 저장소 역할 구분 가능 |

---

## 5. 터미널 기본 조작

개발자는 터미널 안에서 작업 환경을 만들고 정리해야 하므로, 이 단계는 전체 실습의 출발점이다.

### 5-1. 현재 위치 및 목록 확인

```bash
pwd
ls -la
```

```
tangled01257696@c5r4s1 ~ % pwd
/Users/tangled01257696
tangled01257696@c5r4s1 ~ % ls -la
total 8
drwxr-x---+ 18 tangled01257696  tangled01257696   576 Apr 24 19:07 .
drwxr-xr-x   8 root             admin             256 Apr 24 18:38 ..
-r--------   1 tangled01257696  tangled01257696     7 Apr 24 18:38 .CFUserTextEncoding
drwx------+  2 tangled01257696  tangled01257696    64 Apr 24 18:39 .Trash
drwxr-xr-x   5 tangled01257696  tangled01257696   160 Apr 24 18:39 .docker
drwxr-xr-x  10 tangled01257696  tangled01257696   320 Apr 24 18:39 .orbstack
drwxr-xr-x   3 tangled01257696  tangled01257696    96 Apr 24 18:39 .ssh
drwxr-xr-x   5 tangled01257696  tangled01257696   160 Apr 24 18:42 .vscode
drwx------   3 tangled01257696  tangled01257696    96 Apr 24 19:07 .zsh_sessions
drwx------+  3 tangled01257696  tangled01257696    96 Apr 24 18:38 Desktop
drwx------+  4 tangled01257696  tangled01257696   128 Apr 24 18:46 Documents
drwx------+ 13 tangled01257696  tangled01257696   416 Apr 24 18:48 Downloads
drwx------@ 80 tangled01257696  tangled01257696  2560 Apr 24 18:54 Library
drwx------   3 tangled01257696  tangled01257696    96 Apr 24 18:38 Movies
drwx------+  3 tangled01257696  tangled01257696    96 Apr 24 18:38 Music
drwx------   4 tangled01257696  tangled01257696   160 Apr 24 18:39 OrbStack
drwx------+  4 tangled01257696  tangled01257696   128 Apr 24 18:39 Pictures
drwxr-xr-x+  4 tangled01257696  tangled01257696   128 Apr 24 18:38 Public
```

- `pwd`는 현재 작업 디렉토리의 절대 경로를 보여준다.
- `ls -la`는 숨김 파일까지 포함해 목록과 권한 정보를 함께 보여준다.

### 5-2. 작업 디렉토리 생성 및 이동

```bash
mkdir -p ~/dev-workstation-practice
cd ~/dev-workstation-practice
pwd
```

```
tangled01257696@c5r4s1 ~ % mkdir -p ~/dev-workstation-practice
tangled01257696@c5r4s1 ~ % cd ~/dev-workstation-practice
tangled01257696@c5r4s1 dev-workstation-practice % pwd
/Users/tangled01257696/dev-workstation-practice
tangled01257696@c5r4s1 dev-workstation-practice % 
```

- `mkdir -p`를 사용하면 중간 디렉토리가 없어도 한 번에 생성할 수 있다.

### 5-3. 파일 생성 / 내용 작성 / 내용 확인

```bash
touch memo.txt
cat memo.txt
echo "hello workstation" > memo.txt
cat memo.txt
```

```
tangled01257696@c5r4s1 dev-workstation-practice % touch memo.txt
tangled01257696@c5r4s1 dev-workstation-practice % cat memo.txt
tangled01257696@c5r4s1 dev-workstation-practice % echo "hello workstation" > memo.txt
tangled01257696@c5r4s1 dev-workstation-practice % cat memo.txt
hello workstation
tangled01257696@c5r4s1 dev-workstation-practice % 
```

- `touch`로 빈 파일 생성, `echo ... >` 로 내용 기록, `cat`으로 내용 확인.

### 5-4. 파일 복사 / 이동 / 이름 변경

```bash
cp memo.txt copy.txt
mv copy.txt renamed.txt
ls
```

```
tangled01257696@c5r4s1 dev-workstation-practice % cp memo.txt copy.txt
tangled01257696@c5r4s1 dev-workstation-practice % mv copy.txt renamed.txt
tangled01257696@c5r4s1 dev-workstation-practice % ls
memo.txt	renamed.txt


```

- `cp`는 복사, `mv`는 이동 또는 이름 변경에 사용된다.

### 5-5. 디렉토리 생성 / 이동 / 삭제

```bash
mkdir sample-dir
mv renamed.txt sample-dir
ls sample-dir
rm -rf sample-dir
ls
```

```
tangled01257696@c5r4s1 dev-workstation-practice % mkdir sample-dir
tangled01257696@c5r4s1 dev-workstation-practice % mv renamed.txt sample-dir
tangled01257696@c5r4s1 dev-workstation-practice % ls sample-dir
renamed.txt
tangled01257696@c5r4s1 dev-workstation-practice % rm -rf sample-dir
tangled01257696@c5r4s1 dev-workstation-practice % ls
memo.txt

```

- `rm -rf`는 디렉토리와 내부 내용을 함께 삭제하므로 주의해서 사용해야 한다.

### 5-6. 절대 경로와 상대 경로

| 구분 | 정의 | 예시 |
|------|------|------|
| 절대 경로 | 루트(`/`)부터 시작하는 전체 경로 | `/Users/[사용자명]/dev-workstation-practice/memo.txt` |
| 상대 경로 | 현재 위치를 기준으로 표현하는 경로 | `./memo.txt`, `sample-dir/renamed.txt` |

절대 경로는 어디서 실행하든 항상 같은 위치를 가리킨다.  
상대 경로는 현재 위치가 달라지면 가리키는 위치도 달라진다.  
→ 스크립트나 자동화 환경에서는 절대 경로가 안전하고, 프로젝트 내부 파일 참조에는 상대 경로가 편리하다.

---

## 6. 파일 및 디렉토리 권한 검증

파일과 디렉토리에서 `x` 권한의 의미가 다르다는 점을 직접 확인했다.

### 6-1. 파일 권한 확인 및 변경

```bash
echo 'echo hello' > script.sh
ls -l script.sh
./script.sh       # 실행 권한 없어서 Permission denied
chmod 744 script.sh
ls -l script.sh
./script.sh       # 실행 가능
```

```
tangled01257696@c5r4s1 dev-workstation-practice % echo 'echo hello' > script.sh
tangled01257696@c5r4s1 dev-workstation-practice % ls -l script.sh
-rw-r--r--  1 tangled01257696  tangled01257696  11 Apr 24 19:17 script.sh
tangled01257696@c5r4s1 dev-workstation-practice % ./script.sh
zsh: permission denied: ./script.sh
tangled01257696@c5r4s1 dev-workstation-practice % chmod 744 script.sh
tangled01257696@c5r4s1 dev-workstation-practice % ls -l script.sh
-rwxr--r--  1 tangled01257696  tangled01257696  11 Apr 24 19:17 script.sh
tangled01257696@c5r4s1 dev-workstation-practice % ./script.sh
hello

```

- 파일에서 `x`는 실행 가능 여부를 의미한다.
- `chmod 744` 적용 후 소유자에게 실행 권한이 추가되어 정상 실행된다.

### 6-2. 디렉토리 권한 확인 및 변경

```bash
mkdir permission-dir
touch permission-dir/test.txt
ls -ld permission-dir
chmod 655 permission-dir
cd permission-dir   # 소유자만 x 권한이므로 다른 사용자는 cd 실패
```

```
tangled01257696@c5r4s1 dev-workstation-practice % mkdir permission-dir
tangled01257696@c5r4s1 dev-workstation-practice % touch permission-dir/test.txt
tangled01257696@c5r4s1 dev-workstation-practice % ls -ld permission-dir
drwxr-xr-x  3 tangled01257696  tangled01257696  96 Apr 24 19:18 permission-dir
tangled01257696@c5r4s1 dev-workstation-practice % chmod 655 permission-dir
tangled01257696@c5r4s1 dev-workstation-practice % cd permissopn-dir
cd: no such file or directory: permissopn-dir
tangled01257696@c5r4s1 dev-workstation-practice % cd permission-dir
cd: permission denied: permission-dir
tangled01257696@c5r4s1 dev-workstation-practice % 
```

- 디렉토리에서 `x`는 실행이 아니라 해당 디렉토리 **안으로 들어갈 수 있는지** 여부다.

### 6-3. 권한 숫자 규칙

```
r = 4
w = 2
x = 1
```

| 숫자 | 권한 문자 | 의미 |
|------|-----------|------|
| 7 | rwx | 읽기 + 쓰기 + 실행 |
| 6 | rw- | 읽기 + 쓰기 |
| 5 | r-x | 읽기 + 실행 |
| 4 | r-- | 읽기만 |

**대표 예시:**

| 표기 | 소유자 | 그룹 | 기타 |
|------|--------|------|------|
| 744 | rwx | r-- | r-- |
| 755 | rwx | r-x | r-x |
| 644 | rw- | r-- | r-- |

세 자리 숫자 순서는 `소유자 / 그룹 / 기타` 순이다.

---

## 7. Docker 동작 확인

macOS에서는 OrbStack을 통해 Docker 실행 환경을 구성했다.

### 7-1. 점검 명령

```bash
docker --version
docker info
docker context ls
```

```
tangled01257696@c5r4s1 dev-workstation-practice % docker --version
Docker version 28.5.2, build ecc6942
tangled01257696@c5r4s1 dev-workstation-practice % docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/tangled01257696/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/tangled01257696/.docker/cli-plugins/docker-compose

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 28.5.2
 Storage Driver: overlay2
  Backing Filesystem: btrfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 1c4457e00facac03ce1d75f7b6777a7a851e5c41
 runc version: d842d7719497cc3b774fd71620278ac9e17710e0
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.17.8-orbstack-00308-g8f9c941121b1
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 15.67GiB
 Name: orbstack
 ID: 30dabf87-4a8d-4e7e-8b58-b44d7bb76137
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
 Default Address Pools:
   Base: 192.168.97.0/24, Size: 24
   Base: 192.168.107.0/24, Size: 24
   Base: 192.168.117.0/24, Size: 24
   Base: 192.168.147.0/24, Size: 24
   Base: 192.168.148.0/24, Size: 24
   Base: 192.168.155.0/24, Size: 24
   Base: 192.168.156.0/24, Size: 24
   Base: 192.168.158.0/24, Size: 24
   Base: 192.168.163.0/24, Size: 24
   Base: 192.168.164.0/24, Size: 24
   Base: 192.168.165.0/24, Size: 24
   Base: 192.168.166.0/24, Size: 24
   Base: 192.168.167.0/24, Size: 24
   Base: 192.168.171.0/24, Size: 24
   Base: 192.168.172.0/24, Size: 24
   Base: 192.168.181.0/24, Size: 24
   Base: 192.168.183.0/24, Size: 24
   Base: 192.168.186.0/24, Size: 24
   Base: 192.168.207.0/24, Size: 24
   Base: 192.168.214.0/24, Size: 24
   Base: 192.168.215.0/24, Size: 24
   Base: 192.168.216.0/24, Size: 24
   Base: 192.168.223.0/24, Size: 24
   Base: 192.168.227.0/24, Size: 24
   Base: 192.168.228.0/24, Size: 24
   Base: 192.168.229.0/24, Size: 24
   Base: 192.168.237.0/24, Size: 24
   Base: 192.168.239.0/24, Size: 24
   Base: 192.168.242.0/24, Size: 24
   Base: 192.168.247.0/24, Size: 24
   Base: fd07:b51a:cc66:d000::/56, Size: 64

WARNING: DOCKER_INSECURE_NO_IPTABLES_RAW is set
tangled01257696@c5r4s1 dev-workstation-practice % docker context ls
NAME         DESCRIPTION                               DOCKER ENDPOINT                                           ERROR
default      Current DOCKER_HOST based configuration   unix:///var/run/docker.sock                               
orbstack *   OrbStack                                  unix:///Users/tangled01257696/.orbstack/run/docker.sock   
```

- `docker --version`: CLI 설치 및 버전 확인
- `docker info`: Docker Engine과의 통신 여부 확인 (Client/Server 모두 출력되면 정상)
- `docker context ls`: 현재 컨텍스트가 `orbstack`으로 연결되어 있는지 확인

---

## 8. Docker 기본 운영 명령

### 8-1. 이미지 다운로드 및 목록 확인

```bash
docker pull nginx:latest
docker images
```

```
tangled01257696@c5r4s1 dev-workstation-practice % docker context ls
NAME         DESCRIPTION                               DOCKER ENDPOINT                                           ERROR
default      Current DOCKER_HOST based configuration   unix:///var/run/docker.sock                               
orbstack *   OrbStack                                  unix:///Users/tangled01257696/.orbstack/run/docker.sock   
tangled01257696@c5r4s1 dev-workstation-practice % docker pull nginx:latest
latest: Pulling from library/nginx
3531af2bc2a9: Pull complete 
ce776bbcda0d: Pull complete 
85c66128325a: Pull complete 
4677c2a9a3d4: Pull complete 
ff048f1f2159: Pull complete 
677c63196868: Pull complete 
801a1ad15b4e: Pull complete 
Digest: sha256:6e23479198b998e5e25921dff8455837c7636a67111a04a635cf1bb363d199dc
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
tangled01257696@c5r4s1 dev-workstation-practice % docker images
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
nginx        latest    6c3a6ea6608c   2 days ago   161MB
tangled01257696@c5r4s1 dev-workstation-practice % 

```

### 8-2. 컨테이너 실행 / 중지 / 목록 확인

```bash
docker run -d --name web-test1 -p 8080:80 nginx
docker ps
docker stop web-test1
docker ps -a
```

```
tangled01257696@c5r4s1 dev-workstation-practice % docker run -d --name web-test1 -p 8080:80 nginx
e99b26bcc8fd2509e83c8cc2a236fef386bf877a69cc1faaaafe235939c4a4e9
tangled01257696@c5r4s1 dev-workstation-practice % docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                     NAMES
e99b26bcc8fd   nginx     "/docker-entrypoint.…"   5 seconds ago   Up 4 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   web-test1
tangled01257696@c5r4s1 dev-workstation-practice % docker stop web-test1
web-test1
tangled01257696@c5r4s1 dev-workstation-practice % docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                     PORTS     NAMES
e99b26bcc8fd   nginx     "/docker-entrypoint.…"   17 seconds ago   Exited (0) 4 seconds ago             web-test1
tangled01257696@c5r4s1 dev-workstation-practice % 

```

- `docker ps`: 실행 중인 컨테이너만
- `docker ps -a`: 종료된 컨테이너까지 포함한 전체

### 8-3. 로그 및 리소스 사용량 확인

```bash
docker logs web-test1
docker stats --no-stream
```

```
tangled01257696@c5r4s1 dev-workstation-practice % docker logs web-test1
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/04/24 10:21:33 [notice] 1#1: using the "epoll" event method
2026/04/24 10:21:33 [notice] 1#1: nginx/1.29.8
2026/04/24 10:21:33 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19) 
2026/04/24 10:21:33 [notice] 1#1: OS: Linux 6.17.8-orbstack-00308-g8f9c941121b1
2026/04/24 10:21:33 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 20480:1048576
2026/04/24 10:21:33 [notice] 1#1: start worker processes
2026/04/24 10:21:33 [notice] 1#1: start worker process 29
2026/04/24 10:21:33 [notice] 1#1: start worker process 30
2026/04/24 10:21:33 [notice] 1#1: start worker process 31
2026/04/24 10:21:33 [notice] 1#1: start worker process 32
2026/04/24 10:21:33 [notice] 1#1: start worker process 33
2026/04/24 10:21:33 [notice] 1#1: start worker process 34
2026/04/24 10:21:45 [notice] 1#1: signal 3 (SIGQUIT) received, shutting down
2026/04/24 10:21:45 [notice] 29#29: gracefully shutting down
2026/04/24 10:21:45 [notice] 29#29: exiting
2026/04/24 10:21:45 [notice] 29#29: exit
2026/04/24 10:21:45 [notice] 32#32: gracefully shutting down
2026/04/24 10:21:45 [notice] 33#33: gracefully shutting down
2026/04/24 10:21:45 [notice] 32#32: exiting
2026/04/24 10:21:45 [notice] 33#33: exiting
2026/04/24 10:21:45 [notice] 30#30: gracefully shutting down
2026/04/24 10:21:45 [notice] 30#30: exiting
2026/04/24 10:21:45 [notice] 33#33: exit
2026/04/24 10:21:45 [notice] 32#32: exit
2026/04/24 10:21:45 [notice] 31#31: gracefully shutting down
2026/04/24 10:21:45 [notice] 30#30: exit
2026/04/24 10:21:45 [notice] 31#31: exiting
2026/04/24 10:21:45 [notice] 31#31: exit
2026/04/24 10:21:45 [notice] 34#34: gracefully shutting down
2026/04/24 10:21:45 [notice] 34#34: exiting
2026/04/24 10:21:45 [notice] 34#34: exit
2026/04/24 10:21:45 [notice] 1#1: signal 17 (SIGCHLD) received from 33
2026/04/24 10:21:45 [notice] 1#1: worker process 32 exited with code 0
2026/04/24 10:21:45 [notice] 1#1: worker process 33 exited with code 0
2026/04/24 10:21:45 [notice] 1#1: signal 29 (SIGIO) received
2026/04/24 10:21:45 [notice] 1#1: signal 17 (SIGCHLD) received from 32
2026/04/24 10:21:45 [notice] 1#1: signal 17 (SIGCHLD) received from 29
2026/04/24 10:21:45 [notice] 1#1: worker process 29 exited with code 0
2026/04/24 10:21:45 [notice] 1#1: worker process 30 exited with code 0
2026/04/24 10:21:45 [notice] 1#1: signal 29 (SIGIO) received
2026/04/24 10:21:45 [notice] 1#1: signal 17 (SIGCHLD) received from 34
2026/04/24 10:21:45 [notice] 1#1: worker process 34 exited with code 0
2026/04/24 10:21:45 [notice] 1#1: signal 29 (SIGIO) received
2026/04/24 10:21:45 [notice] 1#1: signal 17 (SIGCHLD) received from 31
2026/04/24 10:21:45 [notice] 1#1: worker process 31 exited with code 0
2026/04/24 10:21:45 [notice] 1#1: exit
tangled01257696@c5r4s1 dev-workstation-practice % docker stats --no-stream
CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS

```

- `docker logs`: 컨테이너 실행 이력 로그 확인
- `docker stats --no-stream`: CPU/메모리 사용량 1회성 스냅샷

### 8-4. 이미지와 컨테이너 차이

| 구분 | 설명 |
|------|------|
| 이미지(Image) | 실행을 위한 설계도. 변경되지 않는 읽기 전용 파일. |
| 컨테이너(Container) | 이미지를 실제로 실행한 인스턴스. 실행 중 상태와 변경사항을 가질 수 있음. |

→ 이미지는 "틀(템플릿)", 컨테이너는 "그 틀로 만들어 실행 중인 결과물"이다.  
→ 같은 이미지로 컨테이너를 여러 개 만들 수 있고, 각 컨테이너는 독립적으로 동작한다.

---

## 9. 컨테이너 기본 실습

### 9-1. hello-world 실행

```bash
docker run hello-world
```

```
tangled01257696@c5r4s1 dev-workstation-practice % docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
Digest: sha256:f9078146db2e05e794366b1bfe584a14ea6317f44027d10ef7dad65279026885
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

tangled01257696@c5r4s1 dev-workstation-practice % 

```

`hello-world`는 Docker가 이미지를 내려받고, 컨테이너를 만들고, 실행 결과를 출력하는 **전체 흐름이 정상인지** 확인하는 기본 테스트다.

### 9-2. ubuntu 컨테이너 내부 진입

```bash
docker run -it ubuntu bash
```

컨테이너 내부에서 다음을 실행했다.

```bash
ls
touch test.txt
echo "hello" > test.txt
cat test.txt
pwd
exit
```

```tangled01257696@c5r4s1 dev-workstation-practice % docker run -it ubuntu bash
root@8ef3de62a16a:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@8ef3de62a16a:/# touch 
touch: missing file operand
Try 'touch --help' for more information.
root@8ef3de62a16a:/# touch test.txt
root@8ef3de62a16a:/# echo "hello" > test.txt
root@8ef3de62a16a:/# pwd
/
root@8ef3de62a16a:/# exit
exit

```

- `-it` 옵션으로 컨테이너 내부 쉘과 상호작용할 수 있다.
- 컨테이너 안에서 파일을 만들고 내용을 작성해, 호스트와 분리된 별도 환경임을 확인했다.

### 9-3. attach 와 exec 차이 확인

```bash
# exec 방식: 이미 실행 중인 컨테이너에 새 프로세스로 진입
docker run -d -it --name ubuntu-test1 ubuntu
docker exec -it ubuntu-test1 /bin/bash

# attach 방식: 컨테이너의 메인 프로세스에 직접 연결
docker attach ubuntu-test1
```

| 방식 | 설명 | exit 시 컨테이너 상태 |
|------|------|----------------------|
| `exec` | 실행 중인 컨테이너 안에 새로운 프로세스로 들어간다 | 컨테이너 유지 |
| `attach` | 실행 중인 컨테이너의 메인 프로세스에 직접 연결된다 | bash가 메인 프로세스라면 exit 시 컨테이너도 종료 |

---

## 10. Dockerfile 기반 커스텀 웹 서버 구축

### 10-1. 베이스 이미지 선택

- **베이스 이미지:** `nginx:latest`
- **선택 이유:** 웹 서버 동작 확인이 쉽고, 포트 매핑 실습에 적합하며, 정적 파일 교체만으로 커스텀 결과를 쉽게 확인할 수 있다.

### 10-2. Dockerfile

```dockerfile
FROM nginx:latest
COPY app/index.html /usr/share/nginx/html/index.html
```

**커스텀 포인트:**

| 항목 | 내용 |
|------|------|
| `FROM nginx:latest` | 기존 nginx 공식 이미지를 베이스로 사용 |
| `COPY app/index.html ...` | 기본 nginx 페이지를 내가 만든 HTML로 교체 |

### 10-3. HTML 파일 (`app/index.html`)

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

```
tangled01257696@c5r4s1 dev-workstation-practice % docker build -t my-web .
[+] Building 1.9s (7/7) FINISHED                                docker:orbstack
 => [internal] load build definition from Dockerfile                       0.2s
 => => transferring dockerfile: 108B                                       0.0s
 => [internal] load metadata for docker.io/library/nginx:latest            0.0s
 => [internal] load .dockerignore                                          0.1s
 => => transferring context: 2B                                            0.0s
 => [internal] load build context                                          0.4s
 => => transferring context: 327B                                          0.0s
 => [1/2] FROM docker.io/library/nginx:latest                              0.9s
 => [2/2] COPY app/index.html /usr/share/nginx/html/index.html             0.3s
 => exporting to image                                                     0.2s
 => => exporting layers                                                    0.1s
 => => writing image sha256:55114bdbb0f480c3c19d879af079742484f839d81410b  0.0s
 => => naming to docker.io/library/my-web                                  0.0s
tangled01257696@c5r4s1 dev-workstation-practice % docker images
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
my-web        latest    55114bdbb0f4   10 seconds ago   161MB
nginx         latest    6c3a6ea6608c   2 days ago       161MB
ubuntu        latest    0b1ebe5dd426   2 weeks ago      78.1MB
hello-world   latest    e2ac70e7319a   4 weeks ago      10.1kB
tangled01257696@c5r4s1 dev-workstation-practice % docker rm -f my-web-app 2>/dev/null
tangled01257696@c5r4s1 dev-workstation-practice % docker run -d -p 8080:80 --name my-web-app my-web
ede51ac1e22e3f6d04ea1be21846317ed2c3d62ca750e6c2024c74b35e9064bc
tangled01257696@c5r4s1 dev-workstation-practice % docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                     NAMES
ede51ac1e22e   my-web    "/docker-entrypoint.…"   7 seconds ago   Up 6 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web-app
tangled01257696@c5r4s1 dev-workstation-practice % curl http://localhost:8080
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
tangled01257696@c5r4s1 dev-workstation-practice % 
```

### 10-5. 포트 매핑 접속 결과

```bash
curl http://localhost:8080
```

```
위(10-4) 박스 내의 결과 확인
```

![커스텀 이미지 실행 화면](screenshots/07-port-8080-browser.png)

- 호스트의 8080 포트와 컨테이너 내부의 80 포트를 연결했기 때문에 `localhost:8080`으로 접속할 수 있다.
- **포트 매핑이 필요한 이유:** 컨테이너는 격리된 네트워크 네임스페이스 안에 있다. 호스트에서 컨테이너 내부 포트로 직접 접속할 수 없기 때문에, `-p 호스트포트:컨테이너포트`로 연결 규칙(NAT)을 만들어줘야 한다.

---

## 11. 바인드 마운트와 Volume 검증

### 11-1. 바인드 마운트 실습

바인드 마운트는 호스트의 실제 파일을 컨테이너 내부 경로에 직접 연결하는 방식이다.

**목적:** 이미지를 다시 빌드하지 않아도 호스트 파일 변경이 컨테이너에 바로 반영되는지 검증한다.

```bash
docker run -d -p 8081:80 \
  --name bind-test \
  -v "$(pwd)/app/index.html:/usr/share/nginx/html/index.html" \
  my-web
```

**변경 전 확인**

```bash
curl http://localhost:8081
```

```
tangled01257696@c5r4s1 dev-workstation-practice % docker run -d -p 8081:80 \
  --name bind-test \
  -v "$(pwd)/app/index.html:/usr/share/nginx/html/index.html" \
  my-web
6291493befb3cb11c403ae96af525c6238419979efda15271f67266d9db822c7
tangled01257696@c5r4s1 dev-workstation-practice % curl http://localhost:8081
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
tangled01257696@c5r4s1 dev-workstation-practice % 
```

![바인드 마운트 변경 전](screenshots/08-bind-mount-before.png)

**호스트 파일 수정 후 재확인**

```bash
echo "bind mount success" > app/index.html
curl http://localhost:8081
```

```
tangled01257696@c5r4s1 dev-workstation-practice % echo "bind mount success" > app/index.html
tangled01257696@c5r4s1 dev-workstation-practice % curl http://localhost:8081
bind mount success
tangled01257696@c5r4s1 dev-workstation-practice % 

```

![바인드 마운트 변경 후](screenshots/09-bind-mount-after.png)

이미지를 다시 빌드하지 않아도, 호스트 파일을 수정하자 컨테이너 내부 웹 페이지도 즉시 바뀌었다.  
→ 바인드 마운트는 **개발 중 빠른 반영 확인**에 유리한 방식이다.

---

### 11-2. Docker Volume 영속성 검증

컨테이너 내부 데이터는 컨테이너 삭제 시 사라질 수 있다.  
Docker Volume을 사용하면 컨테이너 밖에 데이터를 저장해 영속성을 확보할 수 있다.

**볼륨 생성 및 컨테이너 연결**

```bash
docker volume create my-data
docker run -d --name volume-test1 -v my-data:/data nginx
docker exec volume-test1 sh -c "echo 'volume data' > /data/test.txt"
docker exec volume-test1 cat /data/test.txt
```

```
tangled01257696@c5r4s1 dev-workstation-practice % docker volume create my-data
my-data
tangled01257696@c5r4s1 dev-workstation-practice % docker run -d --name volume-test1 -v my-data:/data nginx
01e546fb82285ba6fb6083b4f06369aeb71a75ccbe37a038f22454d1da88536d
tangled01257696@c5r4s1 dev-workstation-practice % docker exec volume-test1 sh -c "echo 'volume data' > /data/test.txt"
tangled01257696@c5r4s1 dev-workstation-practice % docker exec volume-test1 cat /data/test.txt
volume data
```

![볼륨 데이터 생성](screenshots/10-volume-before-delete.png)

**컨테이너 삭제 후 새 컨테이너에서 데이터 확인**

```bash
docker stop volume-test1
docker rm volume-test1
docker run -d --name volume-test2 -v my-data:/data nginx
docker exec volume-test2 cat /data/test.txt
```

```
tangled01257696@c5r4s1 dev-workstation-practice % docker stop volume-test1
volume-test1
tangled01257696@c5r4s1 dev-workstation-practice % docker rm volume-test1
volume-test1
tangled01257696@c5r4s1 dev-workstation-practice % docker run -d --name volume-test2 -v my-data:/data nginx
5381f944f488b6fd9c4be8bcd0b1082f78ac5ca1278a2001a0f5dbbea00f599e
tangled01257696@c5r4s1 dev-workstation-practice % docker exec volume-test2 cat /data/test.txt
volume data
tangled01257696@c5r4s1 dev-workstation-practice % 

```

![볼륨 영속성 확인](screenshots/11-volume-after-recreate.png)

컨테이너는 삭제되었지만 `my-data` 볼륨은 남아 있었고, 새 컨테이너에서도 이전 데이터가 그대로 확인되었다.  
→ **Volume은 컨테이너 생명주기와 분리된 저장공간**이라는 점을 직접 검증했다.

---

### 11-3. 바인드 마운트 vs Volume 비교

| 항목 | 바인드 마운트 | Docker Volume |
|------|--------------|--------------|
| 저장 위치 | 호스트의 지정 경로 | Docker가 관리하는 별도 공간 |
| 누가 관리 | 사용자 직접 | Docker 자동 관리 |
| 주요 용도 | 개발 중 빠른 반영 확인 | 운영 환경 영속 데이터 저장 |
| 이식성 | 낮음 (경로 의존) | 상대적으로 높음 |

---

## 12. Git 설정 및 GitHub 연동

### 12-1. Git 설정

```bash
git config --global init.defaultBranch main
git config --global user.name "[사용자명]"
git config --global user.email "[이메일]"
git config --list
```

```
tangled01257696@c5r4s1 dev-workstation-practice % git config --global init.defaultBranch main
tangled01257696@c5r4s1 dev-workstation-practice % git config --global user.name "김현서"    
tangled01257696@c5r4s1 dev-workstation-practice % git config --global user.email "tangled0125@gmail.com"
tangled01257696@c5r4s1 dev-workstation-practice % git config --list
credential.helper=osxkeychain
init.defaultbranch=main
user.name=김현서
user.email=tangled0125@gmail.com
tangled01257696@c5r4s1 dev-workstation-practice % 
```

### 12-2. 원격 저장소 연결 확인

```bash
git remote -v
```

```
[실제 출력 붙여넣기]
```

### 12-3. Git과 GitHub 차이

| 구분 | 역할 |
|------|------|
| Git | 로컬에서 소스 코드 변경 이력을 관리하는 버전 관리 도구 |
| GitHub | Git 저장소를 원격으로 올리고 공유·협업하는 플랫폼 |

→ Git은 버전 관리 자체, GitHub는 그 기록을 원격에서 저장하고 팀원과 공유하는 공간이다.

### 12-4. 연동 증거

![Git 설정 확인](screenshots/12-git-config.png)

![VSCode GitHub 연동](screenshots/13-vscode-github.png)

---

## 13. 트러블슈팅

### 13-1. Ubuntu 컨테이너가 바로 종료되는 문제

**문제**  
`docker run -d --name ubuntu-test1 ubuntu` 실행 후 컨테이너가 곧바로 종료되었다.

**원인 가설**  
Ubuntu 이미지는 기본적으로 셸을 실행하지만, 백그라운드 모드에서 입력을 받을 수 있는 터미널이 없으면 메인 프로세스가 바로 종료된다. Docker 컨테이너는 메인 프로세스가 종료되면 함께 종료된다.

**확인**

```bash
docker ps -a
# STATUS가 Exited (0)으로 표시되어 있는 것 확인
```

**해결**

```bash
docker run -d -it --name ubuntu-test1 ubuntu
docker ps -a
# STATUS가 Up 상태로 유지되는 것 확인
```

**배운 점**  
백그라운드에서 셸을 유지하려면 `-d`만이 아니라 `-it`까지 함께 필요하다.  
컨테이너는 메인 프로세스가 살아 있어야 유지된다는 점을 이해했다.

---

### 13-2. 포트 충돌 후 컨테이너 이름 중복이 발생한 문제

**문제**  
이미 8080 포트를 사용 중인 상태에서 새 컨테이너를 실행하려 했고, 포트를 바꿔 다시 실행했는데도 이름 충돌이 발생했다.

**원인 가설**  
포트 충돌로 컨테이너 실행은 실패했지만, 생성 단계에서 컨테이너 객체가 남아 이름을 점유하고 있었다.

**확인**

```bash
docker ps -a
# 실패한 컨테이너가 Created 또는 Exited 상태로 남아있는 것 확인
```

**해결**

```bash
docker rm bind-test
docker run -d -p 8081:80 --name bind-test \
  -v "$(pwd)/app/index.html:/usr/share/nginx/html/index.html" \
  my-web
```

**배운 점**  
`docker run`이 실패한 뒤에도 컨테이너 껍데기가 남을 수 있다.  
문제가 생기면 먼저 `docker ps -a`로 전체 목록을 확인하는 습관이 중요하다.

---

## 14. 재현 방법

README만 보고도 같은 결과를 확인할 수 있도록 아래 순서로 진행하면 된다.

### 저장소 클론

```bash
git clone https://github.com/[github-id]/[repository-name].git
cd [repository-name]
```

### 커스텀 이미지 빌드

```bash
docker build -t my-web .
```

### 컨테이너 실행 및 접속 확인

```bash
docker run -d -p 8080:80 --name my-web-app my-web
curl http://localhost:8080
```

### 바인드 마운트 확인

```bash
docker run -d -p 8081:80 \
  --name bind-test \
  -v "$(pwd)/app/index.html:/usr/share/nginx/html/index.html" \
  my-web
curl http://localhost:8081
echo "bind mount success" > app/index.html
curl http://localhost:8081
```

### 볼륨 영속성 확인

```bash
docker volume create my-data
docker run -d --name volume-test1 -v my-data:/data nginx
docker exec volume-test1 sh -c "echo 'volume data' > /data/test.txt"
docker rm -f volume-test1
docker run -d --name volume-test2 -v my-data:/data nginx
docker exec volume-test2 cat /data/test.txt
```

---

## 15. 심층 질문 대비 정리

### Q1. 이미지와 컨테이너의 차이를 빌드/실행/변경 관점에서 설명하라

- **빌드:** Dockerfile을 기반으로 이미지를 만드는 과정. 이미지는 읽기 전용으로 완성된다.
- **실행:** 이미지를 기반으로 컨테이너를 생성해 실행한다. 이 시점부터 쓰기 가능한 레이어가 이미지 위에 얹힌다.
- **변경:** 컨테이너 안에서 파일을 수정하면 writable layer에만 기록된다. 원본 이미지는 변하지 않는다. 컨테이너를 삭제하면 그 변경사항도 사라진다.

### Q2. 컨테이너 내부 포트로 직접 접속할 수 없는 이유와 포트 매핑이 필요한 이유

컨테이너는 호스트와 격리된 별도의 네트워크 네임스페이스 안에 있다.  
외부에서는 그 내부 네트워크 공간(IP/포트)을 직접 찾을 수 없다.  
`-p 호스트포트:컨테이너포트`를 설정하면, Docker가 호스트 포트로 들어온 트래픽을 컨테이너 포트로 전달하는 NAT 규칙을 만들어준다.  
즉, 포트 매핑은 호스트와 컨테이너 사이의 "번역기" 역할을 하는 것이다.

### Q3. 절대 경로와 상대 경로를 어떤 상황에서 선택하는지

- **절대 경로 사용:** 스크립트, 자동화, cron, Dockerfile 등 실행 위치가 달라질 수 있는 환경. 어디서 실행해도 항상 같은 위치를 가리킨다.
- **상대 경로 사용:** 프로젝트 내부 파일 참조, 터미널에서 현재 작업 디렉토리 기준으로 이동할 때. 간결하고 프로젝트 구조에 유연하다.

### Q4. 호스트 포트가 이미 사용 중이라 포트 매핑이 실패한다면, 어떤 순서로 진단하는지

1. `docker ps -a` → 같은 포트를 점유한 컨테이너가 있는지 확인
2. `lsof -i :포트번호` (macOS) 또는 `ss -ltnp` (Linux) → 호스트에서 해당 포트를 사용 중인 프로세스 확인
3. 원인이 되는 컨테이너 또는 프로세스를 중지
4. 포트를 바꿔서 컨테이너를 다시 실행

### Q5. 컨테이너 삭제 후 데이터가 사라진 경험이 있다면, 방지 대안은

컨테이너 안에서 파일을 직접 수정하면 writable layer에만 저장되어, 컨테이너 삭제 시 함께 사라진다.

**대안:**
- **Docker Volume:** 영속 데이터 저장. 컨테이너가 삭제되어도 볼륨은 남는다. (`-v my-data:/data`)
- **바인드 마운트:** 호스트의 특정 경로를 컨테이너에 직접 연결. 호스트에 파일이 남아있어 삭제되지 않는다. (`-v $(pwd)/app:/app`)

### Q6. 프로젝트 디렉토리 구조를 어떤 기준으로 구성했는지

- `app/`: 웹 서버에 올릴 소스. Dockerfile의 COPY 경로와 바인드 마운트 경로를 일치시켜 혼동을 줄였다.
- `logs/`: 터미널·Docker 운영 로그를 분리해 README가 너무 길어지지 않게 했다.
- `screenshots/`: 번호를 붙여 실습 순서대로 확인할 수 있게 했다.

---

---

*이 문서는 단순한 명령어 나열이 아니라, 실제 개발 환경을 구성하고 검증한 과정을 재현 가능하게 남기는 데 목적이 있다.*
