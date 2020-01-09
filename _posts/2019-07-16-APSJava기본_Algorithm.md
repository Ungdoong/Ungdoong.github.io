# 190716(Tue)\_APSJava기본_Algorithm

## JAVA 문법

- `sc.next()`
  - 공백이나 개행문자가 나오기전까지 읽음
  - 그 뒤의 공백이나 개행문자를 없애지 않음
- `sc.nextInt()`
  - 다음 Int형 숫자를 읽음
  - 그 뒤의 공백이나 개행문자를 없애지 않음
- `sc.nextLine()`
  - 개행문자를 만나기전까지 다음 줄을 읽음
  - 그 뒤의 개행문자를 없앰
  - `nextLine()`의 앞에 `next()`또는 `nextInt()`등을 사용할 때는 주의해야함

### 완전 검색(Exaustive Search)

- 생각할 수 있는 모든 경우의 수를 나열해보고 확인
- Brute-force / generate-and-test 기법이라고도 함
- 경우의 수가 작을 때 유용

### 순열(Permutation)

- 서로 다른 것들 중 몇개를 뽑아서 한 줄로 나열

- 서로 다른 n개 중 r개를 택하는 순열

  - nPr = n * (n-1) * (n-2) * ... (n-r+1)

    ​		= n! / (n-r)!

- C++ Standard Template Library(STL) 함수

  - ```java
    template<class BidirectionalIterator> inline
    	bool next_permutation(
    		BidirectionalIterator First,
    		BidirectionalIterator Last
    	)
    ```

### 탐욕(Greedy) 알고리즘

- 최적해를 구하는 데 사용되는 근시안적인 방법
- 여러 경우 중 하나를 결정할 때 그 순간에 최적이라고 생각되는 것을 선택해 나가는 방식
- 지역적 최적으로 얻은 최종적인 해답은 그것이 최적이라는 보장이 없음

## 정렬(Sort)

### 목차

1. 버블 정렬(Bubble Sort)
2. 카운팅 정렬(Counting Sort)
3. 선택 정렬(Selection Sort)
4. 퀵 정렬(Quick Sort)
5. 삽입 정렬(Insertion Sort)
6. 병합 정렬(Merge Sort)

### 1. 버블 정렬(Bubble Sort)

- 인접한 두 개의 원소를 비교하며 자리를 계속 교환

- 시간 복잡도 : O(n^2^)

- 배열을 활용한 버블 정렬

  ```
  BubbleSort(a[], n)
  	for i from n-1 to 0 decreasing by 1{
  		for j from 0 to i {
  			if (a[j] > a[j+1]) then{
  				swap(a[j], a[j+1])
  			}
  		}
  	}
  end BubbleSort
  ```

- ```java
  import java.util.Arrays;
  import java.util.Scanner;
  
  public class Algorithm_Bubble {
  
  	public static void main(String[] args) {
  		int[] a = {10,4,6,7,2,9,3,1,8,5};
  		System.out.println(Arrays.toString(a));
  		for(int i = a.length-1; i >= 0; i--) {
  			for(int j = 0; j < i; j++) {
  				if(a[j] > a[j+1]) {
  					int T = a[j];
  					a[j] = a[j+1];
  					a[j+1] = T;
  				}
  			}
  		}
  		System.out.println(Arrays.toString(a));
  	}
  }
  ```



### 2.  카운팅 정렬(Counting Sort)

- 집합에 각 항목이 몇 개씩 있는지 세는 작업을 하여, 선형 시간에 정렬

- 정수나 정수로 표현할 수 있는 자료에 대해서는 적용 가능

- 시간 복잡도 : O(n + k)

  - n : 리스트 길이, k : 정수의 최대값

- ```java
  package array1;
  
  import java.util.Arrays;
  import java.util.Random;
  
  public class Algorithm_Counting {
  	public static final int SIZE = 20;
  	
  	public static void main(String[] args) {
  		int[] a = new int[SIZE];
  		int i;
  		//a 배열 생성
  		for(i = 0; i < a.length; i++) {
  			Random r = new Random();
  			a[i] = r.nextInt(SIZE)+1;
  		}
  		
  		int M = Arrays.stream(a).max().getAsInt();
  		int[] counting = new int[M+1];
  		int[] new_a = new int[a.length];
  		
  		//counting 배열 초기화
  		for(i = 0; i < counting.length; i++) counting[i] = 0;
  
  		//counting 입력
  		for(i = 0; i < a.length; i++) counting[a[i]]++;
  		
  		//counting 누적하여 정제
  		for(i = 0; i < counting.length-1; i++)
  			counting[i+1] += counting[i];
  
  		//counting을 감소해가며 해당위치에 숫자삽입
  		for(i = a.length-1; i >= 0; i--) {
  			new_a[counting[a[i]]-1] = a[i];
  			counting[a[i]]--;
  		}
  		
  		System.out.println(Arrays.toString(a));
  		System.out.println(Arrays.toString(new_a));
  	}
  }
  ```

  

