### 문제 요약

- 던전을 탐험하려면 최소 필요 피로도를 만족해야 하고, 탐험 후 소모 피로도가 차감됨
- 주어진 초기 피로도 \(k\) 내에서 탐험 가능한 최대 던전 수를 구하는 문제
- 입력: 초기 피로도 \(k\)와 \([최소 필요 피로도, 소모 피로도]\)로 구성된 던전 리스트

---

### 아이디어

1. 모든 던전 탐험 순서를 고려해야 하므로 **순열 알고리즘**을 사용
2. 각 순서에 대해 초기 피로도 \(k\)를 기준으로 탐험 가능한 던전 수를 계산
3. **백트래킹**을 통해 순열을 생성하고, 탐험이 끝난 뒤 상태를 복원하여 다른 순서를 탐색

---

### 문제 풀이법

#### 1. 순열 생성

- **재귀적 백트래킹**을 사용해 던전 리스트의 가능한 모든 순서를 생성
- `visited` 배열로 이미 선택한 던전을 다시 선택하지 않도록 관리

#### 2. 탐험 로직

- 각 순열에 대해:
  - 초기 피로도 \(k\)에서 던전의 소모 피로도를 차감
  - 탐험 가능한 던전 수를 계산
  - 탐험할 수 없는 던전에 도달하면 루프 종료 후 최대 탐험 수를 업데이트

---

### 유의점

### 1. `result.add(current.toArray(new int[0][]))`의 의미

#### **(1). `current.toArray`의 역할**

- `current`는 `List<int[]>` 타입의 리스트로, 현재 생성 중인 순열을 저장함.
- `toArray`는 리스트를 배열로 변환하는 메서드로, 결과를 `int[][]` 배열 형태로 반환.

#### **(2). `new int[0][]`의 의미**

- `toArray` 메서드는 변환될 배열의 타입을 명시하기 위해 템플릿 배열이 필요함.
- `new int[0][]`는 크기가 0인 다차원 배열 템플릿을 생성.
  - 이 템플릿은 단순히 타입 정보를 제공하며, 실제 배열의 크기는 `current` 리스트의 크기에 따라 동적으로 결정됨.
  - 예를 들어, `current.size()`가 3일 경우, 결과 배열의 크기는 `int[3][]`로 확장됨.

#### **(3). 왜 `new int[0][]`를 사용하나?**

- `toArray` 메서드에 템플릿 배열을 전달하면, 내부적으로 리스트의 요소를 복사하여 새 배열을 생성.
- 만약 템플릿 배열 크기가 충분하면 그대로 사용하고, 부족하면 새 배열을 동적으로 생성.
- **장점**:
  - 템플릿 배열을 통해 타입 안정성을 제공.
  - 동적 크기 확장이 가능해 효율적.

#### **(4). 예제**

```java
List<int[]> current = new ArrayList<>();
current.add(new int[]{1, 2});
current.add(new int[]{3, 4});

// 리스트를 배열로 변환
int[][] result = current.toArray(new int[0][]);

// 결과: [[1, 2], [3, 4]]
```

### 2. **백트래킹(`remove`)의 필요성**

- 재귀 호출 후 상태를 복원하지 않으면 **다른 순서를 생성할 수 없음**
- 호출이 끝난 뒤 마지막에 추가된 던전을 삭제해 이전 상태로 되돌림

---

### 시간 복잡도 분석

#### 1. 순열 생성

- 던전의 개수를 \(n\)이라 하면, 가능한 순열의 총 개수는 n!.
- 각 호출에서 던전을 선택하며 백트래킹을 통해 모든 경우를 탐색.
- 순열 생성의 시간 복잡도:
  - O(n!)

#### 2. 탐험 로직

- 생성된 n!개의 순열에 대해, 각 순열의 던전을 탐험하며 탐험 가능한 던전 수를 계산.
- 던전 리스트의 크기가 n일 때, 각 순열에 대해 n번의 탐색.
- 탐험 로직의 시간 복잡도:
  - O(n \* n!)

#### 3. 총 시간 복잡도

- 순열 생성과 탐험 로직을 합산한 시간 복잡도는:
  - O(n!) + O(n _ n!) = O(n _ n!)

#### 4. 최악의 경우

- 문제에서 던전의 최대 개수는 n = 8.
- 최악의 경우 시간 복잡도 계산:
  - n! = 8! = 40,320
  - n _ n! = 8 _ 40,320 = 322,560

#### 5. 결론

- 최대 입력 크기에서도 총 약 322,560번의 연산으로 충분히 문제를 해결 가능.
- 입력 크기가 제한되어 있으므로 O(n \* n!) 복잡도를 가진 이 방식은 효율적으로 동작.
