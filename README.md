# AWS Infra

## 클라우드 인프라 구축
AWS 서비스를 활용하여 확장 가능하고 안정적인 웹 서비스 운영이 가능하도록 인프라를 구축한 프로젝트 입니다.

### 구축 환경
- AWS VPC, EC2, ELB, RDS
- Bastion Host
- NAT Instance
- Amazon Linux
- Kotlin 1.6

## 인프라 구성도
![구성도](./img/구성도.png)

## 구축 과정

### VPC 구성

CIDR 블록으로 사용할 IP 주소의 범위를 설정해서 두 개의 가용 영역에 VPC를 퍼블릭 서브넷과 프라이빗 서브넷을 구축했습니다. 인터넷 게이트웨이를 VPC에 배치해서 퍼블릭 서브넷의 라우팅 테이블은 인터넷 게이트웨이를 가리켜 외부 인터넷에서 트래픽을 주고 받을 수 있습니다. 보안 그룹과 네트워크ACL 설정으로 허용시킬 트래픽을 설정합니다. NAT Gateway 대신 NAT Instance를 생성하여 사용함으로써 비용절감을 하고, 내부에서 외부로 통신할 수 있도록 구성했습니다. 

- [VPC - 보안 네트워크 구축](./서버-구축/AWS_VPC/VPC%20보안%20네트워크%20구축.md)
- [보안그룹과 네트워크ACL](./서버-구축/AWS_VPC/보안%20그룹과%20네트워크ACL.md)
- [NAT Instance - 비용절감](./서버-구축/AWS_VPC/NAT%20Instance%20-%20비용절감.md)


### EC2 구성

퍼블릭 서브넷에 배치된 EC2 서비스 서버를 배포를 통해 누구든지 서비스를 제공 받을 수 있게 구성했습니다. 하지만 퍼블릭 서브넷은 보안적으로 문제 발생 가능성이 있기 때문에 Bastion Host를 도입하였습니다. 서비스 서버를 프라이빗 서브넷으로 이전시키고 Bastion Host를 사용해 비인가된 사용자가 외부에서 내부로 접근할 수 없도록 보안을 강화했습니다.

- [EC2 - 가상서버](./서버-구축/AWS_EC2/EC2%20-%20가상서버.md)
- [Bastion Host - 보안강화](./서버-구축/AWS_EC2/Bastion%20Host%20-%20보안강화.md)

### ELB 구성

서버의 직접적인 접근을 막는 보안 강화의 장점은 있었지만 서비스에 필요한 트래픽도 접근할 수 없게 되는 문제가 발생했습니다. 애플리케이션 로드밸런서를 버플릭 서브넷에 배치하고 외부에서 들어오는 HTTP 트래픽을 받아 프라이빗 서브넷의 EC2로 전달해 다시 서비스를 제공할 수 있도록 만들었습니다. ELB를 사용하게되면 EC2 인스턴스가 수평으로 확장되어 여러 트래픽을 로드밸런싱 해줄 수 있는데, 수동으로는 설정을 해줄 수 없어 ASG를 사용해 해결했습니다. ASG를 사용하기 위해 IAM를 만들어 초기 환경을 미리 구성했습니다. 시작 템플릿을 기반으로 오토 스케일링 그룹을 생성해 로드밸런서와 통합하여 로드밸런싱 기능을 구현했습니다. 또한, 단순 조정정책을 사용해 가용성과 확장성을 확보했습니다.

- [ELB - 고가용성을 위한 로드밸런싱](./서버-구축/AWS_ELB/Elastic%20Load%20Balancer%20-%20고가용성을%20위한%20로드밸런싱.md)
- [ASG - 서비스 자동 확장](./서버-구축/AWS_ELB/Auto%20Scaling%20Group%20-%20서비스%20자동%20확장.md)

### RDS 구성

RDS는 AWS에서 제공하는 관계형 데이터베이스 서비스입니다. AWS에서 운영 부분을 관리해줘서 중단 없이 스토리지를 확장하거나 백업, 마이그레이션을 쉽게 할 수 있습니다. 또한, Multi-AZ 배포, 읽기 전용 복제본, RDS Proxy를 사용해서 데이터베이스의 가용성과 확장성을 확보했습니다.

- [RDS - 관리형 관계형 데이터베이스](./서버-구축/AWS_RDS/RDS%20-%20관리형%20관계형%20데이터베이스.md)
- [RDS - IAM 사용](./서버-구축/AWS_RDS/RDS%20-%20IAM%20사용.md)


### 해결할 문제점

데이터베이스와 파일 저장이 중앙에서 관리되지 않아 각각 인스턴스에 저장되고 일관성이 없는 문제점이 발생했습니다. 추후, S3를 도입함으로 사용자가 저장하는 만큼 계속해서 확장되고, 보안 설정을 통해 안전하게 관리하도록 변경하고 CloudFront를 사용해 조금 더 빠른 서비스를 제공하게 되도록 변경해보겠습니다.