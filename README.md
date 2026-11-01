# CodysseyMission1

# 과제 명 : 내 컴퓨터에 개발자용 '작업실' 꾸미기

## 과제 목표

- 절대 경로와 상대 경로의 차이를 예시를 들어 설명할 수 있다.
  - 절대 경로: 표기된 경로의 맨 앞이 '/'(= 루트 디렉토리)로 시작되는 경로.
  - 상대 경로: 현재 작업중인 디렉토리를 기준으로 위치를 지정하는 경로. 현재 위치가 어디인지 유의해야한다.
- 파일 권한의 의미(r/w/x)와 755, 644 같은 표기가 어떤 규칙으로 해석되는지 설명할 수 있다.
  - r : 읽기 권한, 파일 내부 항목의 이름 조회, 정수 표기시 4
  - w : 쓰기 권한, 파일 내용 변경 등의 작업에 사용, 정수 표기시 3
  - x : 실행 권한, 프로그램이나 스크립트를 실행하는 권한, 정수 표기시 1
  - 생성 및 삭제는 w,x 두가지 권한을 동시에 요구 / 경로 탐색에는 대상 파일의 중간경로의 실행권한이 필요.
  - 권한의 정수표기는 각 그룹 (소유자, 그룹, 기타 사용자)의 권한을 합산하여 표기한 것
  - ex 755 : 소유자는 rwx, 그룹은 rx, 기타 사용자는 rx 권한을 인정함.
- 기존 Dockerfile을 기반으로 “커스텀 이미지”를 만들 수 있다.
- 포트 매핑이 필요한 이유를 설명할 수 있다.
  - 네임스페이스로 분리된 네트워크를 포트 매핑을 통해 경로를 설정해 접근할 수 있다.
- Docker 볼륨(영속 데이터)을 설명할 수 있다.
  - 컨테이너와 분리된 쓰기 공간으로, 컨테이너의 수명주기와 관계없이 데이터를 보존함.
  - 볼륨을 제거하기 전까지는 데이터가 지속되며, 컨테이너에 연결해 사용 가능하다.
- Git과 GitHub의 역할 차이(로컬 버전관리 vs 원격 협업 플랫폼)를 설명할 수 있다.
  - Git : 파일의 변경 이력을 관리하는 분산 버전 관리 시스템. 로컬에서 버전 관리가 가능.
  - GitHub : Git 저장소를 원격으로 호스팅, 저장소 공유 및 협업 기능을 제공하는 플랫폼.
  - Git으로 관리한 이력을 GitHub에 반영하고, `clone`,`fetch`,`pull` 등을 통해 원격저장소의 데이터를 로컬로 가져와 작업 할 수 있다.

## 개발 환경

- 호스트 OS: Windows 11
- Linux 환경: WSL2
- 터미널: Ubuntu 콘솔(WSL)
- 쉘: Bash

## 디렉토리 구조

본 레포지토리는 개발환경 구축 과제의 실습 과정과
실행 결과, 트러블슈팅 내역을 기록하기 위해 구성하였다.

- `README.md` : 챕터별 실습 내용
- `작업내용(new)/` : 실습 결과 이미지
- `트러블 슈팅/`: 오류 및 해결 과정/이미지
- `git-test` : GitHub 연동 과제 결과물

## 작업 내용

### 1. 터미널 로그 조작 기록

- 우분투 설치 중 오류 발생 : [트러블 슈팅 내역](<트러블 슈팅/트러블 슈팅 내역.md##1.-우분투-실행-오류>)

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

![실습 내용](<작업내용(new)/2. 터미널 조작 로그 기록.png>)

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

![실습 내용](<작업내용(new)/3. 권한 실습 및 증거기록1.png>)
![실습 내용](<작업내용(new)/3. 권한 실습 및 증거기록2.png>)

- 체크리스트
  - 권한 확인/변경 명령 수행 [o]
  - 변경 전/후 비교 [o]
  - 변경 수량 : 디렉토리 1개, 파일 1개
 
### 3. Docker 설치 및 기본 점검

- 도커 설치 중 오류 발생 : [트러블 슈팅 내역](<트러블 슈팅/트러블 슈팅 내역.md##2.-Docker-실행-오류>)

```bash
docker --version
docker info
```

![실습 내용](<작업내용(new)/4. 도커 설치 및 기본 점검.png>)
![실습 내용](<작업내용(new)/4. 도커 설치 및 기본 점검2.png>)

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

![실습 내용](<작업내용(new)/5. 도커 기본 운영 명령 수행.png>)

- 체크리스트
  - 이미지 명령 [o]
  - 컨테이너 명령 [o]
  - 운영 명령 [o]

### 5. 컨테이너 실행 실습

```bash
docker run hello-world    //hello world 명령어
docker ps -a              //정상실행 확인
```

![실습 내용](<작업내용(new)/6. 컨테이너 실행 실습 1.png>)

```bash
docker ps -a
docker run -it --name ubuntu ubuntu:24.04 bash      //컨테이너 실행
pwd                                                 //현재 위치 확인(컨테이너 내부)
ls
echo "Hello World!"                                 //간단한 명령어 실행
```

![실습 내용](<작업내용(new)/6. 컨테이너 실행 실습 2.png>)

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

![실습 내용](<작업내용(new)/6. 컨테이너 실행 실습 3.png>)
- attach로 접속된 컨테이너는 exit시 종료됨

```bash
docker start ubuntu                                 //컨테이너 재실행
docekr exec -it ubuntu bash                         //컨테이너 접속(exec)
echo "Executed new Bash"
exit                                                //컨테이너 탈출
docker ps                                           //컨테이너 상태 확인
```

![실습 내용](<작업내용(new)/6. 컨테이너 실행 실습 4.png>)

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

RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    rm -rf /var/lib/apt/lists/*                              // Ubuntu 패키지 목록 및 관련 정보 갱신, curl 프로그램 설치, 불필요한 패키지 제거

RUN useradd -m -s /bin/bash student                          // 사용자 생성 (제3자 권한 제한용)

ENV APP_ENV=practice                                         // 환경변수 생성 (기능 확인용)

WORKDIR /home/student                                        // 이후 명령어의 기본 작업 디렉토리 설정

USER student                                                 // 생성한 사용자를 실행 주체로 지정

CMD ["bash"]                                                 // 컨테이너 실행 시 기본값 지정

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

![실습 내용](<작업내용(new)/7. 커스텀 이미지 1.png>)
![실습 내용](<작업내용(new)/7. 커스텀 이미지 2.png>)

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

![실습 내용](<작업내용(new)/8. 포트 접속 증거(curl1).png>)
![실습 내용](<작업내용(new)/8. 포트 접속 증거(curl2).png>)
![실습 내용](<작업내용(new)/8. 포트 접속 증거.png>)

```bash
docker rm -f nginx-test                                      // 기존 컨테이너 삭제
docekr run -d --name nginx-newtest -p 8080:80 nginx:stable   // 동일한 설정의 컨테이너 생성
docker port nginx-test                                       // 포트 매핑 확인
curl -I http://localhost:8080                                // 응답 재검증
```

![실습 내용](<작업내용(new)/8. 포트 재구현 증거.png>)

- 작동 원리
  - 컨테이너는 일반적인 Docker 브리지 네트워크에서 별도의 네임 스페이스를 사용
    - 즉, 네임 스페이스로 인해 호스트와 컨테이너의 네트워크 환경이 분리됨.
  - 사용한 명령어 중 -p는 호스트의 포트로 들어온 요청을 컨테이너의 지정 포트로 전달하도록 설정함.(포트 포워딩)
  - 따라서, 네임스페이스는 네트워크의 분리 / 포트 포워딩은 접근 경로를 제공한다는 목적을 가진다.
 
```bash
docker run -d --name nginx-conflict -p 8080:80 nginx:stable                  // 동일한 호스트 포트를 사용하는 컨테이너 생성
docker ps
docker port nginx-newtest                                                    // 기존 컨테이너의 포트 매핑정보 확인
sudo ss -ltnp '( sport = :8080 )'                                            // 호스트 포트 점유 확인
docker rm -f nginx-conflict                                                  // 충돌한 컨테이너 제거
docker run -d --name nginx-conflict -p 8081:80 nginx:stable                  // 다른 호스트 포트를 사용하는 컨테이너 생성
docker ps -a
curl -I http://localhost:8081                                                // 컨테이너 확인 및 응답 확인
```

![실습 내용](<작업내용(new)/8. 포트 충돌.png>)

- 동일한 호스트 포트를 사용해 충돌이 발생한 상황

![실습 내용](<작업내용(new)/8. 포트 충돌2.png>)

- 다른 호스트 포트를 설정 후 정상작동 확인

- 체크리스트
  - 브라우저 접속 화면 / curl 응답 [o]
 
### 8. 도커 볼륨 영속성 검증

```bash
docker volume create practice-volume                                                          //도커 볼륨 생성
docker volume ls                                                                              //볼륨 리스트 확인
docker volume inspect practice-volume                                                         //볼륨 세부정보 확인
docker run -it --name volume-test-a --mount source=practice-volume,target=/data my-ubuntu:v1  //컨테이너 생성
```
![실습 내용](<작업내용(new)/9. 볼륨 영속성 증명.png>)

- 명령어 해석
  - --mount : 저장 공간 연결 설정
  - source= : 연결할 볼륨
  - target= : 연결 위치, 해당 디렉토리 및 하위에 작업된 결과가 볼륨에 저장됨.

```bash
docker exec -u root volume-test-a sh -c 'echo "Docker volume persistence test" > /data/test.txt'  //루트 권한으로 출력 데이터를 test.txt에 저장
docker exec volume-test-a cat /data/test.txt                                                      //저장된 텍스트파일 확인
docker stop volume-test-a
docker rm volume-test-a
docker ps -a                                                                                      //컨테이너 삭제 및 확인
docker volume ls                                                                                  //볼륨 리스트 확인(삭제 안됨)
docker run -it --name volume-test-b --mount source=practice-volume,target=/data my-ubuntu:v1      //같은 볼륨에 연결된 새로운 컨테이너 생성
cat /data/test.txt                                                                                //볼륨에 저장된 텍스트파일 확인
```

![실습 내용](<작업내용(new)/9. 볼륨 영속성 증명2.png>)
![실습 내용](<작업내용(new)/9. 볼륨 영속성 증명3.png>)

- 결과
  - 같은 볼륨에 연결한 새로운 컨테이너가 삭제된 컨테이너에서 작업한 결과물을 확인 할 수 있음. (=데이터의 영속성)
  - 동일한 볼륨을 새 컨테이너에 연결해 기존 데이터에 재접속 가능. (접근 재현성)

```bash
mkdir -p ~/docker-backups
docker run --rm \
  --mount source=practice-volume,target=/data,readonly \
  --mount type=bind,source="$HOME/docker-backups",target=/backup \      //파일 백업(바인드 마운트)
  ubuntu:24.04 \
  tar -czf /backup/practice-volume.tar.gz -C /data .                    
ls -lh ~/docker-backups/                                                //백업파일 확인
tar -tzf ~/docker-backups/practice-volume.tar.gz                        //압축파일 내용 확인
docker volume create practice-volume-restored                           //백업용 볼륨 생성
docker run --rm \
  --mount source=practice-volume-restored,target=/data \
  --mount type=bind,source="$HOME/docker-backups",target=/backup,readonly \   //파일 복원(바인드)
  ubuntu:24.04 \
  tar -xzf /backup/practice-volume.tar.gz -C /data                      
docker run --rm \
  --mount source=practice-volume-restored,target=/data \
  ubuntu:24.04 cat /data/test.txt                                       //백업파일 확인
```

![실습 내용](<작업내용(new)/9. 바인드 백업.png>)

- --rm : 실행 후 자동 제거되는 옵션
- tar -czf : gzip 압축 파일 생성
- C /data . : /data 내부 전체 내용 압축

- 바인드와 볼륨의 차이
  - 바인드 : 호스트가 직접 경로를 지정해야하나, 호스트가 컨테이너를 거치지 않고 직접 수정 가능
  - 볼륨 : Docker가 경로를 관리해 경로 지정이 간편하나, 접근성이 바인드에 비해 떨어짐.
  - 따라서 해당 데이터의 관리 주체가 누가(호스트 or 컨테이너 프로그램) 되느냐에 따라 적절한 방식을 선택해야한다.

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

![실습 내용](<작업내용(new)/10. git 연동1.png>)
![실습 내용](<작업내용(new)/10. git 연동2.png>)

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

![실습 내용](<작업내용(new)/10. git 연동3.png>)

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

![실습 내용](<작업내용(new)/10. git 연동4-1.png>)
![실습 내용](<작업내용(new)/10. git 연동4-2.png>)
![실습 내용](<작업내용(new)/10. git 연동5.png>)
![실습 내용](<작업내용(new)/10. git 연동6.png>)
