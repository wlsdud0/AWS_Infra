# Bastion Host - 보안 강화

### Bastion Host

- 서버보다 앞에 위치해 직접적인 접근을 막는 역할을 하는 인스턴스
- 서버을 프라이빗 서브넷에 배치해서 직접적인 접근을 차단
- 배스천 호스트를 퍼블릭 서브넷에 배치
- 배스천 호스트를 통해 프라이빗 서브넷의 서버로 접근

### Bastion Host 다이어그램

<img src="../../img/Bastion Host 다이어그램.png" alt="가용영역" width="300" height="150"> 

- goopang서버와 Bastion Host 각각 보안 그룹을 가지고 있어야 함
- Bastion Host는 SSH 포트만 열어놓고 goopang서버는 SSH 포트를 Bastion Host의 보안 그룹을 대상으로 열어둠