---
title: Java Syntax 5) 참조 타입
author: gabin
date: 2024-04-12 T01:00:00
categories: [Backend, Java, Syntax]
tags: [Syntax, 참조타입, nullPointerExcetption, 이것이자바다]
toc: true
---
> 앞서 배운 정수타입, 실수 타입, 논리타입은 기본 타입에 속한다.
> 자바에서는 이렇게 데이터 타입에 두가지로 나뉘는데 이번에는 참조 타입을 알아보자.

<br>
<br>

# 참조타입
: 객체의 번지를 참조하는 타입 -> 배열, 열거, 클래스, 인터페이스 타입이 존재함

- ❗️객체는 데이터(필드)와 메소드로 구성되어 있다
- ❗️기본 타입의 변수는 값 자체를 저장하지만, 참조 타입의 변수는 메모리 번지에 저장한다


## == 과 != 연산
힙의 값을 참조하기 때문에 스택에서 번지가 달라도 같은 힙의 값을 참조한다면 같은 결과를 얻는다

-> `if` 문과 많이 사용한다

```java
...
if(arr5 == arr6) {
	... // if의 조건문으로 들어간다
}

```

```java
public static void main(String[] args) {
	aar1 = new int[] {1,2,3};
	aar2 = new int[] {1,2,3};
	aar3 = arr2;
	
	System.out.println(arr1 == arr2); //false
	System.out.println(arr2 == arr3); //true
}

```

aar1과 aa2는 같은 값을 저장하는 것처럼 보이지만 힙 다르게 각각 1,2,3 / 1,2,3이 저장된 것이다 즉, 참조하는 곳이 다르다 그러므로 arr1 == arr2 는 거짓이고 aar2 == aar3은 참이다.

<br>


## 메모리 사용 영역
모든 변수들은 스택에 생성되고 기본타입은 **<mark>스택</mark>** 에 직접 값을 저장하지만 참조 타입은 스택에 번지값이 있고 실제 값은 <mark>힙 메모리</mark>에 저장되어있다.

1. 메소드 영역 : 바이트 코드 파일을 읽는 내용이 저장되는 영역(.class파일 을 읽는)
   - 클래스별로 상수, 정적필드, 메소드 코드, 생성자 코드 저장

2. 힙 영역 : 객체가 생성되는 영역
   - 객체의 번지는 메소드 영역과 스택 영역의 상수와 변수에서 참조 가능 => 클래스도 참조 타입이지만 클래스별로 메소드 영역에 저장되기 때문이다.

3. 스택 영역 : 메소드를 호출할 때마다 생성되는 프레임이 저장되는 영역
   - 메소드 호출 후에는 프레임이 없어지고 프레임 내부에는 로컬 변수스택이 존재

<br>
<br>
<br>


---

## 참조타입 변수의 예외
참조 타입 변수는 참조할 수도 있고 안 할 수도 있다 그래서 `null`값이 존재 가능하고 초기값으로 사용할 수 있다.

- **참조 타입 변수가 null 값을 가지는지 확인**  매우 쉬움!
```java
 String aar1 = "자바";
 String aar2 = null;
 
  aar 1 == null //위에서 정의한 대로 aar1은 자바라는 데이터를 저장했기 대문에 false
  aar 1 != null //true

  aar 2 == null //위에서 정의한대로 null값과 같다
  aar 2 != null //false

```

<br>
<br>

### nullPointerException
알다시피, 자바 프로그램에서 실행 중 오류를 예외라고 한다. 참조 변수를 사용하면서 발생하는 예외로는 `nullPointerException`이다
어떨 때 오류가 날까?
참조하려는 값(데이터)가 있어야 하는데 , 값도 없으면서 참조 하려면 오류가 난다. null이니까

```java
int[] aar11 = null;
aar11[0] = 10; //에외발생

```

예제에서 aar11은 배열을 저장하고 null이라고 명시했다. 하지만 바로 밑에 aar11의 0번째 값이 10이라고 하면 저장할 수 없어서 윗줄과 대치된다 그러므로 nullPointerException이 발생한다

<br>
<br>

### 다양한 상황

- **쓰레기 객체??** :  값을 지정한 후 다시 null로 지정하면 값은 있지만 참조할 수 없어서(위치정보를 몰라서) 사용할 수 없게 되면 자바가 쓰레기 취급한다.~~ㅋㅋㅋ 불쌍~~ 쓰레기 수집기(청소기)로 자동제거함!
```java
String work1 = "출장"; //위에서부터 실행되므로 값 저장 후
work1 = null; //null로 지정
```


- **null이 아닌 또 다른 값으로 선언한다면?** : 이것도 마찬가지로 다른 객체의 번지가 대입되어 이전 번지가 잃어버려서 쓰레기 값이 된다

<br>
<br>
<br>

---
## String(문자열) 타입 참조 변수
> 자바에서 문자열은 String 객체(데이터)로 생성된다. 중요한 점은 String A = "gabin"일 때, A = gabin이 아닌 gabin이라는 문자열이 변수 A에 대입되는 것이다.

<br>

### > 문자열 비교 : equals()
자바는 문자열이 동일하면 같은 String 객체를 공유한다

- **같은 객체 공유**
	`String name1 = "gabin;``String name2 = "gabin;` 이라면 name 1과 2의 스택 번지값을 다를지라도 힙에는 같은 String 객체인 gabin을 참조한다


- **new 연산자를 이용**
	새로운 객체를 만드는 연산자(객체 생성 연산자)인 new 연산자를 이용할 수 있다.
	`String name3 = new String("gabin");`   `String name4 = new String("gabin");` 이렇게 되면 String 객체가 각각 새롭게 만들어지기 때문에 같은 값을 참조하지 않는다. 중복으로 데이터(객체)가 생성된다는 것이다

<br>

- ❗️ **new 연산자를 사용하는 것과 그냥 대입 선언 방식은 완전히 다른 연산 결과를 가지게 된다**
<br>
- **내부의 문자열만 비교할 경우, equals()메소드 이용**
	기본 문자열. equals(비교할 문자열)
	```java
	boolean result = name3.equals(name4); //같은지
	boolean result = !name3.equals(name4); //같지 않은지
	```

- **비어있는 문자열일 경우 ""**
	빈 문자열도 객체로 생성되기 때문에 동일하게 equals()메소드를 이용해서 비교를 한다
	```java
	String name22 = "";
	if(name22.equals("")) { //name 22는 빈 객체이므로 true로 다음 if문이 실행된다.
		System~...
	}
	```

<br>

### > 문자 추출 : charAt()
`charAt()`메소드를 이용하면 되고 매개변수 값으로 인덱스의 문자를 리턴한다

```java
String working = "쿠팡 알바"; //쿠팡_알바 총 4번쨰 인덱스까지 나온다
char charValue = working.charAt(3);// working의 3번째 인덱스 추출 -> "알"
```



### > 문자열 길이 : length()

```java
String working = "자바 공부하기";
int length = working.length(); // 공백 포함  길이가 7
```



### > 문자열 대체 : replace()
```java
String working = "이불 정리하기"; 
String newWorking = working.replace("정리하기","이불 빨래하기");
```




### > 문자열 자르기 : substring()

|                  메소드                   |                   특징                    |
|:-----------------------------------------:|:-----------------------------------------:|
|         substring(int 시작인덱스)         |      시작 인덱스에서 끝까지 잘라내기      |
| substring(int 시작인덱스, int 종료인덱스) | 시작인덱스에서 종료인덱스 앞까지 잘라내기 |

```java
String working = "이불 정리하기"; 
String newWorking = working.replace("정리하기","이불 빨래하기");
```


### > 문자열  찾기 : indexOf()
<br>
<br>
<br>

---
## Array(배열) 타입 참조 변수



---
## 다차원 Array(배열) 타입 참조 변수


---
## class, interface 참조타입 배열



---
## 배열 복사(반복) for 문



---
## main() 메소드의 String [ ] 매개변수



---
## Enum(열거) 타입
