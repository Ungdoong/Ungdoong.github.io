# 190723(Tue)_Java\_기초

### 접근 제한자

- public : 모든 접근을 허용
- protected : 같은 패키지(폴더)에 있는 객체와 상속관계의 객체 허용
- default : 같은 패키지(폴더)에 있는 객체들만 허용
- private : 현재 객체 내에서만 허용

### Static

- 객체를 생성하지 않고도 해당 변수에 접근가능
- 객체 간 같은 값을 유지하는 변수에 사용 시 메모리의 이점
- static method 안에서는 인스턴스 변수 접근이 불가능 static 변수만 가능

### Product 코드

```java
public class Product_Practice {
	public static int n, r, casecount;
	public static int[] a;
	
	public static void product(int count) {
		if(count == r) {
			casecount++;
			System.out.println(Arrays.toString(a));
		}
		else {
			for(int i=1; i<=n; i++) {
				a[count] = i;
				product(count+1);
			}
		}
	}

	public static void main(String[] args) {
		n=6;
		r=3;
		a = new int[r];
		casecount=0;
		product(0);
		System.out.println(casecount);
	}
}
```

### Permutation 코드

```java
public class Permutation_Practice {
	public static int n, r, casecount;
	public static int[] a;
	
	public static void permutation(int flag, int count) {
		if(count==r) {
			casecount++;
			System.out.println(Arrays.toString(a));
			return;
		}
		else {
			for(int i=0; i<n; i++) {
				if((flag&1<<i) == 0) {
					a[count] = i+1;
					permutation(flag|(1<<i), count+1);
				}
			}
		}
	}
	
	public static void main(String[] args) {
		n=6; r=3; casecount=0; 
		int flag=0;
		a=new int[r];
		permutation(flag,0);
		System.out.println(casecount);
	}
}
```

### Homogeneous 코드

```java
public class Homogeneous_Practice {
	public static int n, r, casecount;
	public static int[] a;
	
	public static void homogeneous(int start, int count) {
		if(count == r) {
			casecount++;
			System.out.println(Arrays.toString(a));
			return;
		}
		else {
			for(int i=start; i<=n; i++) {
				a[count]=i;
				homogeneous(i, count+1);
			}
		}
	}
	
	public static void main(String[] args) {
		n=6;
		r=3;
		a=new int[3];
		homogeneous(1,0);
		System.out.println(casecount);
	}
}
```

### Combination 코드

```java
package array2;

import java.util.Arrays;

public class Combination_Practice {
	public static int n, r, caseCount, a[];
	
	public static void permutation(int start, int count) {
		if(count == r) {
			caseCount++;
			System.out.println(Arrays.toString(a));
			return;
		}
		else {
			for(int i=start; i<=n; i++) {
				a[count] = i;
				permutation(i+1, count+1);
			}
		}
	}
	
	public static void main(String[] args) {
		n=6;
		r=3;
		caseCount=0;
		a = new int[r];
		permutation(1,0);
		System.out.println(caseCount);
	}
}
```
