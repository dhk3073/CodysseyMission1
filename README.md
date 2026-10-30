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
- Linux 체제 터미널 사용 이유 : chmod 및 권한 정수표기등 과제에서 Linux 체제를 전제로 한 기능 사용 요구
- 쉘: Bash

## 작업 내용

### 1-1. 현재 위치 및 목록 확인

```bash
pwd        //현재 위치 확인
ls -la     //목록 확인(숨김 파일 포함)
```
(이미지 첨부)
- . : 현재 디렉토리
- .. : 부모 디렉토리

### 1-2. 디렉토리 생성 및 이동, 이동 결과 확인

```bash
mkdir asignment1    //디렉토리 생성
ls -la              //목록 확인(디렉토리 생성여부 확인)
cd asignment1       //디렉토리 이동(생성한 디렉토리로 이동)
pwd                 //현재 위치 확인
```
(이미지 첨부)

### 2-1. 파일 생성 및 내용 입력, 확인

```bash
touch original.txt  //파일 생성
ls -l original.txt  //목록 확인(뒤에 추가한 파일명으로 확인 범위를 제한), 파일명이 없어도 생성여부는 확인 가능하나, 생성 실패시 오류 메시지 확인을 위해 범위 지정, 혹은 대량의 파일이 존재할 경우 특정 파일의 확인을 용이하게 하기 위함.
echo "Hello Linux" > original.txt  //문자열 입력 및 입력 위치 지정
echo "CLI" >> original.txt         //'>' : 내용 덮어쓰기 '>>' : 내용 추가하기
cat original.txt                   //파일 내용 확인
```
(이미지 첨부)

### 2-2 파일 복사

```bash
cp original.txt copied.txt        //파일 복사 (원본 유지)
ls -l                             //목록 확인
cat copied.txt                    //복사한 파일 확인
```
(이미지 첨부)

### 2-3 파일 이름 변경 및 이동

```bash
mv copied.txt renamed.txt        //파일 이름 변경
ls -l                            //목록 확인
mkdir backup                     //새 디렉토리 생성
mv renamed.txt backup/           //파일을 지정한 디렉토리로 이동
ls -l backup/                    //지정한 디렉토리의 목록 확인
```
(이미지 첨부)

### 2-4. 파일 및 디렉토리 삭제

```bash
rm original.txt                  //파일 삭제
rm backup/renamed.txt            //파일 삭제(상대 경로 지정)
rmdir backup                     //디렉토리 삭제
ls -la                           //목록 확인
```

### 3-1. 파일 권한 변경

```bash
touch permission.txt            //파일 생성
ls -l                           //목록 확인
chmod 600 permision.txt         //권한 부여
ls -l                           //목록 확인
chmod 644 permission.txt        //권한 부여(수정)
ls -l                           //목록 확인
```
(이미지 첨부)
* 이미지상의 일부 명령어 및 파일등에 오타가 있어 해당 명령문을 재실행함.

### 3-2. 디렉토리 권한 변경

```bash
mkdir permission_dir            //디렉토리 생성
ls -ld permission_dir           //목록 확인 : -d 를 사용해 디렉토리의 권한 확인
chmod 700 permission_dir        //권한 설정
ls -ld permission_dir           //목록 확인
chmod 755 permission_dir        //권한 설정(수정)
ls -ld permission_dir           //목록 확인
```
(이미지 첨부)


