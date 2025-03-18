## 문제
1이상 N이하의 숫자 중 M개의 숫자를 골라 만들 수 있는 모든 조합을 구해주는 프로그램을 작성해보세요.
예를 들어 N이 4, M이 3인 경우 다음과 같이 4개의 조합이 가능합니다.

- 1 2 3
- 1 2 4
- 1 3 4
- 2 3 4

### 최초 구현
```java
import java.io.*;
public class Main {

    static int N,m;

    private static void print(int[] arr) {
		for (int i = 0; i < m; i++) {
			System.out.print(arr[i] + " ");
		}
		System.out.println();
	}

    private static boolean isDuplicated(int num, int[] arr){
		for (int i = 0; i < m; i++) {
			if(num == arr[i]) return true;
		}
		return false;
	}

	private static void bfs3(int cnt, int[] arr){
		if(cnt == m){
			print(arr);
			return;
		}

		for (int i = 1; i <= N; i++) {
			if(!isDuplicated(i, arr)) {
				if(cnt > 0){
					if(arr[cnt - 1] < i){
						arr[cnt] = i;
						bfs3(cnt + 1, arr);
					}
				}else{
					arr[cnt] = i;
					bfs3(cnt + 1, arr);
				}
			}
			arr[cnt] = 0;
		}
	}

    public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] inputs = br.readLine().split(" ");
		N = Integer.parseInt(inputs[0]);
		m = Integer.parseInt(inputs[1]);
		int[] arr = new int[m];
		bfs3(0, arr);
	}
}
```
### 구현 의도 
자릿수별로 현재 숫자보다 더 큰 숫자가 오면 데이터를 추가한다. 이때 cnt가 0일 때는 큰지, 작은지 비교할 수 없기 때문에 그대로 삽입한다.

### 결과
Runtime: **184ms**
Memory: **9MB**
### 보완
하지만 기본개념을 응용하지 못했다는 생각이 드는 찰나, 다른 사람의 풀이를 보고 깨달았다. 현재 자릿수 -1과 비교하는 것이 아니라 현재 자릿수보다 큰 값만 bfs로 넘겨주면 된다고

```java
import java.io.*;
public class Main {

    static int N,m;

    private static void print(int[] arr) {
		for (int i = 0; i < m; i++) {
			System.out.print(arr[i] + " ");
		}
		System.out.println();
	}

    private static boolean isDuplicated(int num, int[] arr){
		for (int i = 0; i < m; i++) {
			if(num == arr[i]) return true;
		}
		return false;
	}

	private static void bfs(int cnt, int lastNum, int[] arr){
		if(cnt == m){
			print(arr);
			return;
		}

		for (int i = lastNum; i <= N; i++) {
			arr[cnt] = i;
			bfs(cnt + 1, i + 1, arr);
			arr[cnt] = 0;
		}
	}

    public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] inputs = br.readLine().split(" ");
		N = Integer.parseInt(inputs[0]);
		m = Integer.parseInt(inputs[1]);
		int[] arr = new int[m];
		bfs(0, 1, arr);
	}
}
```

`lastNum` 변수를 추가하여 그보다 큰 경우에만 자릿수에 취급한다.

### 추가 풀이 방법
- 이 방식이 백트래킹의 기본이라는데 와닿지 않음
- curNum보다 큰 값을 다음 자릿수에 넘겨준다. 만약 현재 curNum을 선택하지 않으면 curNum보다 큰 값을 현재 자릿수에 채운다.

```java
import java.io.*;
public class Main {

    static int N,m;

    private static void print(int[] arr) {
		for (int i = 0; i < m; i++) {
			System.out.print(arr[i] + " ");
		}
		System.out.println();
	}

	private static void bfs(int cnt, int curNum, int[] arr){
		if (cnt == m || curNum == N + 1) {
			if (cnt == m)
				print(arr);

			return;
		}

		arr[cnt] = curNum;
		bfs(cnt + 1, curNum + 1, arr);
		arr[cnt] = 0;

		bfs(cnt, curNum + 1, arr);
	}

    public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String[] inputs = br.readLine().split(" ");
		N = Integer.parseInt(inputs[0]);
		m = Integer.parseInt(inputs[1]);
		int[] arr = new int[m];
		bfs(0, 1, arr);
	}
}
```