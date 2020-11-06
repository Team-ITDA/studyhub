
## 자주 사용하는 String(문자열) 함수

문자열은 불변하는 객체이다.

그렇기 때문에 문자열의 내용이 변경되는 메소드를 사용하더라도, 문자열 자신이 변경되는게 아니라 변경된 새로운 문자열이 반환된다는 사실을 잊으면 안된다.



## int length()

```java
s.length();
```

- 문자열에 있는 공백을 포함한 모든 문자 수 반환

---

### int indexOf(String str)

```java
s.indexOf("찾을문자");
```

- 문자열이 나타나기 시작한 처음 인덱스를 반환한다.
찾지 못하면 -1 반환

### indexOf(String str, int fromIndex)

```java
s.indexOf(찾을문자, index위치이후부터);
```

- fromIndex 이후로 찾을문자열이 처음 나타나는 인덱스를 반환
찾지 못하면 -1 반환

<br/>
참고)
### lastindexOf(String str)

```java
s.lastindexOf("A");
```

- 지정한 문자가 문자열에 마지막 몇 번째에 있는지 ,index 반환

---

### char charAt(int index)

```java
s.charAt(인덱스);
```

- index 번째 해당하는 문자를 반환하는 함수
- 문자열에서 index는 0부터 시작한다.

---

### char[] toCharArray();

```java
char[] carr = s.toCharArray();
```

- 문자열을 문자단위로 분해하여, 새롭게 char형 배열로 반환한다.

<br/>
참고)

new String(배열) - char형 배열을 문자열로 바꾼다.

```java
String s = "abcdefghij";

char[] carr = s.toCharArray();

String str = new String(carr);

System.out.println(s);                     //abcdefghij
System.out.println(Arrays.toString(carr)); //[a, b, c, d, e, f, g, h, i, j]
System.out.println(str);                   //abcdefghij
```

---

### int compareTo(비교할문자열)

```java
s1.comareTo(s2);
```

- s1이 문자열 s2 에 비해 사전순으로
앞에 올 경우 음수
동일할경우 0
뒤에올경우 양수 리턴

<br/>
참고)

기준문자열.comareToIgnoreCase(비교할문자열)

- 두 문자열을 비교할때 대소문자를 구분하지 않는다.

---

### String replace(char oldchar, char newChar)

```java
s1.replace('찾을문자', '바꿀문자');

//"ababab" 문자열이 있을때 s.repalce("a", "b") 하게 되면
//"bbbbbb" 로 반환 된다.
```

- 문자열안에 "찾을 문자"가 있으면 새로 지정한 "바꿀 문자"로 바꾼 새로운 문자열을 반환해주는 함수
- 기존 문자열은 변경되지 않는다

### String replaceAll(String regex, String replacement)

```java
s.replaceAll("찾을문자(정규식)", "바꿀문자");

ex)
String s = "ab cd"
s.replaceAll("\\p{Space}", "*"); // ab*cd
```

- 정규표현식으로 찾은 문자를 , 지정한 문자(replacement)로 바꾼 새로운 문자열을 반환
- 기존 문자열은 변경되지 않는다.

### String replaceFirst(String regex, String replacement)

```java
s.replaceFirst("지정한문자열", "바꿀문자열");

ex)
String a = "AmenA";
System.out.println(a.replace("A","Super")); // SupermenSuper
System.out.println(a.replaceFirst("A","Super")); // SupermenA

String b = "menA";
System.out.println(b.replaceFirst("A","Super")); //menSuper
```

- 문자열에 "지정한 문자열"(regex)이 있으면 첫 번째만 "바꿀 문자열"(replacement)로 바꿔, 새로운 전체 문자열을 반환한다.

---

### String[] split(String regex)

```java
s.split("지정문자(정규식)");

ex)
String s = "abcUdefUghi";
System.out.println(Arrays.toString(s.split(""))); //[a, b, c, U, d, e, f, U, g, h, i]
System.out.println(Arrays.toString(s.split("U")));//[abc, def, ghi]
```

- 지정한 문자(regex)로 대상 문자열을 나누어 배열을 반환한다.
- 공백이 있는 경우, 공백도 문자열 배열에 들어 간다. ("abc  *def  *ghi)
- "문자열"을 문자열 배열로 만들기 위해서는 지정 문자(regex)를 ""로 하면된다.

---

### String substring(int beginIndex)

```java
String s = "abcde";

s.substring(2); //cde
```

- 문자열에 지정한 범위부터 끝까지에 속하는 문자열 반환

### String substring(int beginIndex, int endIndex)

```java
String s = "abcdefg";

s.substring(2, 6); //cdef
```

- 문자열의 지정한 범위에 속하는 문자열을 반환한다.
- 시작범위의 값은 포함하고, 끝나는 범위의 값은 포함하지 않는다.
(문자열의 인덱스는 0부터 센다)



---

### String toLowerCase()   && String toUpperCase();

```java
s.toLowerCase(); //문자열안에 대문자를 소문자로 바꾼 문자열 반환

s.toUpperCase(); //문자열안에 소문자를 대분자로 바꾼 문자열 반환
```

- 문자열 안에 대문자를 소문자로 /  소문자를 대문자로 바꿔준 새로운 문자열을 반환
- 기존 문자열은 변경되지 않는다.

---

### String concat(String s)

```java
s1.concat(s2);

//s1에 s2를 추가한 문자열반환
```

- 두 문자열을 합친 새로운 문자열을 반환하는 함수

---

### String trim()

```java
s.trim();
```

- 문자열에 존재하는 공백을 제거할 때 사용한다.
- 문자열의 앞, 뒤 쪽에 있는 공백만 제거할 수 있으며, 가운데 존재하는 공백은 제거할수 없다.

참고)

가운에 존재하는 공백을 제거하기위해서는 replace() 함수를 사용해야 한다.

---

### boolean contains(CharSquense s)

```java
s1.contains(s2);
```

- 두개의 문자열을 비교해서 비교대상 문자열(s)을 포함하고 있으면 true, 그렇지않으면 false반환

---

### boolean equals(Object anObject)

```java
s1.equals(s2);
```

- s1 과 s2가 동일한 문자이면 true반환, 그렇지 않으면 false
- 문자열은 == 로 비교할 수 없다.

<br/>
참고)

### boolean euqalsIgnoreCase(Object anObject)

```java
s1.equalsIgnoreCase(s2);
```

- 대소문자 구분하지 않고 문자열 비교

---

### boolean endsWith(String suffix)

```java
s1.endWith(s2);
//s1 : hello world
//s2 : world

//s2는 s1의 접미사 true반환
```

- s2가 s1의 접미사이면 true을 반환한다. 그렇지 않으면 false

### boolean startsWith(String preifx)

```java
s1.startWith(s2);
//s1 : hello world
//s2 : hello

//s2는 s1의 접두사 true반환
```

- s2가 s1의 접두사이면 true을 반환한다. 그렇지 않으면 false

<br/>
참고)

- 접미사(suffix)는 파생어(derivative)를 만드는 접사로, 단어의 뒤에 붙어 새로운 단어가 되게 하는 말이다.
- 접두사(prefix)  어떤 단어의 앞에 붙어 뜻을 첨가하여 하나의 다른 단어를 이루는 말을 이른다

---

### boolean matches(String regex)

```java
s.matches("정규식")

ex)
int i = 123456;
String str1 = String.format("%,d", i); // 123,456
String str2 = "123456";

boolean matcher = str1.matches(str2); // false
```

- 지정한 정규표현식과 일치할때, true를 반환하고, 그렇지 않으면 false를 반환한다.

---

### String toString()

```java
s.toString();
```

- 문자열을 반환한다.

---

### String valueOf(...)

```java
String.valueOf(..)

ex)
int i = 123456789;
long l = 1l;
char c = '1';
System.out.println(String.valueOf(i)); // 123456789
System.out.println(String.valueOf(l)); // 1
System.out.println(String.valueOf(c)); // 1
```

- 지정한 개체의 원시값을 반환한다.(문자열로 변환)

---

## toString() 과 valueOf() 차이

toString()과의 차이점이 존재한다.
먼저, toString(), valueOf() 메소드는 모두 Object의 값을 String으로 변환하지만, 변경하고자 하는 Object가 null인 경우 다르다.

- toString() : Null Pointer Exception(NPE) 발생.

- valueOf() : "null"이라는 문자열로 처리한다.
    - valueOf() 함수의 원형은 다음과 같다.
    obj가 null인 경우, “null” 문자열을 반환한다.

```java
public static String valueOf(Object obj) {
    return (obj == null) ? "null" : obj.toString();
}
```

- 두 메소드의 차이는 null 값에 따른 NPE의 발생 유무이다.
null로 인해 발생된 에러는 시간이 지나고, 타인의 소스인 경우 디버깅하기 어렵다.
또한, 어떤 의미를 내포하고 있는지 판단하기 어렵다.
때문에 NPE를 방지하기 위해 toString 보다는 valueOf를 사용하는 것을 추천한다.

---

### String format(String format, Object... args)

```java
String.format("%[옵션]", 인자)

ex)
int i = 123456789;
System.out.println(String.format("%,d",i)); // 123,456,789
```

- 서식 문자열을 이용하여, 서식화된 문자열을 반환한다.
