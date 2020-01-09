# 부분집합(Subset)

### 코드

```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStreamReader;

public class SubsetSum {
	public static int[] numbers;
	public static boolean[] flags;
	public static int N;
	
	public static boolean solve(int count) {
		if(count == N) {
			int sum=0;
			for(int i=0; i<N; i++)
				if(flags[i] == true)	sum += numbers[i];
			if(sum == 0)	return true;
			else			return false;
		}
		else {
			flags[count] = true;
			if(solve(count+1))
				return true;
			flags[count] = false;
			if(solve(count+1))
				return true;
			return false;
		}
	}
	
	public static void main(String[] args) throws Exception{
		System.setIn(new FileInputStream("res/input_subsetsum.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		String[] str_numbers = br.readLine().split(" ");
		N = str_numbers.length;
		numbers = new int[N];
		for(int i=0; i<N; i++)
			numbers[i] = Integer.parseInt(str_numbers[i]);
		
		flags = new boolean[N];
		System.out.println(solve(0));
	}
}
```

