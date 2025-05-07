---
created: 2025-04-16 13:45
tags:
  - SQL
---

---
SQL DELETE 문법


`DELETE FROM board` 
- 삭제할 데이터들을 전부 로그에 집어넣어서 나중에 복원할 가능성을 열어둠.
`TRUCATE board` 
- 삭제할 데이터를 로그에 집어넣지 않고 삭제. 복원 불가능






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
