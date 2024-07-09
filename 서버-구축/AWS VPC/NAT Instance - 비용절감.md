# NAT Instance - 비용절감

### NAT Instance

- NAT 인스턴스를 사용하면 프라이빗 서브넷의 리소스가 인터넷이나 온프레미스 네트워크와 같은 Virtual Private Cloud(VPC) 외부의 대상과 통신할 수 있음

### NAT Gateway VS NAT Instance

![구성도](../../img/NAT%20Gateway%20vs%20NAT%20Instance.png)

### NAT instance 명령

```bash
# iptavles 활성화: 네트워크 패킷을 필터링하고 네트워크 트래픽을 제어하는 방화벽 소프트웨어

sudo yum install iptables-services -y 
sudo systemctl enable iptables 
sudo systemctl start iptables

# IP 포워딩 활성화: 한 네트워크 인터페이스에 들어온 패킷을 다른 네트워크 인터페이스로 전다랗는 기능
# NAT instance에서는 프라이빗 네트워크의 트래픽을 받아 인터넷으로 전달하기 위해 IP 포워딩 사용

# 1.  vi 편집기를 통한 구성파일 생성
sudo vi /etc/sysctl.d/custom-ip-forwarding.conf

# 2.  구성파일에 입력할 활성화 명령어
net.ipv4.ip_forward=1

# 3. 파일 저장 후, 구성 파일 적용 명령어
sudo sysctl -p /etc/sysctl.d/custom-ip-forwarding.conf

# NAT 설정을 위해 네트워크 인터페이스 이름을 확인 명령어
netstat -i

# NAT 구성 명령어
sudo /sbin/iptables -t nat -A POSTROUTING -o 인터페이스이름 -j MASQUERADE
sudo /sbin/iptables -F FORWARD
sudo service iptables save
```

### ping-test-instence

```bash
ping 8.8.8.8

# 트래픽 유실
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
^C
--- 8.8.8.8 ping statistics ---
10 packets transmitted, 0 received, 100% packet loss, time 9362ms

# 프라이빗 서브넷의 라우팅 테이블에 NAT 인스턴스를 설정해주지 않았기 때문
```