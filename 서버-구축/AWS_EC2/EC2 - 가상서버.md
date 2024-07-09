# EC2 - 가상서버

### EC2

Elastic Compute Cloud

- 가상서버를 호스팅할 수 있는 AWS의 서비스
- 대여한 가상머신을 EC2 인스턴스라 함
- 다른 AWS 서비스들과 통합해서 주로 사용

### EC2 특징

- 확장성과 유연성 - 언제든지 대여할 수 있고, 용량 확장이나 축소가 쉬움
- 빠른 배포 - 대여하기만 하면 AWS에서 빠르게 사용할 수 있도록 배포
- 안정성과 가용성 - 여러 가용영역과 리전에 인스턴스를 배포해 가용성 확보
- 유연한 요금체계 - CPU, 메모리, 예약, 상시 사용 등의 요금제로 비용 최적화
- 서비스 통합 - 다른 AWS서비스와 통합 쉬움

### AMI

Amazon Machine Image

- 인스턴스 생성에 필요한 OS나 소프트웨어가 포함된 템플릿
- AWS에서 미리 구성한 템플릿
- 개발자가 직접 만들어서 사용도 가능

### 인스턴스 유형

- EC2 인스턴스의 요구사항에 따라 최적화된 종류
- 범용, 컴퓨팅 최적화, 메모리 최적화, 가속화된 컴퓨팅, 스토리지 최적화, 고성능 컴퓨팅
- 서비스의 특성에 맞게 자원을 효율적으로 사용 가능

### 키페어

- EC2 인스턴스에 접속하기 위해 사용되는 공개키와 개인키의 조합
- 공개키는 EC2 인스턴스에 개인키는 사용자에 제공
- 사용자는 개인키를 통해 EC2 인스턴스에 접속

### EC2의 스토리지

- EBS - 한번에 하나의 인스턴스만 연결하는 스토리지
- EFS - 여러 인스턴스가 동시에 접근할 수 있는 관리형 파일 시스템

### EC2 다이어그램

<img src="../../img/EC2 다이어그램.png" width="300" height="200"> 

### **Git 설치 명령어**

```bash
sudo yum update
sudo yum install git
```

### **JDK 설치 명령어**

```bash
sudo yum install -y java-17-amazon-corretto-devel
```

### **Git Clone 명령어**

```bash
git clone -b monolithic_cloud git@github.com:wlsdud0/aws_infra.git
```

### **빌드 및 배포**

```bash
cd aws-operation-prac

./gradlew clean build

sudo java -jar build/libs/aws-msa-monolithic-prac-0.1.jar
```