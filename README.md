<img src="https://capsule-render.vercel.app/api?type=waving&color=000000&height=300&section=header&text=Minimizing-Docker-Image-Size&fontSize=50&fontColor=FFFFFF&animation=fadeIn&width=1200" width="1200" />


## **📌 Overview**
이 프로젝트는 **Spring Boot 애플리케이션**을 **최적화된 Docker 이미지로 패키징**하여 실행하기 위한 환경을 구성합니다.

✅ 불필요한 패키지를 제거하고 가벼운 환경을 유지하여 최적화된 컨테이너를 제공합니다.

✅ JDK 없이 JRE만 포함하여 컨테이너 크기를 최소화하고 배포 속도를 향상시킵니다.

✅ 최적화된 Dockerfile을 활용하여 실행 속도와 리소스 사용량을 효율적으로 관리합니다.

<br>

## **📌 Contributors**
<br>

|<img src="https://github.com/DoomchitYJ.png" width="220" />|<img src="https://github.com/Jeongho427.png" width="220" />|
|:-:|:-:|
|박영진<br/>[@DoomchitYJ](https://github.com/DoomchitYJ)|박정호<br/>[@Jeongho427](https://github.com/Jeongho427)|

<br>

## **📌 Optimization Strategy**

🚀 경량화된 JRE 기반 Docker 이미지 (eclipse-temurin:17-jre-alpine) 사용 → 불필요한 개발 도구 제거

🚀 최소한의 패키지만 포함하여 컨테이너 크기 절감 → 빌드 및 배포 속도 향상

🚀 JDK 대신 JRE 사용하여 Java 실행 환경만 유지 → 메모리 사용량 감소

✅ 결과적으로, 더 작은 이미지 크기와 더 빠른 배포 속도를 제공하며, 운영 비용도 절감할 수 있음! 🚀

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

>https://hub.docker.com/r/doodoomchit/myspring/tags

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

### 컨테이너 실행 확인

![image](https://github.com/user-attachments/assets/2f098a50-ca0e-40b9-9380-2c6bab10036d)


✅ **포트 8080을 매핑하여 `http://localhost:8080`에서 접속 가능!** 🚀

### **웹 접속 성공 화면**

![image](https://github.com/user-attachments/assets/ebf1201c-a712-4a0e-b8b0-ed262b605988)


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
