# 고지식한 패턴 검색(Brute Force)

### 요약

- 문자열을 처음부터 끝까지 차례대로 순회하면서 패턴 내의 문자들을 일일이 비교
- 시간 복잡도 : O(MN)

### 코드

```java
public class BruteForce {
	
	public static int bruteForce(char[] p, char[] t) {
		int i=0;		int j=0;
		int N=p.length;	int M=t.length;
		while(i<N && j<M) {
			if(p[i] != t[j]) { i = i-j; j = -1; }
			i++; j++;
		}
		if (j == M)	return i-M;
		else return i;
	}
	
	public static void main(String[] args) {
		char[] a = {'b', 'c', 'a', 'd', 'a', 'b', 'c', 'e'};
		char[] b = {'a', 'b', 'c'};
		System.out.println(bruteForce(a, b));
	}
}
```

