# Scalability vs Availability

확장성(Scalability): 어플리케이션이나 시스템이 늘어나는 부하, 사용자 수, 또는 데이터양을 효율적으로 처리할 수 있는 능력.

- 수직적 확장(Scaling Up): 하드웨어의 성능을 향상시켜 처리 능력을 증가시키는 방법.

- 수평적 확장(Scaling Out): 더 많은 노드를 추가하여 처리 능력을 분산시키는 방법.

Availability(가용성): 시스템이 필요할 때 사용할 수 있는 정도.

- 장애 복구 계획, 지역 분산, 중복 구축 등을 이용할 수 있다.

# ELB

![ELB](../pictures/ELB.png)

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
