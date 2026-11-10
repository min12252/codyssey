# 미션1: 개발 환경설정

## 프로젝트 개요
본 미션은 팀원 누구나 같은 방식으로 실행, 배포, 디버깅할 수 있는 환경 구성을 목표로 합니다.

## 실행환경
OS: macOS 26.2
쉘: /bin/zsh
터미널: iTerm2
Docker: 29.9.2
Git: 2.39.3 (Apple Git-146)

## 수행 항목 체크리스트 및 검증방법
- [x] 터미널 기본 조작 및 폴더 구성
- [x] 권한 변경 실습
- [x] Docker 설치/점검
- [x] hello-world 실행
- [x] Dockerfile 빌드/실행
- [x] 포트 매핑 접속(2회)
- [x] 바인드 마운트 반영
- [x] 볼륨 영속성
- [x] Git 설정 + VSCode GitHub 연동

<br>

1. 터미널 조작 로그 기록
``` bash
# 현재 위치 확인
pwd

# 목록 확인 (숨김 파일 포함)
ls -a

# 파일 생성 (빈 파일)
touch testFile.txt

# 폴더 생성
mkdir testFolder

# 이동
mv testFile.txt ./testFolder

# 복사
cp testFile.txt copy_testFile.txt

# 이동/이름변경
mv testFile.txt ../new.txt

# 삭제
rm new.txt

# 파일 내용 확인
cat copy_testFile.txt
```
[터미널 로그](log/터미널조작.png)

<br>

2. 권한
```bash
# 권한 확인
ls -l

# 권한 변경
chmod 700 file.txt
```
[권한 로그](log/권한.png)

<br>

3. Docker
```bash
# 버전확인
docker --version

# 점검
docker info

# 이미지 다운로드, 컨테이너 만들기, 실행
docker run hello-world

# 이미지 목록 확인
docker images

# 컨테이너 목록 확인
docker ps
docker ps -a

# 컨테이너 실행
docker run -it --name test ubuntu

# 컨테이너 중지 확인
docker stop test

# 로그 확인
docker logs test

# 리소스 확인 
docker stats --no-stream
```
[도커 로그](log/docker_logs.txt)

<br>

4. Dockerfile 기반 커스텀 이미지 제작, 포드매핑, 볼륨 영속성

<br>

```bash
# 빌드,이미지 만들기
% docker build -t my-web

# 포트매핑
% docker run -d -p 8080:80 --name my-web my-web

# 내 폴더를 컨테이너에 연결 (호스트의 html 디렉터리를 컨테이너 내부의 NGINX 웹 디렉터리에 연결)
% docker run -d -p 8082:80 --name my-web-live -v ${PWD}:/usr/share/nginx/html nginx
```
<br>

```bash
# 볼륨 생성
% docker volume create mydata

# 새 컨테이너 생성 및 볼륨 연결
% docker run -it --name vol-test -v mydata:/data ubuntu

# 볼륨 리스트 확인
% docker volume ls
```

<br>

* 선택한 베이스 이미지: nginx:latest (Nginx 웹 서버 기본 이미지)

<br> 

* 커스텀 포인트 및 목적:
기존 Nginx의 기본 환영 페이지(Welcome to nginx!) 대신, 사용자가 직접 작성한 index.html 파일을 도커 이미지 빌드 시점에 컨테이너 내부의 웹 경로(/usr/share/nginx/html/index.html)로 복사(COPY)하도록 설정함.   
이를 통해 서버가 실행될 때 커스텀 웹페이지가 기본으로 서빙되도록 함.

<br>

[도커파일 로그](log/dockerfile_custom.txt)

<br>

[web 실행화면](log/dockerfile실행.png)

<br>

[볼륨 영속성](log/볼륨영속성.txt)

<br>

5. run, attach, exec 비교
``` bash
docker run # 새로 만들고 실행
docker attach # 메인에 접속
docker exec # 독립적으로 내부 접속
```

| 명령어 | 대상 | 하는 일 | 주요쓰임 |
| --- | --- | --- | --- |
| run | 이미지 | 새 컨테이너 생성 + 실행 | 프로그램을 처음 켤 때 |
| attach | 실행 중인 컨테이너 | 메인 화면에 그냥 접속 | 돌아가는 상태를 실시간으로 볼 때 |
| exec | 실행 중인 컨테이너 | 안에서 새로운 명령(bash 등) 실행 | 컨테이너 내부를 직접 탐색하고 싶을 때 |

* exec는 exit으로 나와도 컨테이너가 꺼지지 않지만 run,attach는 꺼짐

<br>

6. git
```bash
% git config --global user.name
min12252
% git config --global user.email
bbrr7709@gmail.com
```
<br>

[깃 연결 확인](log/git.txt)

<br>

[vs code git연동](log/vscode.png)

<br>

## 트러블슈팅
문제1.  nginx서버 띄웠는데 html 파일이 안뜨는 문제

* 원인가설
기본 nginx 화면은 뜨는걸로 봐서 서버의 문제는 아니고 파일에 접근이 안되는 문제라고 예측

* 확인
Dockerfile 파일 확인

* 해결/대안
Dockerfile 내에 경로가 잘못 설정되어있어서 수정후 오류 해결

문제2. 컨테이너가 없는데 새로 생성되지 않는 오류

```bash
% docker run -d -p 8082:80 --name my-web2 my-web

docker: Error response from daemon: Conflict. The container name "/my-web2" is already in use by container "2167d1ec6700d55ea990919749e4e3681b85f6bfeb3c1f5d235ad7cc01038058". You have to remove (or rename) that container to be able to reuse that name.
```
* 원인가설
컨테이너가 생성시 포트 연결이 안되서 에러가 나도 컨테이너는 생성이 되었을거라 예측

* 확인
docker ps -a 확인

* 해결/대안
정지된 컨테이너 중에 컨테이너가 있어서 지우고 새로 생성