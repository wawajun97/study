# Next Permutation

Next Permutation은 현재 순열 기준으로 사전순 다음 순열을 구하는 알고리즘이다.

## 예시

```text
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
```

## 진행 순서

1. 뒤에서부터 탐색해 앞 원소가 더 작은 위치 `i-1`을 찾는다. (`i`가 꼭대기 위치)
2. 다시 뒤에서부터 탐색해 `input[i-1]`보다 큰 값이 처음 나오는 위치 `j`를 찾는다.
3. `i-1`과 `j`를 교환한다.
4. `i`부터 끝까지를 오름차순이 되도록 뒤집는다.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class NextPermutationInputTest {
	public static int N;
	public static int[] input;
	public static void main(String[] args) throws NumberFormatException, IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

		N = Integer.parseInt(br.readLine());
		input = new int[N]; // 입력된 수가 저장되는 배열

		for(int i =0;i<N;i++) {
			input[i] = Integer.parseInt(br.readLine());
		}

		Arrays.sort(input); //오름차순 정렬

		do {
			System.out.println(Arrays.toString(input));
		} while(np());
	}

	public static boolean np() { // 현재 순열 기반으로 다음단계 순열 생성 : 가능하면 true, 불가능하면 false-> 가장 큰 순열이면
		int i = N-1;

    // i-1 교환 위치 찾기
		while(i > 0 && input[i-1] >= input[i]) --i;

		//가장 큰 순열이면 return false
		if(i == 0) return false;

		//i-1 교환위치값보다 한단계 큰 수 뒤쪽부터 찾기
		int j = N-1;
		while(input[i-1] >= input[j]) --j;

		// i-1 위치값과 j위치값 교환
		swap(i-1,j);

		// 꼭대기부터 맨 뒤까지 가장 작은 수로 만듦(오름차순 정렬) -> 이미 내림차순으로 정렬되어 있기 때문에 0 <-> i, 1 <-> i-1 ... 으로 스왑하면 됨
		int k = N-1;
		while(i<k) {
			swap(i++,k--);
		}

		return true;
	}

	public static void swap(int a, int b) {
		int temp = input[a];
		input[a] = input[b];
		input[b] = temp;
	}
}
```
