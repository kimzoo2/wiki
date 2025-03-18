## DP란?
Dynamic Programming(동적계획법)의 약자로, 복잡한 문제를 **작은 문제로 나누어 먼저 해결한 뒤그 결과를 이용하여 문제를 해결하는 방법**을 의미한다.
![[스크린샷 2024-07-17 오후 4.42.54.png]]

가장 대표적으로 피보나치의 수 문제가 존재한다.

## 해결방법
- 재귀함수
- for문

### memoization - 탑다운 방법
- 값을 기록하고, 그 기록한 값을 참조하는 것
``` java
function fibbo(n)
    if memo[n] != -1           // 이미 n번째 값을 구해본 적이 있다면
        return memo[n]         // memo에 적혀있는 값을 반환해줍니다.
    if n <= 2                  // n이 2이하인 경우에는 종료 조건이므로 
        memo[n] = 1            // 해당하는 숫자를 memo에 넣어줍니다.
    else                       // 종료조건이 아닌 경우에는 
        memo[n] = fibbo(n - 1) + fibbo(n - 2)   // 점화식을 이용하여 답을 구한 뒤
                                                // 해당 값을 memo에 저장해줍니다.
    return memo[n]             // memo 값을 반환합니다.

```

### Tabulation - 바텀업 (낮은 수에서 높은 수로)
```java
set dp = [0, 0, 0, ...]

dp[1] = 1
dp[2] = 1

for i = 3 ... i <= n:
    dp[i] = dp[i - 1] + dp[i - 2]

print(dp[n])

```

### DP 푸는 방법

- memoization 하거나 tabulation 둘 중 하나 선택한다
- Math.min 하거나 Math.max로 경우의 수를 채워간다