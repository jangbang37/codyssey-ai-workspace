# Codyssey AI All-in-One Mission
## 내 컴퓨터에 개발자용 작업실 꾸미기

### 1. 프로젝트 소개
개발 환경을 구성하고 Windows 터미널, Linux, Git, GitHub, Docker의 기본 기능을 실습한 프로젝트입니다.

### 2. 실습 환경

| 항목 | 환경 |
|---|---|
| 개인 PC OS | Windows |
| 터미널 | CMD, Git Bash |
| 코드 편집기 | Visual Studio Code 1.144.0 |
| Git | 2.56.0.windows.4 |
| Linux 실습 환경 | Killercoda Ubuntu 24.04 |
| Docker | 29.1.3 |

※ Docker와 Linux 실습은 Windows PC가 아닌 Killercoda의 임시 Ubuntu 환경에서 진행했습니다.

### 3. 실습 체크리스트

- [x] 터미널 기본 명령어 실습
- [x] 파일 및 디렉터리 생성·복사·이동·삭제
- [x] Linux 파일 및 디렉터리 권한 변경
- [x] Docker 버전 및 엔진 정보 확인
- [x] Docker 이미지 다운로드 및 목록 확인
- [x] 컨테이너 실행·중지·목록 확인
- [x] Dockerfile 작성 및 사용자 정의 이미지 빌드
- [x] 포트 매핑 및 웹 서버 접속 확인
- [x] Docker Volume 데이터 유지 확인
- [x] Bind Mount 파일 공유 확인
- [x] Volume 백업 파일 생성 및 내부 목록 확인
- [x] Git 설정 및 GitHub 저장소 연결

### 4. 터미널 기본 명령어

Windows CMD에서 다음 작업을 실습했습니다.

| 작업 | 명령어 |
|---|---|
| 현재 위치 확인 | `cd` |
| 파일 목록 확인 | `dir` |
| 숨김 파일 포함 확인 | `dir /a` |
| 폴더 이동 | `cd 폴더명` |
| 폴더 생성 | `mkdir 폴더명` |
| 빈 파일 생성 | `type nul > 파일명` |
| 파일 내용 작성 | `echo 내용 > 파일명` |
| 파일 내용 확인 | `type 파일명` |
| 파일 복사 | `copy 원본 복사본` |
| 파일 이름 변경 | `ren 기존이름 새이름` |
| 파일 이동 | `move 파일명 폴더명` |
| 파일 삭제 | `del 파일명` |
| 빈 폴더 삭제 | `rmdir 폴더명` |

### 5. Linux 권한 실습

Killercoda Ubuntu에서 파일과 디렉터리의 권한을 확인하고 변경했습니다.

파일 권한:
```bash
touch practice.txt
stat -c "%a %n" practice.txt
chmod 744 practice.txt
stat -c "%a %n" practice.txt
```

디렉터리 권한:
```bash
mkdir practice_dir
ls -ld practice_dir
chmod 700 practice_dir
stat -c "%a %n" practice_dir
```

실습 결과:
- 파일 권한: 644 → 744
- 디렉터리 권한: 755 → 700

권한의 의미:
- r: 읽기
- w: 쓰기
- x: 실행 또는 디렉터리 접근

### 6. Docker 기본 실습

Docker 버전 및 엔진 정보:
```bash
docker --version
docker info
```

기본 이미지 실행:
```bash
docker run hello-world
docker images
```

Ubuntu 컨테이너 실행:
```bash
docker run -it --name codyssey-ubuntu ubuntu bash
```

컨테이너 내부에서 `pwd`, `ls`, `cd`, `echo`, `cat` 명령어를 실습했습니다.

컨테이너 확인:
```bash
docker ps
docker ps -a
docker start codyssey-ubuntu
docker exec -it codyssey-ubuntu bash
docker logs codyssey-web
docker stats --no-stream codyssey-web
```

### 7. 사용자 정의 Docker 이미지 제작

Nginx 기반 Dockerfile을 작성했습니다.

Dockerfile:
```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

이미지 빌드:
```bash
docker build -t codyssey-web:v1 .
docker images
```

컨테이너 실행:
```bash
docker run -d --name codyssey-web -p 8080:80 codyssey-web:v1
```

웹 서버 접속 확인:
```bash
curl http://localhost:8080
```

두 번째 포트도 확인했습니다.
```bash
docker run -d --name codyssey-web-2 -p 8081:80 codyssey-web:v1
curl http://localhost:8081
```

두 포트 모두 `Hello, Codyssey!`가 포함된 HTML을 정상 출력했습니다.

### 8. Docker Volume 실습

볼륨 생성:
```bash
docker volume create codyssey-data
docker volume ls
```

볼륨에 데이터 저장:
```bash
docker run -it --name volume-test -v codyssey-data:/data ubuntu bash
```

컨테이너 내부에서:
```bash
echo "Docker volume persistence test" > /data/saved.txt
cat /data/saved.txt
```

테스트 컨테이너 삭제:
```bash
docker rm volume-test
```

새 컨테이너에서 데이터 확인:
```bash
docker run --rm -v codyssey-data:/data ubuntu cat /data/saved.txt
```

출력 결과:
```text
Docker volume persistence test
```

컨테이너를 삭제한 후에도 볼륨의 데이터가 유지되는 것을 확인했습니다.

### 9. Bind Mount 실습

호스트 디렉터리 생성 및 파일 작성:
```bash
mkdir -p /root/codyssey-host
echo "Hello from host folder" > /root/codyssey-host/host.txt
```

컨테이너에 연결:
```bash
docker run --rm -it -v /root/codyssey-host:/mnt/host ubuntu bash
```

컨테이너 내부에서 확인 및 수정:
```bash
cat /mnt/host/host.txt
echo "Modified inside container" >> /mnt/host/host.txt
```

호스트에서도 변경된 내용을 확인했습니다.

### 10. Volume 백업

볼륨 데이터를 압축 파일로 백업했습니다.

```bash
docker run --rm -v codyssey-data:/data -v /root:/backup ubuntu tar -czf /backup/codyssey-data-backup.tar.gz -C /data .
```

백업 파일 확인:
```bash
ls -lh /root/codyssey-data-backup.tar.gz
tar -tzf /root/codyssey-data-backup.tar.gz
```

압축 파일 내부에서 `saved.txt`를 확인했습니다.

※ 백업 파일 생성과 내부 목록 확인까지 진행했으며, 복원 실습은 진행하지 않았습니다.

### 11. Git 및 GitHub

Git 사용자 설정:
```bash
git config --global user.name "Jiyoung"
git config --global user.email "내 GitHub 이메일"
git config --global init.defaultBranch main
git config --global --list
```

저장소 초기화 및 커밋:
```bash
git init
git add practice.txt
git commit -m "Add practice file"
git status
```

GitHub 원격 저장소 연결:
```bash
git remote add origin https://github.com/jangbang37/codyssey-ai-workspace.git
git push -u origin main
```

GitHub 저장소:
https://github.com/jangbang37/codyssey-ai-workspace

### 12. 문제 해결 기록

**문제 1. Git 명령어를 인식하지 못함**

- 원인: Git 실행 파일 경로가 Windows PATH에 등록되지 않음
- 해결: `C:\Program Files\Git\cmd`를 사용자 PATH에 추가
- 확인: `git --version` 정상 출력

**문제 2. Git 설정 파일을 찾을 수 없음**

- 원인: 최초 Git 사용자 설정이 되어 있지 않았음
- 해결: `git config --global`로 사용자 이름, 이메일, 기본 브랜치 설정

**문제 3. Git Bash에서 chmod 변경이 반영되지 않음**

- 원인: Windows NTFS 환경의 Git Bash에서는 Linux와 같은 권한 변경이 그대로 반영되지 않을 수 있음
- 해결: Killercoda Ubuntu에서 권한 변경을 다시 실습하고 결과 확인

**문제 4. Docker 명령어를 찾을 수 없음**

- 원인: Ubuntu 컨테이너 내부에서 Docker 명령어를 실행함
- 해결: `exit`로 호스트 터미널로 돌아와 Docker 명령어 실행

### 13. 마무리

이번 실습을 통해 터미널 사용법, Linux 권한 관리, Git 및 GitHub 버전 관리, Docker 이미지와 컨테이너, 포트 매핑, Volume 및 Bind Mount의 기본 개념을 익혔습니다.