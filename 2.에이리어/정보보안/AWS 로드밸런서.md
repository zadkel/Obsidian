---
created: 2025-04-15 23:32
tags:
  - 클라우드
---

---

## AWS ELB (Elastic Load Balancing)

![[Pasted image 20250415233448.png]]

ELB는 AWS에서 안정적이고 확장 가능한 애플리케이션을 구축하기 위한 핵심 네트워킹 서비스 중 하나로, 자동으로 들어오는 트래픽을 분산시켜준다.

### 1.정의

- **개념:** 들어오는 애플리케이션 트래픽을 EC2 인스턴스, 컨테이너(ECS/EKS), Lambda 함수, IP 주소 등 **여러 대상(Target)에 걸쳐 자동으로 분산**시켜주는 AWS의 관리형 로드 밸런싱 서비스임.
- **목적:** 애플리케이션의 **내결함성(Fault Tolerance) 및 고가용성(High Availability)** 을 높이고, 트래픽 부하 분산을 통해 **확장성(Scalability)** 을 확보하는 데 사용됨.

### 2.핵심 개념 및 특징

- **로드 밸런서 (Load Balancer):**
    - 클라이언트 요청의 **단일 진입점(Single Point of Contact)** 역할 수행. 수신된 요청을 등록된 정상 상태의 대상 그룹 내 대상들에게 라우팅함.
- **리스너 (Listener):**
    - 설정된 프로토콜(HTTP, HTTPS, TCP 등) 및 포트 번호를 사용하여 **클라이언트로부터의 연결 요청을 확인**하는 프로세스. 각 로드 밸런서에는 하나 이상의 리스너가 필요함. 리스너 규칙에 따라 요청을 특정 대상 그룹으로 전달.
- **대상 그룹 (Target Group):**
    - 로드 밸런서로부터 트래픽을 수신하는 **대상(EC2 인스턴스, IP 주소, Lambda 함수 등)들의 모음**. 리스너 규칙은 트래픽을 라우팅할 대상 그룹을 지정함.
- **상태 검사 (Health Check):**
    - ELB가 등록된 대상의 상태를 **주기적으로 확인**하는 메커니즘. 지정된 경로/포트로 헬스 체크 요청을 보내 응답을 확인하고, 비정상(Unhealthy) 대상으로 판명되면 해당 대상으로는 트래픽 라우팅을 일시 중단함. 대상이 다시 정상 상태가 되면 라우팅 재개.
- **자동 확장성 (Auto Scaling):**
    - 로드 밸런서 자체도 **수신 트래픽 양에 따라 자동으로 처리 용량을 확장/축소**함.
- **고가용성 (High Availability):**
    - ELB 서비스 자체는 특정 리전(Region) 내 **여러 가용 영역(AZ)에 걸쳐 이중화**되어 설계되므로 단일 실패 지점(Single Point of Failure)이 되는 것을 방지함.

### 3.로드 밸런서 종류

ELB는 워크로드 특성에 맞춰 선택할 수 있는 여러 유형의 로드 밸런서를 제공함.

- **Application Load Balancer (ALB):**
    - **계층:** 애플리케이션 계층 (Layer 7).
    - **특징:** HTTP/HTTPS 트래픽 로드 밸런싱에 최적화. 요청 내용(경로, 호스트, 헤더 등) 기반의 고급 라우팅 규칙, 마이크로서비스 및 컨테이너 환경 지원(ECS, EKS 통합), WebSocket, HTTP/2 지원.
    - **주요 용도:** 웹 애플리케이션, 마이크로서비스 아키텍처, 컨테이너 기반 애플리케이션.
- **Network Load Balancer (NLB):**
    - **계층:** 전송 계층 (Layer 4).
    - **특징:** TCP, UDP, TLS 트래픽 처리. 초고성능(수백만 req/sec), 매우 낮은 지연 시간. AZ별 고정 IP(Elastic IP) 할당 가능, 클라이언트의 소스 IP 주소 보존.
    - **주요 용도:** 고성능 TCP/UDP 애플리케이션, 극도의 성능 및 낮은 지연 시간이 중요한 워크로드, 소스 IP 보존 필요 시, 고정 IP 필요 시.
- **Gateway Load Balancer (GWLB):**
    - **계층:** 네트워크 계층 (Layer 3 Gateway) + 전송 계층 (Layer 4 Load Balancing).
    - **특징:** 타사 가상 어플라이언스(방화벽, IDS/IPS, DPI 시스템 등)의 배포, 확장, 관리를 용이하게 하도록 설계됨. 트래픽을 해당 어플라이언스로 투명하게 라우팅.
    - **주요 용도:** 네트워크 트래픽 흐름에 보안 또는 네트워크 가상 어플라이언스 통합 시.
- **Classic Load Balancer (CLB):**
    - **계층:** 전송 계층(Layer 4) 및 애플리케이션 계층(Layer 7 - 제한적 기능) 모두 작동.
    - **특징:** 이전 세대 로드 밸런서. EC2 인스턴스 간 기본 로드 밸런싱 제공.
    - **참고:** AWS는 새로운 애플리케이션에는 ALB 또는 NLB 사용을 권장함. (레거시 지원 목적)


| 유형 (Type)                           | OSI 계층              | 주요 프로토콜               | 핵심 특징 및 주요 용도                                                                          |
| :---------------------------------- | :------------------ | :-------------------- | :------------------------------------------------------------------------------------- |
| **Application Load Balancer (ALB)** | Layer 7 (애플리케이션)    | HTTP, HTTPS           | - 경로/호스트/헤더 기반 라우팅 등 고급 규칙<br>- 마이크로서비스, 컨테이너(ECS/EKS) 환경 최적화<br>- 웹 애플리케이션, API 게이트웨이 |
| **Network Load Balancer (NLB)**     | Layer 4 (전송)        | TCP, UDP, TLS         | - 초고성능, 낮은 지연 시간<br>- AZ별 고정 IP(EIP) 지원<br>- 소스 IP 주소 보존<br>- TCP/UDP 트래픽, 성능 중요 워크로드  |
| **Gateway Load Balancer (GWLB)**    | Layer 3/4 (네트워크/전송) | IP, TCP, UDP          | - 타사 가상 네트워크 어플라이언스(방화벽, IDS/IPS 등) 배포 및 확장<br>- 투명한 트래픽 라우팅 및 검사                      |
| **Classic Load Balancer (CLB)**     | Layer 4/7 (혼합)      | TCP, SSL, HTTP, HTTPS | - 이전 세대 로드 밸런서<br>- EC2 인스턴스 간 기본 로드 밸런싱<br>- **(레거시, 신규 사용 비권장)**                     |
**선택 가이드:**
- 일반적인 웹 애플리케이션, 마이크로서비스에는 **ALB**가 가장 적합함.
- 최고 성능, 낮은 지연 시간, TCP/UDP 트래픽 또는 고정 IP가 필요하면 **NLB**를 선택함.
- 네트워크 트래픽 중간에 가상 보안 장비 등을 통합해야 할 경우 **GWLB**를 사용함.
- 기존에 CLB를 사용하고 있지 않다면, **CLB는 현재 고려 대상이 아님**.


### 4.주요 이점

- **고가용성:** 여러 AZ에 걸친 대상 분산 및 상태 검사를 통해 서비스 가용성 증대.
- **확장성:** 트래픽 변화에 따라 로드 밸런서 및 대상 그룹(Auto Scaling 연동 시) 자동 확장/축소.
- **보안:** 보안 그룹 연동, SSL/TLS 종료(ALB, CLB), AWS WAF(Web Application Firewall) 및 AWS Certificate Manager(ACM) 통합 지원.
- **상태 검사:** 비정상 대상 자동 감지 및 트래픽 제외.
- **트래픽 관리:** 단일 엔드포인트 제공 및 유연한 트래픽 분산 규칙 설정.




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
