<img src="https://capsule-render.vercel.app/api?type=waving&color=000000&height=300&section=header&text=Minimizing-Docker-Image-Size&fontSize=50&fontColor=FFFFFF&animation=fadeIn&width=1200" width="1200" />

<br>

## **📌 Contributors**
<br>

|<img src="https://github.com/DoomchitYJ.png" width="220" />|<img src="https://github.com/Jeongho427.png" width="220" />|
|:-:|:-:|
|박영진<br/>[@DoomchitYJ](https://github.com/DoomchitYJ)|박정호<br/>[@Jeongho427](https://github.com/Jeongho427)|

<br>

## **📌 Overview**
이 프로젝트는 **Spring Boot 기반 애플리케이션**을 **Docker 컨테이너**에서 실행하기 위한 환경을 구성합니다.  
- 🚀 **경량화된 JRE 기반 Docker 이미지 (`eclipse-temurin:17-jre-alpine`) 사용**
- 🚀 **컨테이너 내에서 Java 애플리케이션(`.jar`) 실행만 수행 (JDK 불필요)**
- 🚀 **최적화된 컨테이너 크기로 배포 속도 향상**

<br>

## **📌 Dockerfile**
### 1️⃣ **경량화된 JRE 기반 이미지 사용**
- **JDK 대신 JRE를 사용하여 컨테이너 크기 감소**
- `eclipse-temurin:17-jre-alpine`은 **OpenJDK 기반의 경량 Alpine Linux 버전**으로 크기가 작고 실행 속도가 빠름.

### 2️⃣ **작업 디렉토리 설정**
```dockerfile
WORKDIR /app
```
- 모든 애플리케이션 파일을 `/app` 디렉토리에서 관리.

### 3️⃣ **파일 복사**
```dockerfile
COPY . /app
```
- 현재 디렉토리의 모든 파일을 컨테이너 내부 `/app`으로 복사.

### 4️⃣ **JAR 실행 명령 설정**
```dockerfile
CMD ["java", "-jar", "step01_basic-0.0.1-SNAPSHOT.jar", "--server.port=8080"]
```
- **포트 8080에서 실행되도록 설정**
- **JDK 없이 JRE만으로 `.jar` 실행 가능** (JRE는 `java -jar` 실행만 지원)

<br>

## **📌 Docker Image Build & Execute**
### **1️⃣ Docker Image Build**
```bash
docker build -t 본인DockerHubID/이미지이름:1.0 .
```
✅ **이 명령어는 `본인DockerHubID/이미지이름:1.0`이라는 태그를 가진 이미지를 생성합니다.**


### **2️⃣ Upload on Docker Hub**
**1. Docker Hub 로그인**
```bash
docker login
```
**2. Docker Hub에 이미지 업로드**
```bash
docker push 본인DockerHubID/이미지이름:1.0
```
✅ 이제 `본인DockerHubID/이미지이름:1.0`으로 어디서든 `pull` 가능!


### **3️⃣ Download Image from Docker Hub (on other PC)**
```bash
docker pull 본인DockerHubID/이미지이름:1.0
```
✅ **Docker Hub에서 이미지를 내려받아 사용할 수 있음!**


### **4️⃣ Execute Container**
#### ✅ **포트 매핑하여 실행**
```bash
docker run -p 8080:8080 본인DockerHubID/이미지이름:1.0
```
✅ **포트 8080을 매핑하여 `http://localhost:8080`에서 접속 가능!** 🚀

<br>

## **⚠️ Troubleshooting**
### **Q1: Docker 컨테이너 실행 후 브라우저에서 접속이 안 됩니다.**
> `docker run 본인DockerHubID/이미지이름:1.0`으로 실행하면 **브라우저에서 접속할 수 없음**.  
> **`-p 8080:8080` 옵션을 추가해야 정상적으로 접속 가능!**

#### ✅ **원인**
- **Docker 컨테이너 내부 네트워크는 기본적으로 `localhost`에 갇혀 있음.**
- `-p 8080:8080` 옵션이 없으면 **컨테이너 내부에서만 포트 8080이 열리고, 호스트에서 접근 불가능!**
- `-p` 옵션을 사용해야 컨테이너의 8080 포트를 호스트의 8080 포트와 연결할 수 있음.

#### ✅ **올바른 실행 방법**
```bash
docker run -p 8080:8080 본인DockerHubID/이미지이름:1.0
```
✅ **이제 `http://localhost:8080`에서 애플리케이션에 접근 가능!**

<br>

## **📌 Conclusion**
- ✅ **JDK 없이 JRE만 포함된 경량화된 Docker 이미지 사용 (`eclipse-temurin:17-jre-alpine`)**
- ✅ **포트 8080을 사용하여 Spring Boot 실행**
- ✅ **Docker Hub에 이미지를 업로드하고 다른 환경에서도 실행 가능**
- ✅ **`docker run -p 8080:8080`으로 포트 매핑을 설정해야 정상 접속 가능!**
