Arrow Function  (=>) 
==========

화살표 함수 표현(arrow function expression)은 
- function 표현에 비해 구문이 짧고 자신의 this, arguments, super 또는 new.traget을 바인딩하지 않는다.

- 메소드 함수가 아닌 곳에 가장 적합하다.

- 생성자로서 사용할 수 없다.

<br>

## 1. 화살표 함수의 선언
화살표 함수(Arrow function)은 function 키워드 대신 화살표 (=>)를 사용하여 보다 간략한 방법으로 함수를 선언할 수 있다.

하지만 모든 곳에 화살표 함수를 사용할 수 있는 것은 아니다.

기본 문법을 알아보자.

```
// 매개변수 지정 방법
    () => { ... }   // 매개변수가 없을 경우
     x => { ... }   // 매개변수가 한개인 경우, 소괄호를 생략할 수 있다.
(x, y) => { ... }   // 매개변수가 여러 개 인 경우, 소괄호를 생략할 수 없다.


// 함수 몸체 지정 방법
x => { return x * x }    // single line block
x => x * x               // 함수 몸체가 한 줄의 구문이라면 중괄호를 생략할 수 있으며 암묵적인 return이 된다.
                         // 위 두 표현은 동일하다.

() => { return { a: 1 }; }
() => ({ a: 1 })         // 위 표현과 동일하다. 객체 반환시 소괄호를 사용한다.

() => {
    const x = 10;
    return x * x;
};
```

<br>

## 2. 화살표 함수의 호출
화살표 함수는 익명 함수로만 사용할 수 있다.

따라서 화살표 함수를 호출하기 위해서는 함수 표현식을 사용한다.

```
// es5
var pow = function (x) {
    return x * x;
}

console.log(pow(10));   // 100
```

```
// es6
const pow = x => x * x;
console.log(pow(10));   // 100
```

또는 콜백 함수로 사용할 수 있다.

이 경우에는 일반적인 함수 표현식보다 표현이 간결하다.

```
// es5
var arr = [1, 2, 3];
var pow = arr.map( function (x) {
    return x * x;
}

console.log(pow);   // [1, 4, 9]
```

```
// es6
const arr = [1, 2, 3];
const pow = arr.map(x => x * x);

console.log(pow);   // [1, 4, 9]
```

<br>

## 3. this
function 키워드로 생성한 일반 함수와 화살표 함수의 가장 큰 차이점은 <strong>this</strong>이다.

<br>

### 3.1 일반 함수의 this
JavaScript의 경우 함수 호출 방식에 의해 this에 바인당할 어떤 객체가 동적으로 결정된다.

다시말해, 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, <strong>함수를 호출할 때 함수가 어떻게 호출되었는지</strong>에 따라 this에 바인딩할 객체가 동적으로 결정된다.

콜백 함수 내부의 this는 전역 객체 window를 가리킨다.

```
function Prefixer(prefix) {
    this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
    // (A)
    return arr.map(function (x) {
        return this.prefix + ' ' + x;   // (B)
    });
};

var pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee, 'Kim']));
```

> (A)지점에서의 this는 생성자 함수 Prefixer가 생성한 객체, 즉 생성자 함수(pre)의 인스턴스다. 
>
> (B)지점에서 사용한 this는 생성자 함수 Prefixer가 생성한 객체일 것으로 생각하겠지만, 이곳에서의 this는 전역 객체 window를 가리킨다.
>
> 이는 생성자 함수와 객체 메소드를 제외한 모든 함수(내부 함수, 콜백 함수 포함) 내부의 this는 전역 객체를 가리키기 때문이다.


콜백 함수 내부의 this가 메소드를 호출한 객체를(생성자 함수의 인스턴스)를 가리키게 하려면 3가지 방법이 있다.

```
// Solution 1. this = this
function Prefixer(prefix) {
    this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
    var that = this;    // this : Prefixer 생성자 함수의 인스턴스
    return arr.map(function (x) {
        return this.prefix + ' ' + x;
    });
};

var pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee, 'Kim']));
```

```
// Solution 2. map(func, this)
function Prefixer(prefix) {
    this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
    return arr.map(function (x) {
        return this.prefix + ' ' + x;
    }, this);   // this : Prefixer 생성자 함수의 인스턴스
};

var pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee, 'Kim']));
```

```
// Solution 3. bind(this)
function Prefixer(prefix) {
  this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
  return arr.map(function (x) {
    return this.prefix + ' ' + x;
  }.bind(this));    // this: Prefixer 생성자 함수의 인스턴스
};

var pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee', 'Kim']));
```
### 3.2 화살표 함수의 this

일반 함수는 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 this에 바인딩할 객체가 동적으로 결정된다.

화살표 함수는 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정된다.

동적으로 결정되는 일반 함수와는 달리 <strong>화살표 함수의 this는 언제나 상위 스코프의 this를 가리킨다. (Lexical this) </strong>

```
funtion Prefixer(prefix) {
    this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {
    // this는 상위 스코프인 prefixArray 메소드 내의 this를 가리킨다.
    return arr.map(x => `${this.prefix} ${x}`);
}

const per = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee', 'Kim']));
``` 

화살표 함수는 call, apply, bind 메소드를 사용하여 this를 변경할 수 없다.

```
window.x = 1;

const normal = function () { 
    return this.x; 
};
const arrow = () => this.x;

console.log(normal.call({ x: 10 }));    // 10
console.log(arrow.call({ x: 10 }));     // 1
```

<br>

## 4. 화살표 함수를 사용해서는 안되는 경우
화살표 함수는 Lexical this를 지원하므로 콜백 함수로 사용하기 편리하다.

하지만 화살표 함수를 사용하는 것이 오히려 혼란을 불러오는 경우도 있으므로 주의하여야 한다.

<br>

### 4.1 메소드
화살표 함수로 메소드를 정의하는 것은 피해야 한다. 화살표 함수로 메소드를 정의해 보면,
```
// 안좋은 예
const person = {
    name : 'Lee',
    sayHi : () => console.log(`Hi ${ this.name }`)
};

person.sayHi();     // Hi undefined
```

위 예제의 경우, 메소드로 정의한 화살표 함수 내부의 this는 메소드를 소유한 객체, 즉 메소드를 호출한 객체를 가리키지 않고 상위 컨택스트인 전역 객체 window를 가리킨다.
따라서 화살표 함수로 메소드를 정의하는 것은 바람직하지 않다.

이와 같은 경우는 메소드를 위한 단축 표기법인 ES6의 축약 메소드 표현을 사용하는 것이 좋다.

```
// 좋은 예
const person = {
    name : 'Lee',
    sayHi() {   // == sayHi : function() {
        console.log(`Hi ${this.name}`);
    }
};

person.sayHi();     // Hi Lee
```

<br>

### 4.2 prototype

화살표 함수로 정의된 메소드를 prototype에 할당하는 경우도 동일한 문제가 발생한다. 화살표 함수로 정의된 메소드를 prototype에 할당하여 보면,
```
// 안좋은 예
const person = {
    name : 'Lee',
};

Object.prototype.sayHi = () => console.log('Hi ${this.name}`);

person.sayHi();     // Hi undefined
```

화살표 함수로 객체를 정의했을 때와 같은 문제가 발생한다. 따라서 prototype에 메소드를 할당하는 경우, 일반 함수를 할당한다.
```
// 좋은 예
const person = {
  name: 'Lee',
};

Object.prototype.sayHi = function() {
  console.log(`Hi ${this.name}`);
};

person.sayHi(); // Hi Lee
```

<br>

### 4.3 생성자 함수

화살표 함수는 생성자 함수로 사용할 수 없다.

생성자 함수는 prototype 프로퍼티를 가지며 prototype 프로퍼티가 가리키는 프로토타입 객체의 constructor를 사용한다.

하지만 화살표 함수는 prototype 프로퍼티를 가지고 있지 않다.

```
const Foo = () => {};

// 화살표 함수는 prototype 프로퍼티가 없다
console.log(Foo.hasOwnProperty('prototype'));   // false

const foo = new Foo();      // TypeError: Foo is not a constructor
```

<br>

### 4.4 addEventListener 함수의 콜백 함수
addEventListener 함수의 콜백 함수를 화살표 함수로 정의하면 this가 상위 컨택스트인 전역 객체 winodw를 가리킨다.

```
// 안좋은 예
var button = document.getElementById('myButton');

button.addEventListener('click', () = {
    console.log(this == window);    // => true
    this.innerHTML = 'Clicked button';
});
```

따라서 addEventListener 함수의 콜백 함수 내에서 this를 사용하는 경우, funtion 키워드로 정의한 일반 함수로 사용하여야 한다.
일반 함수로 정의된 addEventListener 함수의 콜백 함수의 내부의 this는 이벤트 리스너에 바인딩된 요소(currentTarget)를 가리킨다.

```
// 좋은 예
var button = document.getElementById('myButton');

button.addEventListener('click', function() {
  console.log(this === button);     // => true
  this.innerHTML = 'Clicked button';
});
```

<br>
----------
[참조_poiemaweb](https://poiemaweb.com/es6-arrow-function)









