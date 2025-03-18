### 문제
N개의 보석의 정보가 주어졌을 때, 보석을 적절하게 골라 무게의 합이 M을 넘지 않도록 하면서 가치의 합은 최대가 되도록 하는 프로그램을 작성해보세요.

예를 들어 N=4,M=20, 보석의 종류가 다음과 같이 주어졌다고 생각해봅시다.

### 최초 구현
```java
private static void knapsack() throws IOException {  
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
    String[] inputs = br.readLine().split(" ");  
    int n = Integer.parseInt(inputs[0]);  
    int m = Integer.parseInt(inputs[1]);  
  
    int[] W = new int[n];  
    int[] V = new int[n];  
  
    for (int i = 0; i < n; i++) {  
       String[] knapsackInformation = br.readLine().split(" ");  
       W[i] = Integer.parseInt(knapsackInformation[0]);  
       V[i] = Integer.parseInt(knapsackInformation[1]);  
    }  
  
    int[] dp = new int[m + 1];  
    Arrays.fill(dp, -1);  
    dp[0] = 0;  
  
    for (int i = 0; i < n; i++) {  
       for (int j = m; j >= 0; j--) {  
          if(dp[j] > -1){  
             if(j + W[i] <= m){  
                dp[j + W[i]] = Math.max(dp[j + W[i]], dp[j] + V[i]);  
             }  
          }  
       }  
    }  
  
    int max = 0;  
    for (int i = 0; i <= m; i++) {  
       max = Math.max(max, dp[i]);  
    }  
    System.out.println(max);  
}
```
### 구현 의도 
dp(i)에 i 기준으로 최대 배낭 가치를 저장
dp를 뒤에서부터 반복한 이유는 중복값을 제거하기 위함이다. 예를 들어, 앞에서부터 반복문을 시작했을 때 weight가 2, value가 1이라면 dp(2)에 1이 저장되고 2의 배수마다 1씩 더해진다.

- 앞에서부터 시작했을 때
![[8439b43a-87c7-44dc-93c9-06471c83e215.png]]
- 뒤에서 시작했을 때
![[스크린샷 2024-07-09 오후 12.15.27.png]]

### 결과
Runtime: **126ms**
Memory: **9MB**