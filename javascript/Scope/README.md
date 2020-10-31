Scope
==========

정의 : 변수가 접근할 수 있는 영역

자바스크립트에서는 **전역 스코프**와 **지역 스코프**가 존재한다


#### 전역 스코프
스크립트의 전체

*웹 브라우저의 자바스크립트 전역 스코프와 Node.js의 전역 스코프는 다르다*

전역 변수를 선언하면 스크립트 어디에서든 사용할 수 있다

```
const value = "Hello World!"

function printValue(){
  console.log(value);
}

printValue();  // "Hello World!"
console.log(value); // "Hello World!"
```  

#### 지역 스코프
스크립트의 특정 범위

자바스크립트에서의 지역 스코프는 **함수 스코프**와 **블록 스코퍼**로 나뉜다

- 함수 스코프

함수 내의 범위

```
function printHello(){
    const contents = "Hello World!";
    console.log(contents); // "Hello World!"
}

printHello(); // "Hello World!";
console.log(contents); //error
```

- 블럭 스코프

중괄호(<code>{}</code>) 내부에 범위를 뜻한다

```
{
      const contents = "Hello World!";
      console.log(contents); // "Hello World!"
}

printHello(); // "Hello World!";
console.log(contents); //error
```

함수 스코프와 블럭 스코프에서 선언된 변수는 바깥 쪽에서 접근이 불가하다.

*화살표 함수를 사용하지 않는 이상 함수 선언할 때 중괄호를 사용하기 때문에 블럭 스코프는 함수 스코프의 부분 집합이다*
