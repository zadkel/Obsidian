---
created: 2025-04-15 23:03
tags:
  - 클라우드
---

---
## **AWS EC2 (Elastic Compute Cloud)**

![[Pasted image 20250415230515.png]]
### **1. EC2 정의**

- **개념:** AWS(Amazon Web Services)에서 제공하는 **안전하고 크기 조정이 가능한 컴퓨팅 파워(가상 서버)** 를 클라우드 환경에서 제공하는 웹 서비스임.
- **핵심:** 사용자가 필요에 따라 가상 머신(VM)을 생성(임대)하고 컴퓨팅 요구 사항을 처리할 수 있도록 하는, AWS의 가장 기본적이고 핵심적인 **IaaS (Infrastructure as a Service)** 형태의 서비스.
- **"Elastic"의 의미:** 컴퓨팅 리소스(CPU, 메모리, 스토리지 등)의 **수요 변화에 따라 용량을 쉽게 늘리거나 줄일 수 있는 탄력성**을 의미함.

### **2. 핵심 개념 및 특징**

![[Pasted image 20250415230808.png]]
- **[[AWS 인스턴스|인스턴스]] (Instance):**
    - EC2에서 생성된 **개별 가상 머신(서버)** 단위. 사용자는 이 인스턴스에 운영체제를 설치하고 애플리케이션을 배포하여 사용함.
- **Amazon Machine Image (AMI):**
    - 인스턴스를 시작(Launch)하는 데 필요한 **정보를 담은 이미지 템플릿**. 운영체제(OS), 애플리케이션 서버, 초기 설정 등을 포함함.
    - AWS 제공 AMI, 커뮤니티 AMI, 사용자가 직접 생성한 맞춤 AMI 사용 가능.
- **스토리지 옵션:**
    - **EBS (Elastic Block Store):** 인스턴스에 연결하여 사용하는 **영구적인 블록 스토리지 볼륨** (가상 하드 디스크). 인스턴스가 중지/종료되어도 데이터 유지됨.
    - **인스턴스 스토어 (Instance Store):** 인스턴스를 호스팅하는 물리적 서버에 직접 연결된 **임시 블록 스토리지**. I/O 성능은 빠르지만, 인스턴스 중지/종료/장애 시 데이터 소실됨.
- **네트워킹:**
    - **[[AWS VPC|VPC]] (Virtual Private Cloud):** 
	    - 사용자의 AWS 계정 전용 가상 네트워크 환경 내에서 인스턴스 실행. 
	    - IP 주소 범위, 서브넷, 라우팅 테이블, 네트워크 게이트웨이 등 제어 가능.
    - **보안 그룹 (Security Group):** 인스턴스 수준에서 인바운드/아웃바운드 네트워크 트래픽을 제어하는 **상태 저장(Stateful) 가상 방화벽**.
- **탄력성 및 확장성 (Elasticity & Scalability):**
    - 필요에 따라 **몇 분 만에** 인스턴스 수를 늘리거나(Scale-out) 줄이고(Scale-in), 인스턴스 사양을 변경(Scale-up/down) 가능.
    - **Auto Scaling Group:** 정의된 조건(예: CPU 사용률)에 따라 인스턴스 수를 자동으로 조정하는 기능 제공.
- **요금 모델:**
    - 사용한 만큼 지불하는 **온디맨드(On-Demand)**
    - 약정을 통해 할인받는 **예약 인스턴스(Reserved Instances, RI)** 또는 **Savings Plans**
    - 경매 방식의 저렴한 **스팟 인스턴스(Spot Instances)**

### **3. 주요 사용 사례**

- 웹 사이트 및 웹 애플리케이션 호스팅
- 기업용 애플리케이션 서버 (백엔드, API 서버 등)
- 데이터베이스 서버 (단, 관리형 서비스인 AWS RDS 가 더 선호되기도 함)
- 개발 및 테스트 환경 구축
- 배치 처리 및 데이터 분석
- 고성능 컴퓨팅 (HPC)
- 머신러닝 모델 학습 및 추론





---
# 링크 
## 🔗 아웃링크
```dataview
LIST WITHOUT ID outgoing
FROM ""
WHERE file = this.file
FLATTEN file.outlinks as outgoing
WHERE endswith(lower(outgoing.path), ".md")
SORT outgoing ASC
```
## 🔙 백링크
```dataview
LIST WITHOUT ID file.link
WHERE contains(file.outlinks, this.file.link)
SORT file.link ASC
```
