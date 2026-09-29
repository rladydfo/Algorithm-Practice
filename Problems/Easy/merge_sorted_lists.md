# Merge Two Sorted Lists

## Problem

두 개의 정렬된 연결 리스트 `list1`, `list2`를 하나의 정렬된 연결 리스트로 합친다.

---

## My Approach

두 리스트는 이미 정렬되어 있으므로 전체를 다시 정렬할 필요가 없다.

1. `list1`과 `list2`의 현재 노드 값을 비교한다.
2. 더 작은 노드를 결과 연결 리스트에 연결한다.
3. 결과 리스트의 포인터를 한 칸 이동한다.
4. 선택한 입력 리스트의 포인터도 한 칸 이동한다.
5. 둘 중 하나가 끝날 때까지 반복한다.
6. 한쪽 리스트가 남으면 남은 부분을 그대로 결과 뒤에 연결한다.

---

## Solution

```python
class Solution:
    def mergeTwoLists(self, list1, list2):
        a = ListNode()
        b = a

        while list1 and list2:
            if list1.val < list2.val:
                b.next = list1
                b = b.next
                list1 = list1.next
            else:
                b.next = list2
                b = b.next
                list2 = list2.next

        if list1:
            b.next = list1
        else:
            b.next = list2

        return a.next
```

---

## What I Learned

### 1. Linked List는 Python List와 다르다

Python list에서는:

```python
a = [1, 2, 3]
a[0]
```

처럼 인덱스로 접근할 수 있다.

하지만 Linked List에서는 각 노드가 다음 노드를 가리킨다.

```text
[1] -> [2] -> [3] -> None
```

따라서 `list1[0]`이 아니라 다음과 같이 사용한다.

```python
list1.val
```

현재 노드의 값을 가져온다.

---

### 2. `.val`과 `.next`

```python
node.val
```

현재 노드에 저장된 값이다.

```python
node.next
```

다음 노드를 가리키는 참조(reference)이다.

예:

```text
node
 ↓
[1] -> [2] -> [3]
```

여기서:

```python
node.val
```

은 `1`이고,

```python
node.next
```

는 `[2]` 노드를 가리킨다.

---

### 3. `.next`와 `node = node.next`는 다르다

```python
node.next
```

는 다음 노드를 가리키는 참조를 가져오는 것이다.

반면:

```python
node = node.next
```

는 `node` 자체가 다음 노드를 가리키도록 이동시키는 것이다.

```text
Before

node
 ↓
[1] -> [2] -> [3]


After node = node.next

       node
        ↓
[1] -> [2] -> [3]
```

---

### 4. `b.next = list1`과 `b = b.next`의 차이

이 두 줄은 역할이 완전히 다르다.

```python
b.next = list1
```

현재 `b` 노드의 다음 노드를 `list1`에 연결한다.

```text
b
↓
[0] -> [1]
        ↑
      list1
```

그리고:

```python
b = b.next
```

를 하면 `b` 자체가 방금 연결한 노드로 이동한다.

```text
       b
       ↓
[0] -> [1]
```

따라서 다음 노드를 계속 뒤에 연결할 수 있다.

---

### 5. Dummy Node

```python
a = ListNode()
b = a
```

`a`는 dummy node이다.

처음에는:

```text
a, b
 ↓
[0]
```

둘 다 같은 노드를 가리킨다.

하지만 역할을 다르게 사용한다.

- `a`: 결과 연결 리스트의 시작점을 기억한다.
- `b`: 노드를 연결하면서 계속 앞으로 이동한다.

예:

```text
a                 b
↓                 ↓
[0] -> [1] -> [2] -> [3]
```

`a`를 움직이지 않기 때문에 마지막에 결과의 시작점을 찾을 수 있다.

---

### 6. 왜 `return a.next`인가?

최종적으로:

```text
a
↓
[0] -> [1] -> [1] -> [2] -> [3] -> [4] -> [4]
```

`[0]`은 실제 데이터가 아니라 편의를 위해 만든 dummy node이다.

따라서:

```python
return a.next
```

를 사용한다.

`a.next`는 실제 결과의 첫 번째 노드를 가리키고, 그 노드부터 뒤의 모든 노드가 `.next`로 연결되어 있다.

---

### 7. 입력 리스트의 포인터도 이동해야 한다

처음에는 다음 부분을 빠뜨렸다.

```python
list1 = list1.next
```

또는:

```python
list2 = list2.next
```

노드를 결과에 연결한 뒤 해당 입력 리스트도 이동해야 한다.

예:

```python
b.next = list1
b = b.next
list1 = list1.next
```

이 세 줄을 하나의 흐름으로 생각할 수 있다.

```text
노드 연결
   ↓
결과 포인터 이동
   ↓
사용한 입력 포인터 이동
```

입력 포인터를 이동하지 않으면 계속 같은 노드끼리 비교하게 되어 반복문이 끝나지 않는다.

---

## Important Pattern

이번 문제에서 가장 중요한 패턴:

```python
b.next = list1       # 노드를 결과에 연결
b = b.next           # 결과 포인터 이동
list1 = list1.next   # 입력 포인터 이동
```

즉:

> 연결한다 -> 결과 포인터를 옮긴다 -> 사용한 입력 포인터를 옮긴다

---

## Things I Was Confused About

### `b`가 필요한 이유

`a`만 이동시키면 결과 리스트의 시작점을 잃어버릴 수 있다.

따라서:

```python
a = ListNode()
b = a
```

처럼 두 개의 참조를 사용한다.

`a`는 시작점에 남겨두고 `b`만 이동시킨다.

---

### 연결 리스트 전체가 변수에 저장되는 것인가?

아니다.

```python
b = b.next
```

를 실행한다고 해서 `b`에 뒤의 연결 리스트 전체가 복사되는 것이 아니다.

`b`는 단지 하나의 노드를 가리킨다.

하지만 그 노드의 `.next`를 따라가면 다음 노드로 이동할 수 있기 때문에 결과적으로 뒤의 모든 노드에 접근할 수 있다.

---

## What I Need to Study More

- Linked List의 기본 구조
- `ListNode`가 어떻게 동작하는지
- `.val`과 `.next`
- Reference와 일반 값의 차이
- 포인터/참조를 이동시키는 개념
- Dummy Node 패턴
- Linked List traversal
- Linked List에서 노드를 삽입하거나 삭제하는 방법

---

## Complexity

두 리스트의 각 노드를 최대 한 번씩 확인한다.

- Time Complexity: `O(n + m)`
- Extra Space Complexity: `O(1)`

새로운 리스트의 모든 노드를 다시 만드는 것이 아니라 기존 노드들의 연결을 바꾸어 사용하기 때문이다.

---

## Review

며칠 뒤 코드를 보지 않고 다시 풀어본다.

다시 풀 때 다음 내용을 기억하는지 확인한다.

1. 왜 Python list처럼 `list1[0]`을 사용할 수 없는가?
2. `node.val`은 무엇인가?
3. `node.next`는 무엇인가?
4. `b.next = list1`과 `b = b.next`의 차이는 무엇인가?
5. 왜 dummy node를 사용하는가?
6. 왜 `return a.next`인가?
7. 노드를 선택한 후 왜 `list1 = list1.next` 또는 `list2 = list2.next`가 필요한가?

### 핵심 한 줄

> 작은 노드를 연결하고 -> 결과 포인터를 이동하고 -> 선택한 입력 포인터를 이동한다.
