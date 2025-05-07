---
created: 2025-04-15 23:53
tags:
  - 클라우드
---

---
## 1. S3 (Simple Storage Service)

- **개념:** AWS에서 제공하는 **객체 스토리지(Object Storage)** 서비스임. 업계 최고 수준의 확장성, 데이터 가용성, 보안 및 성능을 제공하도록 구축됨.
- **목적:** 인터넷을 통해 **언제 어디서든 원하는 양의 데이터를 저장하고 검색**할 수 있도록 설계됨. 파일 시스템(EFS 등)이나 블록 스토리지(EBS 등)와 달리 데이터를 '객체' 단위로 관리함.

## 2. 핵심 개념

### 버킷 (Bucket)

- S3에 저장되는 **객체(Object)들을 담는 최상위 컨테이너(저장 공간)**. 폴더와 유사한 개념이지만 계층 구조는 아님.
- 버킷 이름은 **전 세계 모든 AWS 계정에서 고유**해야 함 (Globally Unique).
- 버킷은 사용자가 선택한 특정 **AWS 리전(Region)** 에 생성됨. 저장된 데이터는 기본적으로 해당 리전을 벗어나지 않음.

### 객체 (Object)

- S3에 저장되는 **기본 데이터 단위**. 일반적으로 파일과 그 메타데이터를 의미함.
- **구성 요소:**
    - **데이터 (Data):** 실제 파일의 내용.
    - **키 (Key):** 버킷 내에서 객체를 고유하게 식별하는 이름 (예: `documents/report.pdf`). 전체 경로 포함 가능하여 폴더 구조처럼 보이게 함.
    - **메타데이터 (Metadata):** 객체에 대한 설명 정보 (예: `Content-Type`, `Content-Length`, 최종 수정일 등). 시스템 정의 및 사용자 정의 메타데이터 포함.
    - **버전 ID (Version ID):** 버킷에 버전 관리가 활성화된 경우, 객체의 특정 버전을 식별하는 ID.
    - **접근 제어 정보 (ACL 등):** 객체 수준 접근 권한 설정 (일반적으로 IAM 정책, 버킷 정책 사용 권장).
- **크기:** 단일 객체의 최대 크기는 일반적으로 5TB.

### 키 (Key)

- 버킷 내에서 객체의 **고유한 식별자(이름)**. 전체 경로를 포함하는 문자열 형태 (예: `images/cat.jpg`).
- S3는 본질적으로 평면 구조(Flat structure)이지만, 키 이름에 슬래시(`/`)를 사용하여 디렉토리와 같은 논리적 계층 구조를 표현 가능함.

### 리전 (Region)

- 버킷을 생성할 때 선택하는 **AWS의 지리적 위치**. 데이터는 선택된 리전에 물리적으로 저장됨. 지연 시간 감소, 비용 최적화, 규정 준수 요구 사항 충족 등을 위해 선택.

## 3. 주요 특징 및 이점

- **내구성 (Durability):**
    - 객체 손실률이 극도로 낮도록 설계됨 (99.999999999%). 여러 시설과 디바이스에 데이터를 중복 저장하여 구현.
- **가용성 (Availability):**
    - 필요할 때 데이터에 접근할 수 있는 높은 가용성을 제공하도록 설계됨 (스토리지 클래스별 SLA 상이, 예: S3 Standard 99.99%).
- **확장성 (Scalability):**
    - 사실상 무제한의 저장 공간 제공. 저장 용량이나 트래픽 증가에 맞춰 자동으로 확장됨.
- **보안 (Security):**
    - 데이터 암호화(전송 중/저장 시), IAM 정책, 버킷 정책, ACL을 통한 세분화된 접근 제어, VPC 엔드포인트, 접근 로그 등 다양한 보안 기능 제공.
- **관리 기능 (Management):**
    - **수명 주기 관리(Lifecycle):** 객체를 다른 스토리지 클래스로 자동 이동하거나 삭제하는 규칙 설정.
    - **버전 관리(Versioning):** 객체의 모든 버전을 보존하여 실수로 인한 삭제나 덮어쓰기로부터 보호.
    - **복제(Replication):** 다른 리전 또는 동일 리전의 다른 버킷으로 객체를 자동 복사 (재해 복구, 지연 시간 감소 목적).
    - **이벤트 알림(Event Notifications):** 객체 생성, 삭제 등 이벤트 발생 시 SQS, SNS, Lambda 등으로 알림 전송.
- **비용 효율성 (Cost-Effectiveness):**
    - 사용한 만큼만 비용 지불. 데이터 접근 빈도 및 보관 기간에 따라 다양한 스토리지 클래스 제공.
- **다양한 인터페이스:**
    - AWS 관리 콘솔, AWS CLI, AWS SDK, REST API를 통해 접근 및 관리 가능.

## 4. 스토리지 클래스 (Storage Classes)

- 데이터의 접근 빈도, 검색 시간 요구 사항, 비용 민감도에 따라 최적화된 다양한 스토리지 클래스를 제공함.
- **주요 클래스:**
    - **S3 Standard:** 범용. 자주 접근하는 데이터용. 낮은 지연 시간. (기본값)
    - **S3 Intelligent-Tiering:** 접근 패턴에 따라 자동으로 비용 최적화 계층(Frequent Access / Infrequent Access) 이동. 접근 패턴 예측 어려울 때 유용.
    - **S3 Standard-IA (Infrequent Access):** 자주 접근하지 않지만 필요 시 빠른 검색 필요 데이터용. Standard보다 저렴한 저장 비용, 검색 비용 발생.
    - **S3 One Zone-IA:** Standard-IA와 유사하나 단일 AZ에만 저장. 더 저렴하나 AZ 장애 시 데이터 유실 가능성 있음.
    - **S3 Glacier Instant Retrieval:** 아카이브 데이터지만 밀리초 단위 검색 필요 시. IA보다 저렴한 저장 비용, 검색 비용 발생.
    - **S3 Glacier Flexible Retrieval:** 아카이브 데이터. 검색 시간 유연(분~시간 단위). 매우 저렴한 저장 비용.
    - **S3 Glacier Deep Archive:** 장기 보관용 최저가 아카이브. 검색 시간 수 시간 소요.

## 5. 주요 사용 사례

- 데이터 백업 및 복구
- 데이터 아카이빙 (규정 준수 등)
- 정적 웹 사이트 호스팅
- 빅데이터 분석을 위한 데이터 레이크 구축
- 이미지, 비디오 등 미디어 파일 저장 및 배포 (CDN 연동)
- 소프트웨어 배포
- 로그 파일 저장 및 분석





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
