Next Permutation : 현 순열에서 사전 순으로 다음 순열 생성
 
 ex)
 1 2 3
 1 3 2
 2 1 3
 2 3 1
 3 1 2
 3 2 1

 1. 뒤쪽부터 탐색하여 앞이 더 작은 숫자(i-1) 찾기 (꼭대기 = i)
 2. 뒤쪽부터 탐색하여 i-1보다 작은 숫자 찾기(j)
 3. i-1,j 교환
 4. i부터 맨 뒤까지 오름차순 정렬

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
