# 개발 워크스테이션 구축 실습

## 1. 프로젝트 개요

이 프로젝트는 리눅스 CLI, Docker, Git/GitHub를 활용하여 재현 가능한 개발 워크스테이션을 직접 구축하고 검증한 결과를 정리한 문서다.  
단순히 명령어를 따라 치는 것이 아니라, 다음 내용을 직접 수행하고 결과로 확인하는 것을 목표로 했다.

- 터미널 기반 파일/디렉토리 조작
- 파일 및 디렉토리 권한 확인/변경
- Docker 설치 상태 및 데몬 동작 점검
- Docker 이미지/컨테이너 운영 명령 실습
- `hello-world`, `ubuntu` 컨테이너 실행 실습
- 기존 베이스 이미지를 활용한 커스텀 이미지 제작
- 포트 매핑을 통한 웹 서버 접속 확인
- 바인드 마운트 반영 확인
- Docker 볼륨을 이용한 데이터 영속성 검증
- Git 설정 및 GitHub/VSCode 연동 확인

이 문서는 README만 읽어도 수행한 절차, 사용한 명령어, 실행 결과, 검증 방식, 문제 해결 과정을 모두 이해할 수 있도록 작성했다.

---

## 2. 실행 환경

| 항목 | 내용 |
|---|---|
| OS | [예: macOS Sonoma / Ubuntu 22.04 / Windows 11 + WSL2] |
| Shell | [예: zsh / bash] |
| Terminal | [예: iTerm2 / macOS Terminal / Windows Terminal] |
| Docker Runtime | [예: OrbStack / Docker Desktop] |
| Docker Version | [실제 버전 입력] |
| Git Version | [실제 버전 입력] |
| VSCode Version | [실제 버전 입력] |

### 2-1. 환경 확인 명령

```bash
uname -a
echo $SHELL
docker --version
docker info
git --version
code --version
