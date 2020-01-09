# 선택 정렬(Selection Sort)

### 요약

- 가장 작은 값의 원소부터 차례대로 선택하여 위치를 교환
- 교환의 횟수가 버블, 삽입정렬보다 작음
- 과정
  1. 최소값을 찾음
  2. 그 값을 리스트의 맨 앞에 위치한 값과 교환
  3. 맨 처음 위치를 제외한 나머지 리스트를 대상으로 위의 과정 반복
- 시간 복잡도 : O(n^2^)

### 코드

```java
import java.util.Arrays;

public class SelectionSort {
	public static final int[] a= {2,8,4,7,4,34,7,4,6,4,6,7,3,8,45};
	
	public static void selectionSort(int[] arr) {
		for(int i=0; i< arr.length; i++) {
			int min=arr[i];
			for(int j=i+1; j<arr.length; j++) {
				if(arr[j] < min) {
					min = arr[j];
					arr[j] = arr[i];
					arr[i] = min;
				}
			}
		}
	}
	
	public static void main(String[] args) {
		System.out.println(Arrays.toString(a));
		selectionSort(a);
		System.out.println(Arrays.toString(a));
	}
}
```

