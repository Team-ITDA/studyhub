호이스팅
==========

- 자바스크립트 Parser가 스크립트를 실행하기 전에 전체를 한 번 훑고
스크립트 내의 변수와 함수를 <u>**최상단에 올려 선언**</u>하는 것을 말한다

- 실제 메모리에는 변화가 없다

- 호이스팅의 대상은 var 변수와 함수 선언문이다. let,const 변수와 함수 표현문은 호이스팅되지 않는다.

```
var value = "Hello World!";
```
위의 코드가 실제 실행되었을 때 이처럼 실행된다

```
var value;
value = "Hello World!";
```
전자가 실제 실행되었을 때 후자처럼 value 변수가 호이스팅되서 **최상단에 선언** 된다

- 따라서 후에 선언된 var 변수를 사용했을 경우 **값이 할당이 안되어** undefined 가 반환되지만 let, const 변수는 **선언조차 안되었기 때문에 에러**가 발생한다

```
console.log(value); // undefined
console.log(letValue); //error!

var value = "Hello World!";
let letVlaue = "Hello Let";
```
위의 코드가 실제 실행되었을 때 이처럼 실행된다

```
var value;
console.log(value); //undefined
console.log(letValue);  //error!

value = "Hello World!";
let letValue = "Hello Let";
```

---------------------

- 함수 선언문의 호이스팅

```
printHello(); // "Hello"

function printHello(){
  console.log("Hello");
}
```
위의 코드에서 함수는 호이스팅되어 최상단에 선언되기 때문에 실제로는 아래 코드와 같다
```
function printHello(){
  console.log("Hello");
}

printHello(); // "Hello"
```

- 하지만 함수 표현문의 경우 호이스팅되지 않는다

```
printHello(); // error

var printHello = function(){
  console.log("Hello");
}
```

위의 코드에서 printHello 함수가 실제로 실행되었을 때는 변수로 선언되기 때문에 함수가 아니라는 에러가 뜬다. 아래 코드는 위 코드가 실제 실행되었을 때이다.

```
var printHello;

printHello(); // this is not function!!

printHello = function(){
  console.log("Hello");
}
```


- 함수 선언문과 함수 표현문은 실제 동작하는 것은 다르고, 함수 표현문은 **디버깅 하기가 힘들기 때문에** 충분히 생각하고 사용하자

-----------

> 원문
[[JavaScript] 호이스팅(Hoisting)
](https://gmlwjd9405.github.io/2019/04/22/javascript-hoisting.html)
