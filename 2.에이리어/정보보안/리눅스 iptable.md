---
created: 2025-04-15 12:57
tags:
  - 리눅스
---

---

## **방화벽, Netfilter, iptables**

### **1. 방화벽 (Firewall) - 개념**

- **정의:** 미리 정의된 보안 규칙에 따라 네트워크 트래픽(수신/송신)을 **감시하고 제어**하는 네트워크 보안 시스템 또는 소프트웨어.
- **목적:** 허가되지 않은 접근, 악성 트래픽 등으로부터 시스템이나 네트워크를 보호하는 것이 주 목적임. 가장 기본적인 네트워크 보안 계층.

### **2. Netfilter - 커널 프레임워크**

- 정의: **리눅스 커널 내부에 구축된 프레임워크**로, 네트워크 패킷의 **필터링, 조작(주소 변환 NAT 등), 로깅** 등을 수행하는 기반 구조임.
- 역할: 커널의 네트워크 스택(데이터 처리 경로) 내 여러 지점에 **'훅(Hook)'** 이라는 갈고리를 제공함. 커널 모듈들은 이 훅에 자신의 함수를 등록하여, 해당 지점을 통과하는 패킷을 검사하고 원하는 대로 처리(허용, 차단, 변경 등)할 수 있음.
- 핵심: 실제 패킷 검사 및 제어 작업이 **커널 수준에서** 이루어지도록 하는 핵심 엔진. 방화벽 기능을 구현하는 **기반 기술**.

### **3. iptables - 사용자 공간 도구**

- 정의: **사용자 공간(Userspace)의 커맨드 라인 유틸리티**로, 관리자가 **Netfilter 프레임워크가 사용할 규칙(Rule)을 설정하고 관리**하는 데 사용되는 도구임.
- 역할: 관리자가 정의한 규칙들을 커널 내의 Netfilter에게 전달하여 실제 방화벽 정책을 적용하도록 함.
- 구조:
    - **테이블 (Tables):** 특정 종류의 패킷 처리 목적을 가진 체인(Chain)들의 모음.
        - `filter`: 패킷 필터링(허용/차단) 담당 (가장 기본적이고 많이 사용됨). INPUT, OUTPUT, FORWARD 체인 포함.
        - `nat`: 네트워크 주소 변환(Network Address Translation) 담당. PREROUTING, POSTROUTING, OUTPUT 체인 포함.
        - `mangle`: 패킷 헤더 수정 등 특별한 조작 담당.
        - `raw`: 연결 추적(Connection Tracking) 예외 처리 담당.
    - **체인 (Chains):** 테이블 내에서 규칙(Rule)들이 순서대로 나열된 목록. 패킷은 이 체인을 따라가며 규칙과 매칭되는지 검사됨. (예: `INPUT`, `OUTPUT`, `FORWARD`, `PREROUTING`, `POSTROUTING`)
    - **규칙 (Rules):** 특정 조건(매치: 출발지/목적지 IP, 포트, 프로토콜 등)과 그 조건에 맞을 때 수행할 행동(타겟: `ACCEPT`, `DROP`, `REJECT`, `LOG` 등)으로 구성됨.
- **현대적 관점:** `iptables`는 매우 강력하지만 문법이 복잡하여, 최근에는 `firewalld`나 `ufw` 같은 더 사용하기 쉬운 도구들이 많이 사용됨. 이들 도구는 내부적으로 `iptables`나 그 후속 기술인 `nftables`를 사용하여 Netfilter를 제어하는 경우가 많음.

### **4. 관계 요약**

- **방화벽:** 네트워크 보안을 위한 **일반적인 개념 또는 시스템**
- **Netfilter:** 리눅스 **커널**에 내장되어 방화벽 기능을 **실제로 구현하는 프레임워크**
- **iptables:** **사용자 공간**에서 Netfilter 프레임워크의 **규칙을 설정하고 관리하는 도구**

간단히 말해, 관리자는 `iptables` (또는 `firewalld`, `ufw`) 명령어로 규칙을 만들면, 그 규칙이 커널의 `Netfilter`에 전달되어 실제 네트워크 트래픽에 대한 방화벽 정책이 적용되는 구조임.

![[Pasted image 20250227111011.png]]




`iptabless [-t table] [Action] [Chain] []`

# iptalbes 명령

| 구분           | 종류             | 설명                                                                                               |
| ------------ | -------------- | ------------------------------------------------------------------------------------------------ |
| **Table**    | filter         | - Packet filtering 기능을 수행  <br> - INPUT, FORWARD, OUTPUT에서 사용                                    |
|              | nat            | - Network address translation 기능 수행  <br> - POSTROUTING, PREROUTING, OUTPUT에 사용                  |
|              | mangle         | - Packet 내의 특정 필드값을 변경하는 기능 (ex. TOS)  <br> - POSTROUTING, PREROUTING, INPUT, FORWARD, OUTPUT 사용 |
| **Action**   | -A             | - 정책 추가 (Append)                                                                                 |
|              | -I             | - 정책 삽입 (Insert)                                                                                 |
|              | -D             | - 정책 삭제 (Delete)                                                                                 |
|              | -R             | - 정책 교체 (Replace)                                                                                |
|              | -F             | - 모든 정책 삭제 (Flush)                                                                               |
|              | -L             | - 정책 확인 (List)                                                                                   |
| **Chain**    | INPUT          | - `NF_IP_LOCAL_IN` 훅이 실행될 때 적용                                                                   |
|              | OUTPUT         | - `NF_IP_LOCAL_OUT` 훅이 실행될 때 적용                                                                  |
|              | FORWARD        | - `NF_IP_FORWARD` 훅이 실행될 때 적용                                                                    |
|              | PREROUTING     | - `NF_IP_PRE_ROUTING` 훅이 실행될 때 적용                                                                |
|              | POSTROUTING    | - `NF_IP_POST_ROUTING` 훅이 실행될 때 적용                                                               |
| **Matching** | -s             | - Source matching (ip주소)                                                                         |
|              | -d             | - Destination matching                                                                           |
|              | -p             | - Protocol matching (ex: TCP, UDP)                                                               |
|              | --sport        | - Source port matching                                                                           |
|              | --dport        | - Destination port matching                                                                      |
|              | -i             | - Input Interface 지정 (ex. eth0)                                                                  |
|              | -o             | - Output Interface 지정                                                                            |
| **Target**   | ACCEPT         | - Packet 허용                                                                                      |
|              | DROP           | - Packet drop                                                                                    |
|              | REJECT         | - Packet drop                                                                                    |
|              | LOG            | - Packet을 syslog에 기록                                                                             |
|              | SNAT --to [주소] | - Source NAT: Source IP를 [주소]로 변환                                                                |
|              | DNAT --to [주소] | - Destination NAT: Destination IP를 [주소]로 변환                                                      |
|              | RETURN         | - 호출 체인에서 return                                                                                 |
|              | MASQUERADE     | - `POSTROUTING` Chain의 `nat` Table에 적용                                                           |
|              |                | - 동적으로 IP 주소 mapping                                                                             |

## 명령 예시
- `iptables -I INPUT -s 192.168.56.2 -j ACCEPT` 이 ip로 들어오는 모든 수신 허용
- `iptables -A INPUT -p icmp -j DROP` 내부에서 들어오는 icmp 패킷을 드랍
- `iptables -A INPUT -p tcp --dport 80 -j DROP` http 내부 패킷 드랍
- `iptables -A OUTPUT -o enp0s3 -s 10.0.2.15 -p tcp --dport 443 -j DROP` https 외부 패킷 드랍

*대부분의 서버들은 ping 요청을 해도 응답을 안하도록 설계되어있음*
*확인하기 위해서 `www.google.com`으로 핑을 보낼 것.* 


## 포트 번호확인
- `netstat -na` 포트 변호 확인
- `netstat -nat` t로 시작하는 포트번호 확인
- `netstat -natp` 그 중에서 프로세스 ID 확인




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
