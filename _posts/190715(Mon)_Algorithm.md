# 190715(Mon)_Algorithm

## Eclipse 환경 설정

### Encoding 방식 변경

- Window > Preference
- 'encod' 검색 후 설정을 모두 UTF-8로 변경



### 알고리즘 사이트

- https://swexpertacademy.com/

## 실습

### 1. 스탬프찍기_D1_2046

```java
package array1;
import java.util.Scanner;
import java.io.FileInputStream;

public class Solution_D1_2046_스탬프찍기_서울8반_정택진{
	public static void main(String args[]) throws Exception{
		System.setIn(new FileInputStream("res/input_d1_2046.txt"));
		Scanner sc = new Scanner(System.in);
		int T=sc.nextInt();

		for(int tc = 1; tc <= T; tc++){
			System.out.print("#");
		}
	}
}
```

### 2.원재의메모리복구하기_D3_1289

```java
package array1;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Solution_D3_1289_원재의메모리복구하기_서울8반_정택진{
	public static void main(String args[]) throws Exception{
		//System.setIn(new FileInputStream("res/input_d1_2046.txt"));
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		int[] numbers = new int[50];
		String[] str_numbers = new String[50];
		
		String line = br.readLine();
		int num_test = Integer.parseInt(line);
		int change = 0;
		
		for (int n = 0; n < num_test; n++) {
			change = 0;
			try {
				//숫자를 입력받아 인트형 배열에 저장
				line = br.readLine();
				str_numbers = line.split("");
				for(int i = 0; i < line.length(); i++) {
					numbers[i] = Integer.parseInt(str_numbers[i]);
				}

				for(int j = 0; j < str_numbers.length - 1; j++) {
					if(j == 0 && numbers[j] == 1)
						change++;
					
					if(numbers[j] != numbers[j+1]) {
							change++;
					}
				}
				bw.write("#" + (n+1) + " " + change + "\n");
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
		
		bw.flush();
		bw.close();
	}
}
```

### 3. 최장증가부분수열

```java
package array1;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.Arrays;

public class Solution_D3_3307_최장증가부분수열_서울8반_정택진2{
	public static void main(String args[]) throws Exception{
		System.setIn(new FileInputStream("res/sample_input.txt"));
		InputStreamReader isr = new InputStreamReader(System.in);
		BufferedReader br = new BufferedReader(isr);
		
		OutputStreamWriter osw = new OutputStreamWriter(System.out);
		BufferedWriter bw = new BufferedWriter(osw);
		
		int[] numbers = new int[1000];
		String[] str_numbers = new String[1000];
		
		String line = br.readLine();
		int num_test = Integer.parseInt(line);
		int length_numbers, pre_number, count, max_count;
		
		for (int n = 0; n < num_test; n++) {
			max_count = 0;
			try {
				//숫자를 입력받아 인트형 배열에 저장
				length_numbers = Integer.parseInt(br.readLine());
				line = br.readLine();
				str_numbers = line.split(" ");
				for(int i = 0; i < length_numbers; i++) {
					numbers[i] = Integer.parseInt(str_numbers[i]);
				}
				
				for(int j = 0; j < length_numbers - 1; j++) {
					count = 1;
					
					pre_number = numbers[j];
					for (int k = j + 1; k < length_numbers; k++) {
						if(pre_number < numbers[k]) {
							pre_number = numbers[k];
							count++;
						}
					}
					if(count > max_count)
						max_count = count;
				}
			} catch (IOException e) {
				e.printStackTrace();
			}
			
			bw.write("#" + (n+1) + " " + max_count + "\n");
		}
		
		bw.flush();
		bw.close();
	}
}
```

