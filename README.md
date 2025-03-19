### **🚀 프로젝트 README.md (Docker 기반 Spring Boot 배포)**
📌 **이 README는 Docker를 활용하여 Spring Boot 애플리케이션을 경량화하고, 배포하는 방법을 설명합니다.**  
📌 **JDK 대신 JRE 이미지를 사용하여 컨테이너 크기를 최적화하는 것이 핵심 포인트입니다.**  

---

## **📌 프로젝트 개요**
이 프로젝트는 **Spring Boot 기반 애플리케이션**을 **Docker 컨테이너**에서 실행하기 위한 환경을 구성합니다.  
- 🚀 **경량화된 JRE 기반 Docker 이미지 (`eclipse-temurin:17-jre-alpine`) 사용**
- 🚀 **컨테이너 내에서 Java 애플리케이션(`.jar`) 실행만 수행 (JDK 불필요)**
- 🚀 **최적화된 컨테이너 크기로 배포 속도 향상**

---

## **📌 Dockerfile 설계 과정**
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

---

## **📌 Docker 이미지 빌드 및 실행**
### **1️⃣ Docker 이미지 빌드**
```bash
docker build -t doomchit/myspring:1.0 .
```
✅ **이 명령어는 `doomchit/myspring:1.0`이라는 태그를 가진 이미지를 생성합니다.**

---

### **2️⃣ Docker Hub에 업로드**
**1. Docker Hub 로그인**
```bash
docker login
```
**2. 이미지 태그 설정 (`Docker Hub`에 업로드할 이름으로 변경)**
```bash
docker tag doomchit/myspring:1.0 your-dockerhub-username/myspring:1.0
```
**3. Docker Hub에 이미지 업로드**
```bash
docker push your-dockerhub-username/myspring:1.0
```
✅ 이제 `your-dockerhub-username/myspring:1.0`으로 어디서든 `pull` 가능!

---

### **3️⃣ Docker Hub에서 이미지 다운로드 (다른 환경에서 실행)**
```bash
docker pull your-dockerhub-username/myspring:1.0
```
✅ **Docker Hub에서 이미지를 내려받아 사용할 수 있음!**

---

### **4️⃣ 컨테이너 실행**
#### ✅ **(추천) 포트 매핑하여 실행**
```bash
docker run -p 8080:8080 your-dockerhub-username/myspring:1.0
```
✅ **포트 8080을 매핑하여 `http://localhost:8080`에서 접속 가능!** 🚀

#### ❌ **(비추천) 포트 매핑 없이 실행**
```bash
docker run your-dockerhub-username/myspring:1.0
```
🚨 **포트 매핑을 설정하지 않으면 외부에서 접근 불가능! (`http://localhost:8080` 접속 불가)**

---

## **⚠️ 트러블슈팅 (Troubleshooting)**
### **Q1: Docker 컨테이너 실행 후 브라우저에서 접속이 안 됩니다.**
> `docker run doomchit/myspring:1.0`으로 실행하면 **브라우저에서 접속할 수 없음**.  
> **`-p 8080:8080` 옵션을 추가해야 정상적으로 접속 가능!**

#### ✅ **이유**
- **Docker 컨테이너 내부 네트워크는 기본적으로 `localhost`에 갇혀 있음.**
- `-p 8080:8080` 옵션이 없으면 **컨테이너 내부에서만 포트 8080이 열리고, 호스트에서 접근 불가능!**
- `-p` 옵션을 사용해야 컨테이너의 8080 포트를 호스트의 8080 포트와 연결할 수 있음.

#### ✅ **올바른 실행 방법**
```bash
docker run -p 8080:8080 doomchit/myspring:1.0
```
✅ **이제 `http://localhost:8080`에서 애플리케이션에 접근 가능!**

---

## **📌 결론**
- ✅ **JDK 없이 JRE만 포함된 경량화된 Docker 이미지 사용 (`eclipse-temurin:17-jre-alpine`)**
- ✅ **포트 8080을 사용하여 Spring Boot 실행**
- ✅ **Docker Hub에 이미지를 업로드하고 다른 환경에서도 실행 가능**
- ✅ **`docker run -p 8080:8080`으로 포트 매핑을 설정해야 정상 접속 가능!**

🚀 **이제 가벼운 컨테이너로 Spring Boot 애플리케이션을 배포할 수 있습니다!** 🎯  
