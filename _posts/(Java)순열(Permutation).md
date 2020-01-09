# 순열(Permutation)

### 코드

```java
import java.util.Arrays;

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

