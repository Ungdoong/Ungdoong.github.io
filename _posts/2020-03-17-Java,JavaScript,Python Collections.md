# Java & Python & JavaScript Collections

## Java

_________

### before

```java
import java.util.*;

public class HelloWorld{

	public static void main(String []args){
         
		Calendar calendar = Calendar.getInstance();
		calendar.setTime(new Date());
        
		int year = calendar.get(Calendar.YEAR);
		int month = calendar.get(Calendar.MONTH);
        
        
		int days = 0;
        
        switch(month){
            case 0:
                days = 31;
            case 2:
                days = 31;
            case 4:
                days = 31;
            case 6:
                days = 31;
            case 7:
                days = 31;
            case 9:
                days = 31;
			case 11:
				days = 31;
            case 3:
                days = 30;
            case 5:
                days = 30;
            case 8:
                days = 30;
            case 10:
                days = 30;
            case 1:
                if((year % 4 == 0) && (year % 100 != 0) || (year % 400) == 0)
                    days = 29;
                else
                    days = 28;
        }
            
        System.out.println(days + " days");
        
     }
}
```

- 결과

  `29 days`

  - switch문의 케이스에 `break;`가 없음
  - days를 배열로 선언하여 각 달에 해당하는 날짜 수를 직접접근 하게 함
  - 윤달은 상위에서 체크

### after

```java
import java.util.*;

public class question{

	public static void main(String []args){
         
		Calendar calendar = Calendar.getInstance();
		calendar.setTime(new Date());
        
		int year = calendar.get(Calendar.YEAR);
        int month = calendar.get(Calendar.MONTH);
        
        int[] days = {31, 28, 31, 30, 31, 30 , 31, 31, 30, 31, 30, 31};
        
        if((year % 4 == 0) && (year % 100 != 0) || (year % 400) == 0){
            days[1]++;
        }

        System.out.println(days[month] + " days");
        
        
     }
}
```

- 결과

  `31 days`

## JavaScript

_________

```javascript
const date = new Date()
const year = date.getFullYear()
const month = date.getMonth()

var days = null

switch(month){

	case 0:
	case 2:
	case 4:
	case 6:
	case 7:
	case 9:
	case 11:
		days = 31;
		break;
	
	case 3:
	case 5:
	case 8:
	case 10:
		days = 30;
		break;
	case 1:
		if((year % 4 == 0) && (year % 100 != 0) || (year % 400) == 0)
			days = 29;
		else
			days = 28;
		break;
}

console.log(days + ' days for ' + year + '-' + (month + 1))

```

- 결과

  `31 days for 2020-3`

- 수정 내용

  - days를 배열로 선언하여 직접접근
  - 윤달을 상위에서 체크

### after

```javascript
const date = new Date();
const year = date.getFullYear();
const month = date.getMonth();

let days = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]

if ((year % 4 == 0 && year % 100 != 0) || year % 400 == 0) days[1]++;

console.log(days[month] + " days for " + year + "-" + (month + 1));
```

- 결과

  `31 days for 2020-3`



## Python

```python
import datetime


def getDays(year, month):

    if month == 1:
        return 31
    elif month == 3:
        return 31
    elif month == 5:
        return 31
    elif month == 7:
        return 31
    elif month == 8:
        return 31
    elif month == 10:
        return 31
    elif month == 12:
        return 31
    elif month == 4:
        return 30
    elif month == 6:
        return 30
    elif month == 9:
        return 30
    elif month == 11:
        return 30
    elif month == 2:
        if((year % 4) == 0) and ((year % 100) != 0) or ((year % 400 ) == 0):
            return 29
        else:
            return 28

dt = datetime.datetime.today()

print(getDays(dt.year, dt.month))
```

- 결과

  `31`

- 수정 내용

  - days를 콜렉션으로 수정하여 직접접근

### after

```python
import datetime


def getDays(year, month):

    days = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]

    if((year % 4) == 0) and ((year % 100) != 0) or ((year % 400 ) == 0):
        days[1] += 1

    return days[month]


dt = datetime.datetime.today()

print(getDays(dt.year, dt.month))
```

- 결과

  `31`