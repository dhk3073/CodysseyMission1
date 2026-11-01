# CodysseyMission1

# 과제 명 : 내 컴퓨터에 개발자용 '작업실' 꾸미기

## 과제 목표

- 절대 경로와 상대 경로의 차이를 예시를 들어 설명할 수 있다.
  - 절대 경로: 표기된 경로의 맨 앞이 '/'(= 루트 디렉토리)로 시작되는 경로.
  - 상대 경로: 현재 작업중인 디렉토리를 기준으로 위치를 지정하는 경로. 현재 위치가 어디인지 유의해야한다.
- 파일 권한의 의미(r/w/x)와 755, 644 같은 표기가 어떤 규칙으로 해석되는지 설명할 수 있다.
  - r : 읽기 권한, 파일 내부 항목의 이름 조회
  - w : 쓰기 권한, 파일 내용 변경 등의 작업에 사용
  - x : 실행 권한, 프로그램이나 스크립트를 실행하는 권한
  - 생성 및 삭제는 w,x 두가지 권한을 동시에 요구 / 경로 탐색에는 대상 파일의 중간경로의 실행권한이 필요.
- 기존 Dockerfile을 기반으로 “커스텀 이미지”를 만들 수 있다.
- 포트 매핑이 필요한 이유를 설명할 수 있다.
- Docker 볼륨(영속 데이터)을 설명할 수 있다.
- Git과 GitHub의 역할 차이(로컬 버전관리 vs 원격 협업 플랫폼)를 설명할 수 있다.

## 개발 환경

- 호스트 OS: Windows 11
- Linux 환경: WSL2
- 터미널: Ubuntu 콘솔(WSL)
- 쉘: Bash

## 작업 내용

### 1. 터미널 로그 조작 기록

```bash
pwd                       //현재 위치 확인
ls -la                    //목록 확인(숨김 파일 포함)
mkdir work1               //경로 생성
cd work1                  //경로 이동
touch test1.txt           //빈 파일 생성
cat test1.txt             //파일 내용 확인
cp test1.txt mvtest1.txt  //파일 복사
ls -a                     //복사 여부 확인
mv mvtest1.txt ..         //파일 이동
cd ..
ls -a                     //경로 이동 및 파일 이동확인
rm mvtest1.txt
ls -a                     //파일 삭제 및 삭제여부 확인
```

![실습 결과](작업내용(new)/2. 터미널 조작 로그 기록.png)

- 체크리스트
  - 현재 위치 확인  [o]
  - 목록 확인(숨김파일 포함)  [o]
  - 경로 이동  [o]
  - 경로 생성  [o]
  - 파일 복사  [o]
  - 파일 이동/이름 변경  [o]
  - 파일 삭제  [o]
  - 빈 파일 생성 [o]
 
### 2.권한 실습 및 증거 기록

```bash
ls -la              //권한 확인
chmod 711 work1     //디렉토리 권한 변경
ls -la              //변경된 권한 확인
chmod 755 work1     //기존 권한상태로 복구
ls -la              //내부 파일 권한 확인
chmod 755 test1.txt //파일 권한 변경
ls -la              //변경된 권한 확인
```

- 체크리스트
  - 권한 확인/변경 명령 수행 [o]
  - 변경 전/후 비교 [o]
  - 변경 수량 : 디렉토리 1개, 파일 1개
 
### 3. Docker 설치 및 기본 점검

```bash
docker --version
docker info
```

(이미지 첨부)

- 체크리스트
  - 도커 버전 확인 결과 기록 [o]
  - 도커 데몬 동작 여부 확인 결과 기록 [o]

### 4. Docker 기본 운영 명령 수행

```bash
docker images                                //저장된 이미지 확인, 없을 경우 docker pull로 이미지 다운로드
docker ps
docker ps -a
docker logs practice-container               //로그 확인, 옵션 미선택시 전체 로그 출력
docker stats -no--stream practice-container  //리소스 확인, -no--stream 옵션 미선택시 실시간 측정, ctrl+c로 종료가능
```

(이미지 첨부)

- 체크리스트
  - 이미지 명령 [o]
  - 컨테이너 명령 [o]
  - 운영 명령 [o]

### 5. 컨테이너 실행 실습

```bash
docker run hello-world    //hello world 명령어
docker ps -a              //정상실행 확인
```

(이미지 첨부)

```bash
docker ps -a
docker run -it --name ubuntu ubuntu:24.04 bash      //컨테이너 실행
pwd                                                 //현재 위치 확인(컨테이너 내부)
ls
echo "Hello World!"                                 //간단한 명령어 실행
```

(이미지 첨부)

```bash
exit                                                //컨테이너 탈출
docker ps -a                                        //컨테이너 상태 확인
docker start ubuntu                                 //컨테이너 재실행
docker ps                                           //실행 여부 확인
docker attach ubuntu                                //컨테이너 접속(attach)
echo "Attached to container"
exit                                                //컨테이너 탈출
docker ps -a                                        //컨테이너 상태 확인
```

(이미지 첨부)
- attach로 접속된 컨테이너는 exit시 종료됨

```bash
docker start ubuntu                                 //컨테이너 재실행
docekr exec -it ubuntu bash                         //컨테이너 접속(exec)
echo "Executed new Bash"
exit                                                //컨테이너 탈출
docker ps                                           //컨테이너 상태 확인
```

(이미지 첨부)

- exec로 접속한 컨테이너는 exit 후에도 실행된 상태

- 원인
  - 컨테이너의 생명주기는 주프로세스를 따라감.
  - attach의 경우 컨테이너의 기존 프로세스에 연결해, 해당 프로세스를 exit할 시 주프로세스가 종료되어 컨테이너 또한 종료됨
  - exec의 경우 새로운 프로세스를 생성하기 때문에 exit하더라도 주프로세스가 유지되어있으므로 컨테이너가 종료되지 않음.

- 체크리스트
  - hello-world 실행 성공 [o]
  - ubuntu 컨테이너 실행 후 간단한 명령 수행 [o]
  - 컨테이너 종료/유지 의 차이 관찰 후 정리 [o]
 
### 6. 기존 Dockerfile 기반 커스텀 이미지 제작

```bash
cd work1
mkdir docker-custom
cd docker-custom
pwd                              //커스텀 이미지 제작용 디렉토리 생성 및 확인
nano Dockerfile                  //Dockerfile 작성

// 도커파일 내부

FROM ubuntu:24.04
// Ubuntu 패키지 목록 및 관련 정보 갱신, curl 프로그램 설치, 불필요한 패키지 제거
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    rm -rf /var/lib/apt/lists/*
// 사용자 생성 (제3자 권한 제한용)
RUN useradd -m -s /bin/bash student
// 환경변수 생성 (기능 확인용)
ENV APP_ENV=practice
// 이후 명령어의 기본 작업 디렉토리 설정
WORKDIR /home/student
// 생성한 사용자를 실행 주체로 지정
USER student
// 컨테이너 실행 시 기본값 지정
CMD ["bash"]

// 도커파일 탈출 (ctrl O > enter > ctrl X)

docker build -t my-ubuntu:v1 .                         // 커스텀 이미지 빌드
docker image                                           // 빌드된 이미지 확인
docker run -it --name custom-ubuntu-test my-ubuntu:v1  // 커스텀 이미지로 컨테이너 생성

//컨테이너 내부
whoami
pwd
echo $APP_ENV
curl --version                                         // 커스텀 요소 정상작동 확인
```

(이미지 첨부)

- 커스텀 목록 및 사유
  - RUN apt ~ : curl 기능 추가 및 불필요한 패키지 삭제 (과제 요구사항 + 저장공간 관리)
  - RUN useradd ~ : 커스텀 이미지용 기본 사용자를 생성해 일반적인 상황에서 권한 제한 (과제 요구사항)
  - ENV ~ : 환경변수 생성 (과제 요구사항)
  - WORKDIR ~ : 작업용 전용 디렉토리 생성 (혼선 방지 및 탐색 용이)
  - USER ~ : useradd로 추가한 사용자를 주체로 설정
  - CMD : 도커 컨테이너 실행 시 기본값으로 터미널 진입 지정 (편의성)

### 7. 포트 매핑 및 접속증거

```bash
docker run -d --name nginx-test -p 8080:80 nginx:stable      // 컨테이너 실행(백그라운드 실행)
docker ps                                                    // 실행 여부 확인
curl -I http://localhost:8080                                // 네트워크 응답확인
curl http://localhost:8080                                   // 웹페이지 HTML 내용 확인
```
(이미지 첨부)

- 체크리스트
  - 브라우저 접속 화면 / curl 응답 [o]
 
### 8. 도커 볼륨 영속성 검증

```bash
docker volume create practice-volume                                                          //도커 볼륨 생성
docker volume ls                                                                              //볼륨 리스트 확인
docker volume inspect practice-volume                                                         //볼륨 세부정보 확인
docker run -it --name volume-test-a --mount source=practice-volume,target=/data my-ubuntu:v1  //컨테이너 생성
```
(이미지 첨부)

- 명령어 해석
  - --mount : 저장 공간 연결 설정
  - source= : 연결할 볼륨
  - target= : 연결 위치, 해당 디렉토리 및 하위에 작업된 결과가 볼륨에 저장됨.

```bash
docker exec -u root volume-test-a sh -c 'echo "Docker volume persistence test" > /data/test.txt'  //루트 권한으로 출력 데이터를 test.txt에 저장
docker exec volume-test-a cat /data/test.txt                                                      //저장된 텍스트파일 확인
docker stop volume-test-a
docekr rm volume-test-a
docker ps -a                                                                                      //컨테이너 삭제 및 확인
docker volume ls                                                                                  //볼륨 리스트 확인(삭제 안됨)
docker run -it --name volume-test-b --mount source=practice-volume,target=/data my-ubuntu:v1      //같은 볼륨에 연결된 새로운 컨테이너 생성
cat /data/test.txt                                                                                //볼륨에 저장된 텍스트파일 확인
```

(이미지 첨부)

- 결과
  - 같은 볼륨에 연결한 새로운 컨테이너가 삭제된 컨테이너에서 작업한 결과물을 확인 할 수 있음. (=데이터의 영속성)
 
- 체크리스트
  - 도커 볼륨 생성 후 연결 [o]
  - 컨테이너 삭제 전/후 데이터 확인 [o]
  - 기술 문서에 절차 포함 [o]

### 9. Git 설정 및 GitHub 연동

```bash
git --version
git config --global user.name "(사용자 명)"
git config --global user.email "(사용자 이메일)"
git config --global init.defaultBranch main
git config --list
```

- 진행 내용
- git 사용자 정보 설정 및 기본 브랜치 설정
- 설정 결과 확인

```bash
sudo apt update                              // 관리자 권한으로 Ubuntu 관리도구로 패키지 목록 및 정보 갱신
(터미널 실행 시 입력한 비밀번호 입력)
sudo apt install -y gh                       // 깃헙 CLI 패키지 설치
gh --version                                 // 설치 확인
gh auth login                                // 계정 인증절차 시작
//선택지 GitHub.com > HTTPS > Y > Login with a webbrowser
//웹브라우저 오픈 실패 시, 브라우저로 기기 인증 페이지(https://github.com/login/device/select_account) 접속 후 터미널에 출력된 one-time code 입력 > 이후 연동과정 진행
gh auth status                               // 인증정보 확인
```

```bash
cd ~                                                                  // 초기 위치 이동
pwd
ls                                                                    // 목록 확인 (기존 작업 디렉터리 확인)
git clone https://github/(사용자명)/(저장소명).git assignment1-git    // 저장소 복제
cd ~/assignment1-git
ls -la
git status
git remote -v                                                        // 저장소 연결 확인
```

```bash
git branch --show-current                                            // 브랜치 명
git pul --ff-only                                                    // 변경사항 가져오기
echo "GitHub connection test" > git-test.txt
cat git-test.txt
git status                                                           // 상태 확인
git add git-test.txt                                                 // 작성한 파일 스테이징 영역에 추가
git status
git commit -m "test: verify GitHub connection"                       // 커밋 설정
git log -1 --oneline                                                 // 로그 확인
git push -u origin HEAD                                              // 깃헙에 업로드
```
