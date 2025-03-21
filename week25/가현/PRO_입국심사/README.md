## 1. 문제 요약

여러 명이 입국심사를 기다리고 있으며, 각 심사관의 심사 속도가 다릅니다. 모든 사람이 심사를 받는 데 걸리는 최소 시간을 구하는 것이 목표입니다.

> 입력<br/>
> n: 심사를 받아야 하는 사람 수 (1 ≤ n ≤ 1,000,000,000)<br/>
> times: 각 심사관의 심사 시간 배열 (1 ≤ time ≤ 1,000,000,000)

> 출력: 모든 사람이 심사를 완료하는 최소 시간.

## 2. 핵심 풀이 방법

이진 탐색 (Binary Search)

- 문제의 최소 시간과 최대 시간을 설정한 뒤, 중간값(mid)에서 가능한 사람 수를 계산
- 계산 결과에 따라 범위를 좁혀가는 방식으로, 최적의 최소 시간을 찾는다.

> 이진 탐색을 선택한 이유:
> n과 times의 값이 매우 크기 때문에, 완전 탐색은 비효율적입니다.
> 시간 복잡도를 로그 단위로 줄일 수 있는 이진 탐색이 적합합니다.

## 3. 상세 풀이 방법

### 1. 초기 범위 설정

- 최소 시간(low): 1로 설정
- 최대 시간(high): 가장 느린 심사관(times의 최댓값)이 모든 사람을 심사하는 시간

```
ℎ𝑖𝑔ℎ = max(𝑡𝑖𝑚𝑒𝑠) × 𝑛
```

### 2. 이진 탐색 반복

- low가 high보다 작거나 같을 때까지 반복
- 중간값 mid를 계산:

```
𝑚𝑖𝑑=(𝑙𝑜𝑤+ℎ𝑖𝑔ℎ)/2
```

- mid 시간 안에 모든 심사관이 처리할 수 있는 총 인원을 계산:

```
𝑡𝑜𝑡𝑎𝑙=∑𝑡𝑖𝑚𝑒∈𝑡𝑖𝑚𝑒𝑠⌊𝑚𝑖𝑑/𝑡𝑖𝑚𝑒⌋
```

### 3. 조건 확인 및 범위 조정

- total >= n인 경우: 현재 시간으로도 모든 사람을 처리할 수 있으므로 더 짧은 시간(high)을 탐색. 이때 answer를 업데이트합니다.
- total < n인 경우: 더 긴 시간(low)을 탐색합니다.

## 4. 시간 복잡도

- 이진 탐색 횟수: 최대 탐색 범위가
  `max(𝑡𝑖𝑚𝑒𝑠)×𝑛`이므로, 이진 탐색의 깊이는 `𝑂(log⁡(max(𝑡𝑖𝑚𝑒𝑠)×𝑛))`
- 총 계산량: 각 단계에서 모든 심사관에 대해 처리가 가능하므로, 심사관의 수를 𝑚이라 하면 단계별 계산량은 `𝑂(𝑚)`
- 최종 시간 복잡도: `𝑂(𝑚⋅log⁡(max(𝑡𝑖𝑚𝑒𝑠)×𝑛))`
