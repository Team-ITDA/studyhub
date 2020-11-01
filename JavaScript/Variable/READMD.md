var, let, const의 차이점
==========

크게 유효범위라고 볼 수 있다.

var =>  function scope (함수 단위)

let, const => block scope (중괄호{} 단위)

<br>

var
----------
var 변수 선언은 호이스팅이 일어난다.

>호이스팅이란
>
>   : 함수안에 있는 선언들을 모두 끌어올려서 해당 함수 유효 범위의 최상단에 선언하는 것
> 
> (var 변수선언과 함수선언문만에서만 일어남)
>


```
for(var j=0; j<10; j++) {
  console.log('j', j)
}

console.log('after loop j is ', j) // after loop j is 10


function counter () {
  for(var i=0; i<10; i++) {
    console.log('i', i)
  }
}

counter()

console.log('after loop i is', i) // ReferenceError: i is not defined
```
위 코드에서 변수 i와 j를 비교해보자

호이스팅 개념으로 이해해보면
```
var j

function counter () {
  var i
  for(var i=0; i<10; i++) {
    console.log('i', i)
  }
}

for(var j=0; j<10; j++) {
  console.log('j', j)
}

console.log('after loop j is ', j) // after loop j is 10

counter()

console.log('after loop i is', i) // ReferenceError: i is not defined
```
이 순서대로 진행이된다.

나의 생각으로는 i의 값처럼 출력이되는 것이 맞다.

하지만 j가 10이라는 값이 나올수 있는 이유는 var 호이스팅 때문이다.

나의 생각대로 출력을 만들기 위해서는 function을 사용해야할까

JavaScript에서는 즉시 실행되는 함수 표현식(IIFE, Immediately-Invoked Function Expression)이라는 것이 있다.

IIFE를 이용하여 function-scope인것처럼 만들 수 있다. 

```
// IIFE를 사용하면
// i is not defined가 뜬다.
(function() {
  // var 변수는 여기까지 hoisting이 된다.
  for(var i=0; i<10; i++) {
    console.log('i', i)
  }
})()
console.log('after loop i is', i) // ReferenceError: i is not defined
```

하지만 이와 유사한 코드를 보면,
```
(function() {
  for(i=0; i<10; i++) {
    console.log('i', i)
  }
})()

console.log('after loop i is', i) // after loop i is 10
```

위에 코드가 아무 에러 없이 실행되는 이유는 i가 호이스팅 되어서 전역 변수가 되었기 때문이다.

IIFE를 사용할 때는 function을 사용하지 않는 것과 같이 전역 변수로 호이스팅이 이루어진다.

그래서 아래와 같이 된 것이다.

```
var i

(function() {
  for(i=0; i<10; i++) {
    console.log('i', i)
  }
})()

console.log('after loop i is', i) // after loop i is 10
```

IIFE를 쓰는데 호이스팅이 된다면 쓸 이유가 없다.

그래서 IIFE를 쓰면서 호이스팅을 막기 위해 'use strict'을 사용한다.

>use strict
>   >Strict Mode의 선언방식이다.
>   >
>   >안전한 코딩을 위한 하나의 가이드라인
>   >
>   >   >Strict Mode
>   >   >
>   >   >코드에 더 나은 오류 검사를 적용하는 방법
>   >   >암시적으로 선언한 변수를 사용하거나 읽기 전용 속성에 값을 할당하거나 확장할 수 없는 개체에 속성을 추가할 수 없다.

```
(function() {
  'use strict'
  for(i=0; i<10; i++) {
    console.log('i', i)
  }
})()

console.log('after loop i is', i) // ReferenceError: i is not defined
```

이렇게 사용하면 원래 생각했던 대로 출력이 나온다.

var 변수 선언은 많은 일을 하게끔 만든다.

<br>

let, const
----------

ES6에서 추가된 let, const

var 변수를 이용하면 문제가 많다.

```
// 이미 만들어진 변수이름으로 재선언했는데 아무런 문제가 발생하지 않는다.
var a = 'test'
var a = 'test2'

// 호이스팅으로 인해 ReferenceError에러가 나지 않는다.
c = 'test'
var c
```

하지만 let, const를 이용하면 변수 재선언이 불가능하다.
```
// let
let a = 'test'
let a = 'test2' // Uncaught SyntaxError: Identifier 'a' has already been declared
a = 'test3'     // 재할당 가능

// const
const b = 'test'
const b = 'test2' // Uncaught SyntaxError: Identifier 'a' has already been declared
b = 'test3'    // 재할당 불가능
```

let과 const의 차이점은 immutable(변경 불가능)이다.

let은 변수에 재할당이 가능하지만

const는 변수 재선언, 재할당 모두 불가능하다. 

<br>

그렇다고 호이스팅이 발생하지 않는 것은 아니다.

var이 함수 단위로 호이스팅 되었다면,

let, const는 중괄호 단위로 호이스팅이 발생한다.

```
c = 'test' // ReferenceError: c is not defined
let c
```
위의 코드에서 ReferenceError가 발생하는 이유는 TDZ(Temporal Dead Zone)때문이다.

let는 값을 할당하기 전에 변수가 선언 되어있어야 하는데 그렇지 않기 때문에 에러가 난다.

변수가 undefined로 시작되는 var과는 다르게 let은 실행되기 전까지 초기화 되지 않는다.

따라서 변수가 초기화 되기전에 접근하는 것은 ReferenceError를 뱉어내고 있는 것이다.

> TDZ (Temporal Dead Zone)
>
>   : 위의 예제와 같이 초기화되지 않는 변수가 있는 곳
>
>   : 변수가 초기화되는 순간 TDZ에서 나오게 되며 사용할 수 있게 된다.
>
>   : let, const도 호이스팅이 된다.


const도 확인해 보면,
```
// let은 선언하고 나중에 값을 할당이 가능하지만
let dd
dd = 'test'

// const 선언과 동시에 값을 할당 해야한다.
const aa // Missing initializer in const declaration
```

<br><br><br>
<hr />

###정리
   
| 이름 | 단위 | 변수 재선언 | 재할당 | TDZ의 영향 |
| --- | :---:| :---: | :---: | :---: |
|`var` | function | O | O | X |
|`let` | block | X | O | O |
|`const` | block | X | X | O |



[참조1](https://gist.github.com/LeoHeo/7c2a2a6dbcf80becaaa1e61e90091e5d)







