---
created: 2025-04-15 23:18
tags:
  - 클라우드
---

---

## AWS EC2 인스턴스 (AWS EC2 Instance)

**정의:** AWS EC2(Elastic Compute Cloud) 서비스 내에서 실행되는 **가상 서버(Virtual Machine, VM)** 를 의미함. 클라우드 환경에서 사용자가 빌려 쓰는 독립된 컴퓨터 한 대와 유사함.

## 인스턴스의 주요 개념

### 1. AMI (Amazon Machine Image)

- 인스턴스를 생성(Launch)할 때 필요한 운영체제(OS), 초기 설정, 애플리케이션 등이 포함된 **실행 환경 템플릿**.
- AWS 제공 AMI (Amazon Linux, Ubuntu, Windows 등), 마켓플레이스 AMI, 사용자 정의 AMI 사용 가능.

### 2. 인스턴스 유형 (Instance Type)

- 인스턴스의 하드웨어 사양(vCPU 수, 메모리 크기, 스토리지 종류/성능, 네트워크 대역폭)을 결정하는 **다양한 구성 옵션**.
- **주요 유형 예시:**
    - **범용 (General Purpose - T, M 계열):** 웹 서버, 개발 환경 등 균형 잡힌 성능 필요 시.
    - **컴퓨팅 최적화 (Compute Optimized - C 계열):** CPU 성능이 중요한 배치 처리, 고성능 웹 서버 등.
    - **메모리 최적화 (Memory Optimized - R, X 계열):** 대규모 데이터베이스, 인메모리 캐시 등 메모리 용량이 중요 시.
    - **스토리지 최적화 (Storage Optimized - I, D 계열):** 높은 디스크 I/O 성능이 필요한 데이터 웨어하우스, 분산 파일 시스템 등.

### 3. 인스턴스 상태 (Instance State)

- 인스턴스의 현재 운영 상태를 나타냄. 주요 상태:
    - `running`: 정상 실행 중 **(과금 발생)**.
    - `stopped`: 중지됨 (EBS 볼륨 데이터 유지, 인스턴스 스토어 데이터 삭제. CPU/RAM 과금 없음, EBS 과금은 발생).
    - `terminated`: 영구 삭제됨 (모든 데이터 삭제). 복구 불가.

### 4. 보안 그룹 (Security Group)

- 인스턴스 레벨에서 인바운드/아웃바운드 네트워크 트래픽을 제어하는 **상태 기반 가상 방화벽**. 포트, 프로토콜, 소스/대상 IP 기반 규칙 설정.

### 5. 키 페어 (Key Pair)

- 인스턴스에 안전하게 **SSH(Linux) 또는 RDP(Windows)로 접속하기 위한 공개 키-비밀 키 쌍**. 공개 키는 인스턴스에 저장, 비밀 키(`.pem` 또는 `.ppk` 파일)는 사용자가 안전하게 보관 및 접속 시 사용.

### 6. Elastic IP (EIP, 탄력적 IP 주소)

- 동적으로 할당되는 퍼블릭 IP 주소와 달리, **AWS 계정에 할당되어 인스턴스와 연결/분리할 수 있는 고정 퍼블릭 IPv4 주소**. 인스턴스를 중지/시작해도 동일한 IP 유지 필요 시 사용.



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
