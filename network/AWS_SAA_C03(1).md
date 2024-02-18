# AWS SAA C03 / Access & Computing

# IAM

![IAM](./pictures/AWS/IAM.png)

IAM이란 Identity and Access Management의 약자로 AWS 리소스를 제어하는 권한을 중앙에서 관리할 수 있도록 도와주는 '글로벌' 서비스이다.

user에게만 권한을 부여하는 것이 아니라 EC2와 같은 인스턴스에도 접근 권한 등을 부여할 수 있다.

- IAM Policy: 권한을 정의하는 문서.

- IAM Role: 그러한 권한을 다른 사용자나 서비스에 임시로 할당하는 메커니즘.

- ex) aws-ec2-role를 생성-> S3-full-access-policy를 해당 role에 포함시킴 -> EC2에 aws-ec2-role 적용.

## Users

물리적 유저와 연동되며 AWS console을 위한 비밀번호를 가지고 있다.

## Policies

user나 group에 대한 권한을 부여하는 document.

- JSON 형태로 되어 있다.

- least privilege principle을 적용하여 유저에게 필요 이상의 권한을 부여하지 않는 것이 원칙이다.

## Roles

EC2나 AWS service를 위한 access 권한 관리 policy의 집합.

- EC2 Instance Roles와 같은 형태로 되어 있다.

### Trust policy

다른 AWS 계정, IAM 사용자, IAM 서비스 등이 해당 역할을 맡을 수 있는(assume) 권한을 부여한다.

- Principal이 Action을 할 수 있도록 허용한다.

```JSON
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Lambda가 sts의 역할 권한을 가지고 AWS 리소스에 액세스할 수 있다.

## Groups

여러 user들을 묶어 Access 권한 부여 및 관리하는 것.

## MFA

Multi Factor Authentication의 약자로, password와 security device를 조합해서 사용한다.

## IAM best practice

- 하나의 물리적 유저에 대해서 하나의 AWS 유저를 부여 한다.

- user를 group에 할당하고 group을 기반으로 권한을 부여한다.

- AWS 서비스를 위한 user role을 생성한다.

# Organizations

![Organizations](./pictures/Organizations.png)

- 글로벌 서비스.

- Member 계정은 하나의 Organizations에만 속할 수 있다.

- 모든 계정의 비용을 통합한다.

- 사용량을 종합해서 계산하기 떄문에 비용적 이익이 있다.

- 계정간 Reserved instance와 Savings plan discount를 공유한다.

- Organizations 내에 계정 생성을 자동화 할 수 있는 API가 있어 계정을 쉽게 생성할 수 있다.

## Service Control Policies(SCP)

조직(organization)이나 조직 단위(organizational units, OUs)에 속한 AWS 계정의 권한을 관리하고 제한하는 데 사용되는 정책.

- JSON 형식으로 작성된다.

- 조직의 root, 개별 organization 단위, 또는 계정에 적용될 수 있다.

- 액세스 제어. 최소 권한 모델 적용. 규정 준수 보장 등의 기능을 한다.

## IAM Group vs Organization

- AWS Organization은 AWS 계정을 생성하고, 그룹화(OU, Organizational Unit)하고, 정책(SCP, Service Control Policy)을 적용할 수 있다.

- 반면 IAM은 리소스에 대한 접근을 제어할수 있고, 사용자 및 그룹에 대한 생성, 접근 허용, 접근 거부 등을 설정할 수 있다.

- Organization은 AWS 계정을 대상으로 하지만, IAM은 사용자를 대상으로 한다.

- Organizations의 정책이 IAM의 정책보다 우선한다.

- 출처: [AWS Organizations, IAM 비교](https://ltlkodae.tistory.com/22)

- AWS Organizations는 계정 관리와 조직 전체 정책을 다루는 중앙 관리 서비스(대규모 기업에 적합)이며, IAM은 개별 사용자와 그룹의 권한 관리를 위한 서비스이다.

## Cross Account Access

한 AWS 계정의 사용자가 다른 AWS 계정의 리소스에 액세스할 수 있도록 허용하는 기능이다.

- 리소스 소유자 계정에서 IAM 역할을 생성하고, 신뢰 정책(Trust Policy)을 설정하여다른 계정의 사용자가 이 역할을 맡을 수 있도록 허용한다.

- 액세스 요청자 계정의 사용자는 AWS STS(Security Token Service)를 통해 교차 계정 역할을 맡는다.

# Cognito

![Cognito](./pictures/Cognito.png)

사용자에게 웹 또는 모바일 앱과 상호작용할 수 있는 ID를 제공하는 서비스.

- Cognito User Pool:

  - 앱 유저를 위한 로그인 서비스.

  - API Gateway와 ALB와 통합된다.

  - Serverless database를 생성한다.

- Cognito Identity Pools:

  - 앱에 등록된 사용자에게 임시 AWS 자격 증명을 제공해서 직접 엑세스 할 수 있게 한다.

---

# EC2

![EC2](./pictures/EC2.png)

EC2란 Elastic Compute Cloud의 약자로, 컴퓨팅 리소스를 대여하는 AWS의 서비스이다.

## EC2 User Data

Instance를 실행할 때 한 번만 동작하는 시작 스크립트이다.

## EC2 Instance Types

[Instance-Types](https://aws.amazon.com/ko/ec2/instance-types/)

t2medium을 예로 들었을 때

- t: 인스턴스의 class(bustable 등의 유형을 나타냄).

- 2: 인스턴스의 generation(하드웨어의 성능 및 기능에 따른 특정 시기에 출시된 인스턴스 유형).

- medium: 인스턴스 class의 크기(CPU, 메모리, 스토리지, 네트워크 성능 등에 따른 차이.

## Security Groups

EC2 인스턴스에 대한 방화벽 역할을 하는 서비스이다.

- Allow 규칙만 있고, Deny 규칙은 없다.

- Access port, IP range (IPv4/IPv6), Inbound, Outbound를 규정한다.

- 여러개의 Instance에 붙일 수 있다.

- VPC, Region의 범위로 적용이 제한된다.

- Deny에 해당하는 접근은 EC2의 외부에서 차단된다.

## Purchasing options

### On-Demand Instances

인스턴스를 사용한 초 단위로 비용을 지불 한다.

- 가장 비싸다.

### Reserved

인스턴스를 1 ~ 3년 예약한다.

- 최대 72%까지 할인된다.

- 선불을 어느정도 수준으로 지불할지 결정할 수 있다(할인율 달라짐).

### Savings Plans

1 ~ 3년간 사용량에 대한 약정.

- 구독과 같은 개념으로 사용자는 미리 정해진 사용량에 대해 할인을 받을 수 있다.

- "n년간 1시간에 10$의 사용량을 쓸 수 있다" 형태.

- 최대 72%까지 할인.

#### Compute Saving Plan

- 가장 flexible 하다.

- 인스턴스 패밀리, 사이즈, OS 등에 관계 없이 사용 가능하며, Lambda, Fargate 등에 대해서도 사용할 수 있다.

- 최대 66% 까지 절약.

#### EC2 Instance Savings Plan

- 특정 인스턴스 패밀리 내에서 가장 높은 할인율을 제공한다.

- Lambda, Fargate에서 사용 불가능. EC2 인스턴스에만 사용 가능하다.

### Spot Instances

가격은 저렴하지만 작업 중 인스턴스를 잃을 수 있음.

- 최대 90%까지 할인.

- Spot Fleets라는 서비스를 통해 자동으로 비용에 맞는 Spot instance를 요청할 수 있다.

- 비용 절감이 필요한 단기 Batch 작업, 빅데이터 분석 등에 많이 사용된다.

- Spot 인스턴스가 회수될 때 작업을 다른 인스턴스로 자동으로 이전할 수 있는 메커니즘을 구현하는 등 작업이 필요할 수 있다.

### Dedicated Hosts

전체 물리적 서버를 예약하고 인스턴스 배치를 제어한다.

### Dedicated Instances

다른 고객과 하드웨어를 공유하지 않는 전용 인스턴스.

### Capacity Reservations

사용 할지 안할지는 모르지만, 특정 가용 영역에서 필요한 EC2 용량을 미리 예약하고 보장 받는다.

- 예상치 못한 수요 증가나 리소스 부족으로 인해 필요한 인스턴스를 사용할 수 없는 상황을 방지하기 위해 존재한다.

## EIP

Elastic IP의 약자이다.

- EC2 인스턴스를 재시작 하면 public IP가 변경되게 되는데, 이 때 IP를 고정하기 위해 사용한다.

- 좋지 않은 아키텍처 구조를 유도할 수 있기 때문에 사용을 피하는 것이 좋다.

## Placement Group

EC2 인스턴스의 배치 전략 최적화에 사용된다.

### Cluster

여러 EC2를 같은 AZ, 같은 rack에 배치하여 10Gbps 정도의 낮은 지연 시간을 갖게한다.

- 고성능 컴퓨팅(HPC) 작업이나 대규모 데이터베이스 애플리케이션과 같이 높은 네트워크 처리량과 낮은 지연 시간이 필요한 작업에 적합하다.

### Spread

각 EC2 인스턴스를 서로 다른 하드웨어에 분산시켜, 단일 인스턴스 장애가 다른 인스턴스에 영향을 미치지 않도록 한다.

### Partition

EC2를 여러 파티션으로 나누어 각 파티션의 인스턴스가 서로 다른 물리적 하드웨어 세트에 배치되도록 한다.

## ENI

가상의 네트워크 카드. AZ에 종속된다.

## AMI

Amazon Machine Image의 약자로, EC2의 Customization이다.

- Public AMI, Custom AMI, Marketplace AMI 등이 있다.

## Instance Store

EBS는 네트워크 장치이기 때문에, 고성능 하드웨어 디스크가 필요한 경우 사용한다.

- 빠른 I/O속도를 갖는다.

- EC2가 종료되면 저장된 정보가 삭제된다.

- 백업과 복제가 개인의 책임이다.

---

# EBS

![EBS](./pictures/EBS.png)

Elastic Block Storage는 EC2에 붙이는 '네트워크' 장치이다 (네트워크 UBS 스틱과 유사).

- 물리 장치가 아니라 네트워크 장치이기 때문에 latency가 존재한다.

- 기본적으로는 한 번에 한 인스턴스에만 붙일 수 있다.

- 특정 조건을 만족할 때 multi attach 기능을 통해 하나의 EBS를 어러 개의 인스턴스에 부착할 수도 있고, 하나의 인스턴스에 여러 개의 EBS를 부착할 수도 있다.

- 특정 AZ에 바인딩 되어 있다.

- 사이즈(GB)와 속도(IOPS)의 제한이 있다.

- EC2가 삭제되었을 때 같이 삭제될 것인지를 정할 수 있다.

## EBS Snapshot

특정 시점의 EBS를 백업할 수 있다.

- EC2와 EBS volume(저장 공간의 한 단위 또는 블록 레벨의 스토리지 장치)을 분리하지 않아도 되지만, 분리할 것이 권장된다.

- AZ나 Region을 건너 뛰어 Snapshot을 복사할 수 있다.

- Archive tier에 보관하여 75% 정도의 비용을 아낄 수 있다(복구 시간 느림).

- n일 후 삭제되도록 Recycle bin에 넣을 수 있다.

- Fast Snapshot Restore(FSR)을 이용해 곧바로 복구시킬 수 있다.

## EBS Volume Types

- gp2 / gp3 (SSD): 다양한 워크로드에 대해 가격과 성능을 균형있게 제공하는 범용 SSD 볼륨이다. 최대 16000 IOPS.

- io1 / io2 (SSD): 미션 크리티컬한 저지연 또는 고처리량 워크로드를 위한 가장 높은 성능을 제공하는 SSD 볼륨이다. 최대 64,000 IOPS.

- st1 (HDD): 자주 접근하는 처리량 집중적 워크로드를 위해 설계된 저비용 HDD 볼륨이다.

- sc1 (HDD): 자주 접근하지 않는 워크로드를 위해 설계된 가장 저렴한 HDD 볼륨이다.

## EBS Multi Attach

1. 하나의 EBS볼륨을 여러 개의 인스턴스에 부착.

- io1/io2과 같은 특정 유형에서만 지원됨.

- 모든 관련 인스턴스는 같은 Region 및 AZ에 있어야 한다.

2. 하나의 인스턴스에 여러 EBS Volume 부착.

- 인스턴스 유형에 따라 최대 EBS 볼륨 개수가 정해져 있음.

- 볼륨이 많아질 수록 필요한 대역폭이 늘어나기 때문에 인스턴스 네트워크 I/O 역폭을 확인해야 한다.

- 사용중인 운영체제와 네트워크 드라이버가 다중 EBS를 지원하는지 확인해야 한다.

---

# EFS

![EFS](./pictures/EFS.png)

Elastic File System의 약자로 여러 개의 EC2에 mount할 수 있는 managed network file system이다.

- 다중 AZ의 EC2와 연결이 가능하다.

- 데이터를 여러 가용 영역(AZ)에 걸쳐 자동으로 복제한다. (built-in redundancy)

- 가용성이 높고, 확장성이 높고, 가격이 비싸다.

- Linux base AMI (POSIX)에서만 사용 가능하다.

- Lustre client와의 직접적 통합 미지원.

- EFS 생성시 암호화 활성화 옵션을 선택하면 유휴 데이터(data at rest)를 KMS를 통해 암호화 된다.

- 기능을 활성화 하면 data in transit도 암호화가 가능하다.

## EFS Storage Class

- Standard: 자주 접근하는 파일용.

- Infrequent Access(IA): 파일을 검색하는데 비용이 발생하며, 저장 비용은 더 낮다. (90% 이상 절감 가능)

## EBS vs EFS

EBS:

- 한 인스턴스에 부착 된다. (io1/io2 제외)

- AZ에 고정.

- 디스크 크기 증가 시 I/O증가.

- Migration을 위해서 Snapshot 필요.

- EC2 종료 시 인스턴스의 Root EBS도 종료된다.

EFS:

- 다중 AZ에 100여개의 인스턴스에 연결됨.

- 웹사이트 파일을 공유한다. (워드프레스 등)

- 리눅스 기반 이미지에서만 사용 가능.

- EBS보다 비싸다.
