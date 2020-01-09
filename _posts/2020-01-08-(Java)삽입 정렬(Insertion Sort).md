# 삽입 정렬(Insertion Sort)

- 정렬할 자료를 두개의 부분집합 S와  U로 가정
  - 부분집합 S : 정렬된 앞부분의 원소들
  - 부분집합 U : 아직 정렬되지 않은 나머지 원소들
- U의 원소를 하나씩 S로 정렬하여 삽입
- U가 공집합이 되면 완성
- 시간복잡도 - O(n^2^)

```java
import java.util.Arrays;

public class InsertionSort {

	public static void main(String[] args) {
		int[] a= {69,10,30,2,16,8,31,22};
		System.out.println(Arrays.toString(a));
		for(int i=1; i<a.length; i++) {
			int k=i;
			for(int j=i-1; j>=0; j--) {
				if(a[j]>a[k]) {
					int T=a[j]; a[j]=a[k]; a[k]=T;
					k=j;
				}
			}
			System.out.println(Arrays.toString(a));
		}
	}
}
```

