# 버블 정렬(Bubble Sort)

### 요약

- 인접한 두 개의 원소를 비교하며 자리를 계속 교환하는 방식
- 코딩이 가장 순쉬운 정렬
- 정렬 과정
  1. 첫 번째 원소부터 인접한 원소끼리 계속 자리를 교환하면서 맨 마지막 자리까지 이동
  2. 한 단계가 끝나면 가장 큰 원소가 마지막 자리로 정렬됨
- 시간 복잡도 : O(n^2^)

### 배열을 활용한 버블 정렬

```
BubbleSort(a[], n)
	for i from n-1 to 0 decreasing by 1 {
		for j from 0 to i{
			if(a[j]>a[j+1]) then{
				temp ← a[j];
				a[j] ← a[j+1];
				a[j+1] ← temp;
			}
		}
	}
end BubbleSort
```

