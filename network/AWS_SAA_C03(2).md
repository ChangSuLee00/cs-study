# AWS SAA C03 / High Availability & Scalability

# Scalability vs Availability

확장성(Scalability): 어플리케이션이나 시스템이 늘어나는 부하, 사용자 수, 또는 데이터양을 효율적으로 처리할 수 있는 능력.

- 수직적 확장(Scaling Up): 하드웨어의 성능을 향상시켜 처리 능력을 증가시키는 방법.

- 수평적 확장(Scaling Out): 더 많은 노드를 추가하여 처리 능력을 분산시키는 방법.

가용성 (Availability): 시스템이 필요할 때 사용할 수 있는 정도.

- 장애 복구 계획, 지역 분산, 중복 구축 등을 이용할 수 있다.

# ELB

![ELB](./pictures/ELB.png)

Elastic Load Balancer의 약자로 AWS에서 제공하는 로드 밸런싱 서비스.

- 여러 다운스트림 인스턴스에 걸쳐 부하를 분산시킨다.

- 애플리케이션에 대한 단일 접근 지점(DNS)을 제공한다.

- 다운스트림 인스턴스의 장애를 원활하게 처리한다.

- 인스턴스에 대해 정기적인 헬스체크를 수행한다.

- 웹사이트에 SSL(HTTPS)를 제공할 수 있다.

- 쿠키를 이용한 세션 유지 기능을 강제할 수 있다.

- 여러 가용 영역에 걸친 높은 가용성을 제공한다.

- 공개 트래픽과 사설 트래픽을 분리한다.

## Classic Load Balancer

v1 구세대 로드 벨런서.

- 4계층(TCP), 7계층(HTTP, HTTPS)

- HTTP, HTTPS, TCP, SSL.

## Application Load Balancer

v2 신세대 로드 벨런서

- 7계층.

- HTTP, HTTPS, WebSocket.

- multiple targeting이 가능하다. (다른 Machine Target Group & 같은 Machine Containers)

- HTTP -> HTTPS 등의 redirect가 가능하다.

- URL 혹은 Query를 기반으로 한 라우팅이 가능하다.

- 클라이언트의 IP는 X-Forwarded-For로 들어간다.

## Network Load Balancer

v2 신세대 로드 벨런서

- 4계층.

- TCP, TLS.

- NLB는 각 AZ 당 하나의 정적 IP를 가지며, 탄력적 IP(Elastic IP) 할당을 지원한다.

- 높은 성능, TCP 또는 UDP 트래픽 처리가 필요할 때 사용한다.

## Gateway Load Balancer

VPC끼리의 통신에 사용된다.

- 3계층 - IP protocol.

- 방화벽 사용 가능.

- GENEVE protocol을 6081번 포트에서 사용한다.

- Target Group은 EC2 혹은 IP 주소이다.

- Gateway Load Balancer Endpoint를 통해 VPC간 간편한 연결을 지원한다.

## Sticky session

같은 클라이언트가 항상 로드 밸런서 뒤의 동일한 인스턴스로 리디렉션되도록 쿠키를 사용해서 스티키 세션을 구현할 수 있다.

### Application-based Cookies

- Target에 의해 생성된다.

- 쿠키의 이름은 AWSALBAPP이다.

### Duration-based Cookies

- Load Balancer에 의해 생성된다.

- 쿠키 이름은 ALB의 경우 AWSALB, CLB의 경우 AWSELB이다.

## Cross-Zone Load Balancing

각 로드 밸런서 인스턴스는 모든 가용 영역에 등록된 모든 인스턴스에 걸쳐 균등하게 트래픽을 분산시킨다.

## SSL Certificates

- 로드 밸런서는 X.509인증서(SSL/TLS 서버 인증서)를 사용하고 ACM을 통해 인증서를 관리한다.

- 다중 도메인을 지원하기 위한 인증서를 추가할 수 있다.

- 클라이언트는 SNI(Server Name Indication) 통해 접근을 하려고 하는 호스트네임을 특정할 수 있다.

## SNI

- 하나의 웹 서버에 여러 SSL 인증서를 로딩하여 다수의 웹사이트를 제공한다.

- 클라이언트가 어떤 타겟 서버로 연결할지를 지정 해주어야 한다.

- ALB 및 NLB에서만 작동한다.

## Connection Draining

로드 밸런서가 특정 인스턴스로의 새로운 요청을 중단하는 동시에, 해당 인스턴스에서 이미 처리 중인 요청을 정상적으로 완료할 수 있도록 일정 시간을 부여하는 기능이다.

- ALB와 NLB에서는 이와 유사한 기능을 하는 Deregistration Delay가 있다.

# ASG

![ASG](./pictures/ASG.png)

Auto Scaling Group의 약자로 수요에 따라 자동으로 인스턴스를 추가, 제거하는 서비스.

- Launch Template: 미리 정의된 생성될 EC2에 대한 데이터로 AMI, 인스턴스 타입, EBS, SSH Keypair 등이 있다.

- Predictive Scaling: 예측을 통해 부하를 예상하고 스케일링을 한다.

- Scaling Cooldowns: 스케일링 활동을 한 이후 추가적인 launching을 하지 않는 기간.

## Scaling

- Simple scaling policy: 특정 CloudWatch 경보 조건(예: CPU 사용률)이 충족될 때 인스턴스 수를 조정하는 기본적인 Auto Scaling 정책.

- Target tracking policy: 지정된 지표(예: CPU 사용률)가 정해진 목표값을 유지하도록 인스턴스 수를 자동으로 조정하는 지능형 Auto Scaling 정책.

- Scheduled scaling actions: 특정 시간에 맞춰 Auto Scaling 그룹의 인스턴스 수를 미리 정해진 계획에 따라 조정하는 정책.
