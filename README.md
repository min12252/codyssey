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

2. 권한
```bash
# 권한 확인
ls -l

# 권한 변경
chmod 700 file.txt
```

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

4. Dockerfile 기반 커스텀 이미지 제작, 포드매핑, 볼륨 영속성
선택한 베이스 이미지: nginx:latest (Nginx 웹 서버 기본 이미지)   
커스텀 포인트 및 목적:
기존 Nginx의 기본 환영 페이지(Welcome to nginx!) 대신, 사용자가 직접 작성한 index.html 파일을 도커 이미지 빌드 시점에 컨테이너 내부의 웹 경로(/usr/share/nginx/html/index.html)로 복사(COPY)하도록 설정함.   
이를 통해 서버가 실행될 때 커스텀 웹페이지가 기본으로 서빙되도록 함.

5. run, attach, exec 비교
``` bash
docker run # 새로 만들고 실행
docker attach # 메인에 접속
docker exec # 독립적으로 내부 접속
```
| 명령어 | 대상 | 하는 일 | 주요쓰임 |
| --- | --- | --- |
| run | 이미지 | 새 컨테이너 생성 + 실행 | 프로그램을 처음 켤 때 |
| --- | --- | --- |
| attach | 실행 중인 컨테이너 | 메인 화면에 그냥 접속 | 돌아가는 상태를 실시간으로 볼 때 |
| --- | --- | --- |
| exec | 실행 중인 컨테이너 | 안에서 새로운 명령(bash 등) 실행 | 컨테이너 내부를 직접 탐색하고 싶을 때 |

exec는 exit으로 나와도 컨테이너가 꺼지지 않지만 run,attach는 꺼짐

7. git



## 트러블슈팅
문제1.
    원인가설
    확인
    해결/대안
문제2.
    원인가설
    확인
    해결/대안