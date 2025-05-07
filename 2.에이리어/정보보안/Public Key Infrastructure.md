---
created: 2025-04-15 15:41
tags:
  - 암호학
---

---
## **공개 키 기반 구조 (PKI - Public Key Infrastructure)**

![[Pasted image 20250415154210.png]]

### **1. PKI 정의**

- **개념:** 디지털 인증서(Digital Certificates)를 **생성, 관리, 배포, 사용, 저장 및 폐기**하고, **공개 키 암호화**를 관리하는 데 필요한 **역할, 정책, 하드웨어, 소프트웨어, 절차** 등을 포괄하는 **하나의 프레임워크 또는 시스템**임.
- **목적:** 인터넷과 같이 신뢰할 수 없는 공개된 네트워크 환경에서 **안전한 전자 거래 및 정보 교환**을 가능하게 하는 기반 제공. 주로 데이터의 **기밀성, 무결성, 인증, 부인 방지** 기능 구현에 사용됨.

### **2. 핵심 구성 요소**

- **디지털 인증서 (Digital Certificate):**
    - 공개 키가 특정 개인/단체/서버 등(주체, Subject)의 **신원(Identity)에 속함을 증명**하는 전자 문서.
    - **공개 키, 주체 정보, 발급자(CA) 정보, 유효 기간, CA의 디지털 서명** 등을 포함함 (주로 X.509 표준 형식).
    - 역할: 공개 키의 소유자(주체)가 신뢰할 수 있음을 보증함.
- **인증 기관 (CA - Certification Authority):**
    - 디지털 인증서를 **발급하고 검증하는 신뢰할 수 있는 제3자 기관**. PKI의 신뢰 근간(Trust Anchor) 역할.
    - 인증서 발급 요청자의 신원을 확인하고, 인증서에 자신의 비밀 키로 디지털 서명하여 인증서의 유효성 보증.
    - 인증서 폐기 목록(CRL) 관리 및 게시.
- **등록 기관 (RA - Registration Authority):**
    - 인증 기관(CA)을 대신하여 인증서 **신청자의 신원 확인 및 등록 절차**를 수행하는 기관. CA의 업무 부담 경감. (CA가 직접 수행하기도 함)
- **정책 기관 (PA - Policy Authority):**
	- PKI가 운영되는 기준이 되는 정책(Policy) 및 실행 지침(Practice)을 수립, 승인, 관리하는 주체.
	- 인증서 정책(CP - Certificate Policy: 인증서의 용도, 적용 범위 등 정의)과 인증 실행 준칙(CPS - Certification Practice Statement: CA가 CP를 어떻게 구현하는지 기술) 등을 정의함.
	- 즉, CA나 RA 등이 **어떤 규칙과 절차에 따라 운영되어야 하는지를 결정**하는 역할. (규모가 작은 PKI에서는 CA가 PA 역할을 겸하기도 함)
- **인증서 저장소 (Certificate Repository):**
    - 발급된 디지털 인증서 및 인증서 폐기 목록(CRL)을 **저장하고 공개적으로 접근 가능하도록** 하는 데이터베이스 또는 디렉토리.
- **인증서 폐기 목록 (CRL - Certificate Revocation List):**
    - 유효 기간이 만료되기 전에 **더 이상 유효하지 않게 된(폐기된) 인증서들의 일련번호 목록**. CA가 주기적으로 게시함. (예: 개인 키 유출 시 해당 인증서 폐기)
    - **OCSP (Online Certificate Status Protocol):** CRL의 단점을 보완하여 실시간으로 인증서 상태를 확인할 수 있는 프로토콜.

### **3. 주요 기능 / 목표**

PKI는 공개 키 암호화와 디지털 인증서를 활용하여 다음 보안 목표 달성을 지원함.

- **인증 (Authentication):** 통신 당사자(사용자, 서버 등)의 신원 확인.
- **기밀성 (Confidentiality):** 공개 키 암호화를 통해 인가된 수신자만 데이터 복호화 가능.
- **무결성 (Integrity):** 디지털 서명을 통해 데이터가 전송 중 위변조되지 않았음을 보장.
- **부인 방지 (Non-repudiation):** 디지털 서명을 통해 특정 행위(메시지 전송, 거래 승인 등)를 수행했음을 부인할 수 없도록 함.

---

PKI는 웹 사이트 보안(HTTPS/SSL/TLS), 이메일 보안(S/MIME), 전자 서명, VPN 등 다양한 보안 응용 분야의 핵심 기반 기술로 활용됨.

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
