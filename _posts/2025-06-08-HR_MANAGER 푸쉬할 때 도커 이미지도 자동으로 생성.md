---
layout: post
title: HR_MANAGER 푸쉬할 때 도커 이미지도 자동으로 생성
categories: [Docker]
comments: true
---

### 1. HR_MANGER 도커 이미지 생성 Dockerfile 스크립트 생성
```
# Use JDK 21 base image
FROM eclipse-temurin:21-jdk

# Set working directory
WORKDIR /app

# Copy the built JAR into the container
COPY target/hr_manager-1.0.0.war app.war

# Create the h2 directory
RUN mkdir -p h2

# Copy the test_db file into the h2 directory
COPY ./h2/hr_manager.mv.db ./h2/hr_manager.mv.db

# Expose application port
EXPOSE 8080

# Command to run the application
CMD ["java", "-jar", "app.war"]
```

## 스크립트 설명

- 서비스가 실행될 환경을 자바 21로 구성
- h2 파일 생성
- DB정보가 들어있는 파일 이미지에 복사
- 포트 8080으로 설정
- 이미지 실행시 CMD 명령어 실행

## 실행 조건
- 해당 스크립트는 직접 빌드해야 이미지가 생성됨
- 'docker build ~' 명령어로 빌드해야 이미지 생성

### 2. 프로젝트 PUSH 시 자동으로 도커 이미지 생성될 수 있도록 스크립트 생성
```
name: HR_MANAGER 프로젝트 빌드

on:
  push:
    branches:
      - master

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # 1. Repository Checkout
      - name: Checkout Code
        uses: actions/checkout@v3

      # 2. Set Up JDK 21
      - name: Set up JDK 21
        uses: actions/setup-java@v3
        with:
          java-version: 21
          distribution: temurin

      # 3. Cache Maven Dependencies
      - name: Cache Maven Dependencies
        uses: actions/cache@v3
        with:
          path: ~/.m2
          key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}
          restore-keys: |
            ${{ runner.os }}-maven-

      # 4. Build Maven Project
      - name: Build with Maven
        run: mvn clean package -DskipTests

      # 5. Build API Docker Image
      - name: Build API Docker Image
        run: |
          docker build \
            -f Dockerfile \
            -t ghcr.io/currychoco/hr-manager:latest .

      # 6. Push Docker Image to Docker Hub
      - name: Log in to GitHub Container Registry
        uses: docker/login-action@v2
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GHCR_TOKEN }}

      - name: Push API Docker Image
        run: docker push ghcr.io/currychoco/hr-manager:latest
```

## 스크립트 설명
- 'master' 브랜치가 push 되었을 때 아래의 스크립트 실행
- 깃허브 빌드 컨테이너에서 스크립트가 실행됨
- 1. HR_MANAGER 프로젝트를 checkout
- 2. 빌드 환경은 21로 구성
- 3. 프로젝트에 필요한 maven dependency 들을 캐시화
- 4. 프로젝트 빌드
- 5. 프로젝트 내에 있는 'Dockerfile' 스크립트를 통해 프로젝트 이미지화
- 6. 생성된 도커 이미지를 지정된 저장소에 저장
 
## 해당 스크립트 생성 후 프로젝트 PUSH 시 발생하는 이벤트
![image](https://github.com/user-attachments/assets/9c90d361-9164-4d08-8607-2e604dc2e1a8)

- 해당 명령어를 실행시키면 이미지를 pull 받을 수 있음
![image](https://github.com/user-attachments/assets/a9e14ded-82dd-4d16-b91b-77fa25e7d67f)

- 위의 명령어 싫행 후, 다운로드된 이미지 실행
![image](https://github.com/user-attachments/assets/d846c9c0-6e88-49fd-a4a1-22e611d18c0f)

- 정상적으로 실행되는 HR_MANAGER
![image](https://github.com/user-attachments/assets/d13653b3-cf27-409f-8e91-931b48ef8650)

### 후기
DB의 경우에는 뷰테이블에 컨테이너가 올라갔다 내려갔다 할 때 마다 사라지는 이슈가 있는데,
이건 원래 DB까지 같이 말아놓는 상황이 없기 때문에 특수솽황으로 인지.
이거 뷰테이블썼던 부분은 모두 join 으로 바꿔서 문제 없이 데이터 가져오게 수정해야겠음.
