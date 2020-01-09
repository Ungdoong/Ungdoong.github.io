# 셀렉션(Selection)

### 요약

- 저장되어 있는 자료로부터 k번째로 큰 혹은 작은 원소를 찾는 방법
  - 최소값, 최대값 혹은 중간값을 찾는 알고리즘을 의미하기도 함
- 과정
  1. 정렬 알고리즘을 이용하여 자료 정렬
  2. 원하는 순서에 있는 원소를 가져옴

### 코드

```java
public class Selection {
	public static final int[] a= {9,8,7,6,5,4,3,2,1,0,10,11,12,13};
	
	public static int select(int key) {
		for(int i=0; i<key; i++) {
			int minIndex=i;
			int minValue=a[i];
			for(int j=i+1; j<a.length;j++) {
				if(a[j] < minValue) {
					minIndex=j;
					minValue = a[j];
				}
			}
			int temp = a[i];
			a[i] = a[minIndex];
			a[minIndex] = temp;
		}
		return a[key-1];
	}
	
	public static void main(String[] args) {
		System.out.println(select(8));
	}
}
```

