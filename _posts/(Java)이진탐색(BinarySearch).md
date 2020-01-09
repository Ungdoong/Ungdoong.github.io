# 이진탐색(BinarySearch)

### 코드

```java
import java.util.Arrays;

public class BinarySearch_Practice {
	public static final int[] a= {2,65,3,2,5,4,124,6,123,3,28,567,8,4};
	
	public static boolean binarySearch(int low, int high, int key) {
		if(low>high)	return false;
		int middle=(low+high)/2;
		if(a[middle] == key)	return true;
		else if(a[middle]<key)
			return binarySearch(middle + 1, high, key);
		else	return binarySearch(low, middle-1, key);
	}
	
	public static void main(String[] args) {
		Arrays.sort(a);
		System.out.println(binarySearch(0,a.length-1,123));
	}
}
```

