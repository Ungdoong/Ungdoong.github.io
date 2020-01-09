# 중복조합(Homogeneous)

### 코드

```java
import java.util.Arrays;

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

