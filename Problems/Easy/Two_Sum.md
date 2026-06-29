### 💡 문제 핵심 요약
- 정수 배열 `nums`와 `target` 값이 주어졌을 때, 합해서 `target`이 되는 두 숫자의 인덱스를 찾아 반환하는 문제.

### 🧠 접근 방식 (Logic)
- **방법 1 (Brute Force)**: 이중 for문을 사용하여 모든 경우의 수를 탐색. (시간 복잡도: O(n^2))
- **방법 2 (Hash Map)**: 딕셔너리를 사용하여 한 번의 순회로 필요한 값을 찾음. (시간 복잡도: O(n))

### 💻 코드 (Python)
```python
def twoSum(nums, target):
    prev_map = {}  # {값: 인덱스}
    
    for i, n in enumerate(nums):
        diff = target - n
        if diff in prev_map:
            return [prev_map[diff], i]
        prev_map[n] = i
    return []
```
### 배울 점 & 반성
- 문제를 보고 머릿속에 그려진 논리를 구현하는 실력 키우기
- for문 range 범위값 잘 설정
- for문이나 if문에서 그냥 끝내버리고 싶으면 return을 이용해서 끝낼수도 있다
- 리스트는 순서가 중요하거나 기억을 해야할 때 쓰는 자료구조이고 해시는 빠르게 검색하거나 있는지 없는지만 볼 때 쓰는 자료구조
