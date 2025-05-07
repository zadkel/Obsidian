---
created: 2025-04-18 14:35
tags:
---

---

## MITRE ATT&CK (마이터 어택)

- **실제 관찰된 사이버 공격 사례들을 기반으로 한 공격자들의 행동 방식(전술 및 기술)에 대한 포괄적인 지식 베이스(knowledge base)**
- (마이터 어택)은 미국의 비영리 연구 개발 조직인 **MITRE Corporation**에서 개발하고 유지 관리 중

**ATT&CK**은 **A**dversarial **T**actics, **T**echniques, and **C**ommon **K**nowledge** 의 약자

## **Adversarial Tactics (공격 전술):**
- 공격자가 특정 목표를 달성하기 위해 사용하는 상위 수준의 계획 또는 목적.
- 종류:
	- 초기 접근(Initial Access)
	- 실행(Execution)
	- 지속성(Persistence)
	- 권한 상승(Privilege Escalation)
	- 방어 회피(Defense Evasion)
	- 자격 증명 접근(Credential Access)
	- 탐색(Discovery)
	- 횡적 이동(Lateral Movement)
	- 수집(Collection)
	- 명령 및 제어(Command and Control)
	- 데이터 유출(Exfiltration)
	- 영향(Impact)
- ATT&CK 프레임워크에서는 이것들이 보통 매트릭스의 열(Column)에 해당 됨.

## **Techniques (공격 기술):** 
- 공격자가 각 전술(Tactics)을 달성하기 위해 사용하는 구체적인 방법 또는 수단.
- 예시:
	- 초기 접근 전술을 위한 피싱(Phishing)
	- 실행 전술을 위해 파워셸(PowerShell)
- ATT&CK 매트릭스에서는 각 전술 열 아래에 나열됨.
- 많은 기술들은 더 세분화된 하위 기술(Sub-techniques)로 나뉨.

## **Common Knowledge (공통 지식):** 
- 실제 세상에서 관찰되고 문서화된 공격 사례들을 기반으로 만들어진 공통된 지식


**쉽게 말해, MITRE ATT&CK은:**

- 사이버 공격자들이 목표 시스템을 침투하고, 내부에서 활동하며, 최종 목표를 달성하기까지 **어떤 단계(전술)를 거치고, 각 단계에서 어떤 구체적인 방법(기술)들을 사용하는지**를 체계적으로 정리해 놓은 "공격 백과사전" 또는 "공격 플레이북"과 같습니다.
- 보안 전문가(방어팀, 위협 헌팅팀, 위협 인텔리전스 분석가, 레드팀 등)들이 공격자의 행동을 이해하고, 탐지하며, 대응 전략을 수립하는 데 **공통된 언어와 프레임워크**를 제공합니다.

## **주요 특징 및 활용 방안:**

- **다양한 플랫폼 포괄:** 기업 환경(Windows, macOS, Linux, 클라우드 등), 모바일(iOS, Android), 산업 제어 시스템(ICS) 등 다양한 환경에 대한 별도의 매트릭스를 제공.
- **실제 사례 기반:** 이론이 아닌 실제 공격 그룹 및 악성코드 캠페인에서 관찰된 TTP(Tactics, Techniques, Procedures)를 기반으로 구성.
- **지속적인 업데이트:** 새로운 위협과 기술이 등장함에 따라 MITRE에서 지속적으로 업데이트.
- **활용 분야:**
    - **위협 인텔리전스(Threat Intelligence):** 공격 그룹 프로파일링 및 TTP 분석.
    - **위협 헌팅(Threat Hunting):** 특정 공격 기술에 대한 탐지 시나리오 개발.
    - **보안 관제 및 탐지(Security Monitoring & Detection):** SIEM 룰, EDR 탐지 로직 개발 및 개선.
    - **보안 아키텍처 및 정책 평가:** 현재 보안 솔루션 및 정책이 어떤 공격 기술을 커버하는지 갭 분석(Gap Analysis).
    - **레드팀 및 침투 테스트(Red Teaming & Penetration Testing):** 실제 공격자 행동을 모방한 시나리오 설계.
    - **보안 도구 평가:** 보안 솔루션이 특정 ATT&CK 기술을 얼마나 잘 탐지/방어하는지 평가.
    - **사고 대응(Incident Response):** 침해 사고 발생 시 공격자의 단계를 파악하고 대응 계획 수립.

## 결론:
- MITRE ATT&CK은 현대 사이버 보안 분야에서 공격자의 행동을 이해하고 이에 효과적으로 대응하기 위한 프레임 워크





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
