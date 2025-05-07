---
created: 2025-04-17 15:32
tags:
  - 파이썬/모듈
---

---

## 개요

리스트에 있는 각 원소의 개수를 세는 함수.

`collections`에 있는 `Counter`를 호출하는 게 편함

## 사용 예시
```python
from collections import Counter

my_list = ['apple', 'banana', 'apple', 'orange', 'banana', 'apple']

# Counter를 사용하여 각 원소의 개수를 셉니다.
element_counts = Counter(my_list)

# Counter 객체는 딕셔너리처럼 사용할 수 있습니다.
print(element_counts)
# 출력: Counter({'apple': 3, 'banana': 2, 'orange': 1})

# 만약 표준 dict 타입이 꼭 필요하다면 dict()로 변환할 수 있습니다.
element_counts_dict = dict(element_counts)
print(element_counts_dict)
# 출력: {'apple': 3, 'banana': 2, 'orange': 1}
```


## 라이브러리 없이 구현

```python
def count_list_elements(input_list):
  """
  라이브러리 없이 리스트의 각 원소 개수를 세어 딕셔너리로 반환합니다.

  Args:
    input_list: 개수를 셀 리스트.

  Returns:
    원소를 키로, 개수를 값으로 가지는 딕셔너리.
  """
  element_counts = {} # 결과를 저장할 빈 딕셔너리
  for item in input_list:
    if item in element_counts:
      element_counts[item] += 1 # 이미 키가 있으면 카운트 1 증가
    else:
      element_counts[item] = 1 # 처음 나오는 키면 1로 초기화
  return element_counts
```


딕셔너리에 접근할 때, `get(key,defalut_value)`의 형식으로 접근 가능










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
