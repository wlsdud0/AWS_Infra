# 보안그룹과 네트워크ACL

### 보안그룹

- 인스턴스 단위의 가상 방화벽
- 인바운드와 아웃바운드 트래픽을 허용 설정
- 인바운드는 외부에서 보안그룹 내부로 들어오는 트래픽,
- 아웃바운드는 보안그룹 내부에서 외부로 나가는 트래픽
- 허용 대상을 IP나 다른 보안그룹을 기준으로도 가능
- 보안그룹은 상태를 기억하는 Stateful한 성질

### 보안그룹 인바운드 규칙

<img src="../../img/보안그룹 인바운드 규칙.png" width="300" height="300"> 

### 보안그룹의 Stateful

<img src="../../img/보안그룹의 Stateful.png" width="300" height="150"> 

인바운드에서 허용된 트래픽은 허용된 상태가 유지되어 응답이 나갈때도 허용되는 특징

단, 응답으로 나가는 트래픽과 아웃바운드 트랙픽을 헷갈리면 안됨

### 네트워크ACL

Network Access Control List

- 서브넷 단위의 가상 방화벽
- 서브넷에 속한 인스턴스에게 적용
- 트래픽을 허용과 거부
- 대상을 IPv4, IPv6, CIDR으로만 지정 가능
- 네트워크 ACL은 Stateless

### 네트워크ACL의 Stateless

<img src="../../img/네트워크ACL의 Stateless.png" width="300" height="150"> 

### 보안그룹과 네트워크ACL

<img src="../../img/보안그룹과 네트워크ACL.png" width="300" height="300"> 