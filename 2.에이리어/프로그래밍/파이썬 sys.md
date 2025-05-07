---
created: 2025-04-10 18:14
tags:
  - 파이썬/모듈
---

---
## 개요
`sys` 모듈은 파이썬 인터프리터와 관련된 기능을 제공하는 표준 라이브러리.
주로 **입출력 최적화**, **명령줄 인자 처리**, **인터프리터 제어** 등을 위해 사용됨.

---
## 🔷 1. 주요 기능 요약

### 1. `sys.stdin`, `sys.stdout`, `sys.stderr`

- 표준 입출력 스트림을 직접 다룰 수 있음.
- 특히 `sys.stdin.readline()`은 `input()`보다 **빠른 입력 처리**가 가능해서 **알고리즘 문제 풀이**에 자주 사용됨.

```python
import sys

line = sys.stdin.readline().strip()
print(f"입력: {line}")
```

### 2. `sys.argv`

- 명령줄 인자를 리스트 형태로 가져옴.
- `argv[0]`은 실행한 스크립트 이름이고, 뒤는 사용자가 전달한 인자들.


```python
# $ python script.py hello world
import sys
print(sys.argv)  # ['script.py', 'hello', 'world']

```

### 3. `sys.exit()`

- 프로그램을 강제로 종료시킴. 인자로 상태 코드 전달 가능 (`0`은 정상 종료).

```python
import sys
if error:
    sys.exit("에러 발생으로 종료")
```

### 4. `sys.path`

- 모듈을 찾을 때 사용하는 경로 목록.
- 새로운 경로를 추가해서 외부 모듈을 불러올 때 활용 가능.

```python
import sys
sys.path.append('/my/custom/module/path')
```

### 5. `sys.version`, `sys.platform`

- 파이썬 버전이나 실행 중인 OS 정보를 확인 가능.

```python
import sys
print(sys.version)
print(sys.platform)
```

---

## 🔷 2. 입출력 스트림 상세

### 1. `sys.stdin`

- 표준 입력을 처리하는 객체임.
- `input()` 함수보다 빠름.
- `sys.stdin.readline()` 또는 `sys.stdin.read()` 형태로 사용됨.

#### ✅ `sys.stdin.readline()`

- 한 줄씩 입력을 읽음 (`\n` 포함됨).
- 문자열의 끝에 줄바꿈 문자인 `\n`이 붙어 있으므로, 필요에 따라 `.strip()`으로 제거해야 함.
- 반복문에서 줄 단위 입력 처리 시 유용함.

```python
import sys
line = sys.stdin.readline()
print(line)         # 'abc\n'
print(line.strip()) # 'abc'
```

#### ✅ `sys.stdin.read()`

- 전체 입력을 한 번에 읽어옴 (`Ctrl+D` 또는 EOF 입력까지).
- 주로 한 번에 많은 양의 입력을 받을 때 사용됨.
- 줄바꿈도 함께 포함되므로 후처리 필요.

```python
import sys
data = sys.stdin.read()
lines = data.strip().split('\n')
```
 
 **차이 요약**:

| 함수           | 입력 단위 | 줄바꿈 포함 | 사용 목적    |
| ------------ | ----- | ------ | -------- |
| `readline()` | 한 줄   | 포함     | 줄 단위 입력  |
| `read()`     | 전체    | 포함     | 전체 입력 처리 |

### 2. `sys.stdout.write()`

- 표준 출력을 처리하는 객체임.
- `print()`보다 빠르며, 문자열만 출력 가능.
- 자동 줄바꿈(`\n`)이 없음 → 수동으로 넣어줘야 함.

```python
import sys
sys.stdout.write('hello\n')
```

| 함수                   | 출력 대상  | 자동 줄바꿈          | 속도  |
| -------------------- | ------ | --------------- | --- |
| `print()`            | 모든 자료형 | 있음 (`end='\n'`) | 느림  |
| `sys.stdout.write()` | 문자열만   | 없음 (직접 작성해야 함)  | 빠름  |

### 3. 알고리즘 문제풀이 사용 예

```python
import sys #입력
N = int(sys.stdin.readline()) 
arr = list(map(int, sys.stdin.readline().split()))
```

```python
import sys #출력
sys.stdout.write('\n'.join(map(str, result)) + '\n')
```

---
## 🔷 3. 기타 유용한 속성

|속성/함수|설명|
|---|---|
|`sys.getrecursionlimit()`|최대 재귀 깊이 확인|
|`sys.setrecursionlimit(n)`|재귀 한도 설정 (DFS 같은 곳에서 사용)|
|`sys.modules`|현재 로드된 모듈의 딕셔너리|
|`sys.getsizeof(obj)`|객체의 바이트 크기 반환|




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
