# 조합(Combination)

### 코드

```java
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

