---
created: 2025-04-15 14:23
tags:
  - 리눅스
---

---
## **tcpdump 사용법 및 주요 프로토콜**

### **1. `tcpdump` 개요**

- **정의:** 커맨드 라인 기반 네트워크 패킷 분석 도구. 네트워크 인터페이스의 패킷 캡처 및 표시.
- **목적:** 네트워크 문제 해결, 통신 분석, 보안 감사 등.
- **특징:** 실시간 캡처/분석, 파일 저장/읽기, 필터링 기능 제공. 관리자 권한 필요.

### **2. 주요 네트워크 프로토콜 (관련)**

- **ARP (Address Resolution Protocol):**
	- 로컬 네트워크에서 IP 주소 -> MAC 주소 변환.
- **ICMP (Internet Control Message Protocol)**
	- 오류 보고 및 제어 메시지 전달 (예: `ping`).
- **DNS (Domain Name System)**
	- 도메인 이름 <-> IP 주소 변환 (주로 UDP/TCP 53).
- **DHCP (Dynamic Host Configuration Protocol)**
	- IP 주소 및 네트워크 설정 자동 할당 (UDP 67, 68).

### **3. `tcpdump` 기본 사용법 및 주요 옵션**

- **기본 형식:** `tcpdump [옵션...] [필터 표현식(expression)]`
- **주요 옵션:**


| 옵션 (Option)       | 핵심 설명                                                              |
| :------------------ | :--------------------------------------------------------------------- |
| `-i <인터페이스명>` | 패킷 캡처할 네트워크 인터페이스 지정 (예: `eth0`, `any`)               |
| `-n`                | 주소/포트를 이름 변환 없이 숫자로 표시 (`-nn`: 둘 다 변환 안 함)        |
| `-v`, `-vv`, `-vvv` | 출력 정보 상세 수준 높임                                               |
| `-c <숫자>`         | 지정된 개수만큼 패킷 캡처 후 종료                                      |
| `-w <파일명.pcap>`  | 캡처 내용을 화면 대신 파일에 저장 (바이너리 형식)                        |
| `-r <파일명.pcap>`  | 파일에 저장된 패킷 내용 읽기                                           |
| `-A`                | 패킷 내용을 ASCII 문자로 출력                                          |
| `-X`                | 패킷 내용을 Hex(16진수)와 ASCII로 함께 출력                            |
| `-s <크기>`         | 캡처할 패킷의 최대 크기 지정 (`0`: 전체 크기, 기본값 주의)             |

### **4. 패킷 필터링 (Expressions)**

- **BPF(Berkeley Packet Filter) 문법**을 사용하여 원하는 패킷만 선별 캡처.
- **주요 필터 요소:**


| 요소 (Element)       | 설명 및 예시                                                                                |
| :----------------- | :------------------------------------------------------------------------------------- |
| **타입 (Type)**      | `host <IP/Hostname>` (특정 호스트), `net <Network/Mask>` (네트워크 대역), `port <Number>` (포트 번호) |
| **방향 (Direction)** | `src` (출발지), `dst` (목적지)                                                               |
| **프로토콜 (Proto)**   | `tcp`, `udp`, `icmp`, `arp` 등 명시적 프로토콜 지정                                              |
| **논리 연산 (Logic)**  | `and` (`&&`), `or` (`\|\|`), `not` (`!`) 로 조건 조합. 괄호 `()` 사용 가능 (셸 이스케이프 `\(` `\)` 필요) |

**필터 조합 예시:**

- `src host 10.0.0.5 and dst port 80` (출발지가 10.0.0.5이고 목적지 포트가 80)
- `port 443 or port 80` (포트가 443 또는 80)
- `icmp and not host 192.168.1.1` (ICMP 트래픽 중 호스트 192.168.1.1 제외)

### **5. 프로토콜별 캡처 예시**

- **ARP 캡처:** `tcpdump -n arp`
- **ICMP 캡처:** `tcpdump -n icmp`
- **DNS 캡처 (UDP 53):** `tcpdump -n udp port 53`
- **DHCP 캡처 (UDP 67, 68):** `tcpdump -n 'udp port 67 or udp port 68'`
- **특정 호스트 HTTP 트래픽 (ASCII 내용):** `tcpdump -n -A host www.example.com and tcp port 80`

### **6. 기타 예제**

- eth0 인터페이스의 ARP 패킷만 캡처 (주소 변환 안 함)
- `sudo tcpdump -i eth0 arp -n`

- eth0 인터페이스의 ICMP 패킷만 캡처
- `sudo tcpdump -i eth0 icmp -n`

- 특정 호스트(예: 8.8.8.8)와의 ICMP 통신만 캡처
- `sudo tcpdump -i eth0 icmp and host 8.8.8.8 -n`

- eth0 인터페이스의 53번 포트(DNS) 트래픽 캡처
- `sudo tcpdump -i eth0 port 53 -n`

- 특정 DNS 서버(예: 8.8.8.8)와의 DNS 통신만 캡처 (내용 ASCII로 보기)
- `sudo tcpdump -i eth0 host 8.8.8.8 and port 53 -nA`

- eth0 인터페이스의 67번 또는 68번 포트(DHCP) 트래픽 캡처 (자세히 보기)
- `sudo tcpdump -i eth0 'port 67 or port 68' -nvv`

- eth0 인터페이스, 특정 호스트(192.168.0.10)의 모든 트래픽 캡처
- `sudo tcpdump -i eth0 host 192.168.0.10 -n`

- eth0 인터페이스, 특정 호스트(192.168.0.10)와 특정 포트(80) 통신 캡처
- `sudo tcpdump -i eth0 host 192.168.0.10 and port 80 -n`

- eth0 인터페이스의 모든 TCP 트래픽 캡처, 내용을 Hex/ASCII로 보기
- `sudo tcpdump -i eth0 tcp -nX`

- 캡처 결과를 파일로 저장
- `sudo tcpdump -i eth0 -w my_capture.pcap`

- 파일에서 특정 조건(예: TCP 포트 443) 패킷 읽어오기
- `tcpdump -r my_capture.pcap 'tcp port 443' -n`

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
