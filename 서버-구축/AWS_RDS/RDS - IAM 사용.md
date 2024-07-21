# RDS - IAM 사용

### 사용자 데이터 - 인스턴스 연결

```bash
#!/bin/bash

#Git 레포지토리 클론 및 브랜치로 이동
git clone -b monolithic_rds https://github.com/wlsdud0/aws_infra.git

#폴더 이동
cd aws-operation-prac

# Gradle을 이용한 Spring Boot 프로젝트 빌드 후 빌드된 Spring Boot 애플리케이션 실행
./gradlew build
sudo java -jar build/libs/aws-msa-monolithic-prac-0.1.jar \
--spring.datasource.url=jdbc:postgresql://[엔드포인트:포트/데이터베이스] \
--spring.datasource.username=[유저이름] \
--spring.datasource.password=[비밀번호]
```

### 사용자 데이터 - RDS 읽기 복제본 - 읽기 트래픽 분산

```bash
#!/bin/bash

#Git 레포지토리 클론 및 브랜치로 이동
git clone -b monolithic_rds_read_replica https://github.com/wlsdud0/aws_infra.git

#폴더 이동
cd aws-operation-prac

#Gradle을 이용한 Spring Boot 프로젝트 빌드 후 빌드된 Spring Boot 애플리케이션 실행
./gradlew build

# 읽기 전용으로 사용할 데이터베이스 엔드포인트:포트/DB이름, 유저이름, 비밀번호
# 쓰기 전용으로 사용할 데이터베이스 엔드포인트:포트/DB이름, 유저이름, 비밀번호
java -jar build/libs/aws-msa-monolithic-prac-0.1.jar \
--spring.datasource.write.jdbc-url=jdbc:postgresql://[엔드포인트:포트:데이터베이스] \
--spring.datasource.write.username=[유저이름] \
--spring.datasource.write.password=[비밀번호] \
--spring.datasource.read.jdbc-url=jdbc:postgresql://[읽기 전용 엔드포인트:포트:데이터베이스]  \
--spring.datasource.read.username=[유저이름] \
--spring.datasource.read.password=[비밀번호]
```

### 사용자 데이터 - RDS Proxy - 연결 최적화

```bash
#!/bin/bash

#Git 레포지토리 클론 및 브랜치로 이동
git clone -b monolithic_rds_read_replica https://github.com/wlsdud0/aws_infra.git

#폴더 이동
cd aws-operation-prac

#Gradle을 이용한 Spring Boot 프로젝트 빌드 후 빌드된 Spring Boot 애플리케이션 실행
./gradlew build

# RDS Proxy 엔드포인트:포트/DB이름, 유저이름, 비밀번호
# 읽기 전용 복제본 엔드포인트:포트/DB이름, 유저이름, 비밀번호
sudo java -jar build/libs/aws-msa-monolithic-prac-0.1.jar \
--spring.datasource.write.jdbc-url=jdbc:postgresql://[RDS-proxy-Endpoint]/[database-name] \
--spring.datasource.write.username=[username] \
--spring.datasource.write.password=[password] \
--spring.datasource.read.jdbc-url=jdbc:postgresql://[RDS-read-Endpoint]/[database-name] \
--spring.datasource.read.username=[username] \
--spring.datasource.read.password=[password]
```