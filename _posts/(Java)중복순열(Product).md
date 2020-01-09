# 중복순열(Product)

### 코드

```java
import java.util.Arrays;

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

