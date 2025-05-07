---
created: 2025-04-10 11:18
tags:
  - 파이썬/모듈
  - 자료구조
---

---
## 개요

- 파이썬에서 큐와 스택을 효율적으로 구현할 수 있는 자료구조
- 이름은 double-ended queue (양방향 큐)의 줄임말.
- `deque`는 `collections` 모듈에 포함돼 있다. 


## 🔷 1. 기본 개념

- `deque`는 **양쪽 끝에서 삽입과 삭제가 모두 O(1)** 의 시간복잡도를 가짐.
- 리스트와 달리, **앞쪽에서 원소를 제거하거나 추가해도 느려지지 않음**.
- 내부적으로는 **이중 연결 리스트에 가까운 구조**.

```python
from collections import deque

dq = deque()
dq.append(1)       # 오른쪽 끝에 추가
dq.appendleft(2)   # 왼쪽 끝에 추가
print(dq)          # deque([2, 1])

dq.pop()           # 오른쪽 끝 제거
dq.popleft()       # 왼쪽 끝 제거
```

---
## 🔷 2. 주요 메서드 요약

| 메서드                | 설명                                     |
| ------------------ | -------------------------------------- |
| `append(x)`        | 오른쪽 끝에 x 추가                            |
| `appendleft(x)`    | 왼쪽 끝에 x 추가                             |
| `pop()`            | 오른쪽 끝 요소 제거 및 반환                       |
| `popleft()`        | 왼쪽 끝 요소 제거 및 반환                        |
| `extend(iter)`     | 오른쪽에 iterable의 모든 요소 추가                |
| `extendleft(iter)` | 왼쪽에 iterable의 모든 요소 추가 (순서가 반대로 들어감)   |
| `rotate(k)`        | 모든 요소를 k만큼 회전 (양수: 오른쪽, 음수: 왼쪽)        |
| `maxlen`           | 생성 시 길이 제한을 줄 수 있음 (초과 시 자동으로 앞 요소 제거) |

---
## 🔷 3. 큐 / 스택 구현 예시

✔️ 큐 (FIFO)
```python
q = deque()
q.append('a')     # enqueue
q.append('b')
print(q.popleft())  # dequeue → 'a'
```

✔️ 스택 (LIFO)
```python
s = deque()
s.append('a')     # push
s.append('b')
print(s.pop())     # pop → 'b'
```

---
## 🔷 4. 성능

- `deque.popleft()`는 리스트의 `pop(0)`보다 훨씬 빠름.
- 리스트는 앞 요소를 제거하면 모든 요소를 앞으로 당겨야 해서 **O(n)** 시간이 걸림.
- `deque`는 앞 뒤 모두에서 **O(1)** 로 작업할 수 있어서 대용량 데이터 처리에 적합함.




---
# 링크 
## 🔗 아웃링크
```dataview
LIST WITHOUT ID outgoing
FROM ""
WHERE file = this.file
FLATTEN file.outlinks as outgoing
SORT outgoing ASC
```
## 🔙 백링크
```dataview
LIST WITHOUT ID file.link
WHERE contains(file.outlinks, this.file.link)
SORT file.link ASC
```
