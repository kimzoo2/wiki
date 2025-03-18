## 문제
1이상 K이하의 숫자를 하나 고르는 행위를 N번 반복하여 나올 수 있는 모든 서로 다른 순서쌍을 구해주는 프로그램을 작성해보세요. 단, 연속하여 같은 숫자가 3번 이상 나오는 경우는 제외합니다.

예를 들어 K이 2, N이 3인 경우 다음과 같이 6개의 조합이 가능합니다.

- 1 1 2
- 1 2 1
- 1 2 2
- 2 1 1
- 2 1 2
- 2 2 1

### 최초 구현
```java
import java.io.*;

public class Main {

    private static int n,k;

	private static boolean isDuplicated(int cnt, int n, int[] arr){
		if(cnt < 1) return false;
		for (int i = cnt; i >= 0 && i > cnt - 2; i--) {
			if(arr[i] != n) return false;
		}
		return true;
	}
	private static void bfs(int cnt, int[] arr) {
		if(k == cnt){
			if(isDuplicated(cnt-2, arr[cnt-1], arr)) return;
			for (int i : arr) {
				System.out.print(i + " ");
			}
			System.out.println();
			return;
		}

		for (int i = 1; i <= n; i++) {
			if(isDuplicated(cnt-1, i, arr)) continue;
			arr[cnt] = i;
			bfs(cnt+1, arr);
			arr[cnt] = 0;
		}
	}

    public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] inputs = br.readLine().split(" ");
		n = Integer.parseInt(inputs[0]); // 숫자
		k = Integer.parseInt(inputs[1]); // 자릿수
		int[] arr = new int[k];
		for (int i = 1; i <= n; i++) {
			arr[0] = i;
			bfs(1, arr);
		}
	}
}
```
### 구현 의도 
자릿수 별로 n까지의 숫자를 채우고 3이상일 때는 앞의 2개가 동일한 숫자인지 판별하고자 했다.

### 결과
Runtime: **1102ms**
Memory: **23MB**
### 보완
하지만, `if(isDuplicated(cnt-1, i, arr)) continue;` 코드가 반복적으로 들어간다는 문제가 있었다.

```java
import java.io.*;

public class Main {

    private static int n,k;

	private static boolean isDuplicated(int cnt, int n, int[] arr){
		if(cnt < 1) return false;
		for (int i = cnt; i >= 0 && i > cnt - 2; i--) {
			if(arr[i] != n) return false;
		}
		return true;
	}
	private static void bfs(int cnt, int[] arr) {
		if(k == cnt){
			for (int i : arr) {
				System.out.print(i + " ");
			}
			System.out.println();
			return;
		}

		for (int i = 1; i <= n; i++) {
			if(isDuplicated(cnt-1, i, arr)) continue;
			arr[cnt] = i;
			bfs(cnt+1, arr);
			arr[cnt] = 0;
		}
	}

    public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] inputs = br.readLine().split(" ");
		n = Integer.parseInt(inputs[0]); // 숫자
		k = Integer.parseInt(inputs[1]); // 자릿수
		int[] arr = new int[k];
		bfs(0, arr);

	}
}
```

main 메소드에서 자릿수를 채우지 않고 bfs에서만 자릿수를 채우게 만들었다.

### 결과
결과도 유사하다.
Runtime: **1129ms**
Memory: **24MB**