this
==========

<Strong>Java vs JavaScript</Strong>

Java에서의 this

> Java에서의 this는 객체 자신(self)을 가리키는 참조변수로, this가 객체 자신에 대한 참조 값을 가지고 있다는 뜻이다.
>
> 주로 매개변수와 객체 자신이 가지고 있는 멤버변수명이 같을 경우 이를 구분하기위해 사용된다.
```
public Class Car{
    private String name;            // a

    public Person(String name) {    // b
        this.name = name;           // this.a == b;
    }
}
```
<br>

JavaScript에서의 this
> JavaScript의 경우 함수 호출 방식에 의해 this에 바인딩할 어떤 객체가 동적으로 결정된다.
>
>   > 함수호출
>   >
>   > 내부 함수는 일반함수, 메소드, 콜백함수 어디에서 선언되었는지에 관계없이 this는 전역객체를 바인딩한다.
>   >
>   > 전역 객체는 모든 객체의 최상위 객체를 의미하며 일반적으로 Browser-side에서는 window, Server-side(Node.js)에서는 global 객체를 의미한다. 

```
var obj = {
    a : function() {
        console.log(this);
    }
};

obj.a();    // obj
```
위 코드에서 this는 window나 global 객체가 아닌 obj를 가리킨다.

객체의 메소드 호출할 때 this를 내부적으로 바꿔주기 때문이다.

<br><br>

```
function MyClass () {
    this.property1 = "value1";
}

MyClass.prototype.method1 = function() {
    console.log(this.property1);
}

var my1 = new MyClass();
my1.method1();
```

MyClass 메소드를 호출한 객체는 my1이 되며 method1은 메소드가 된다.

method1()이 실행되면 메소드 내부에는 JavaScript 엔진에 의해 this 속성이 생기게 된다.

this에는 method1을 호출한 my1 객체가 저장된다.


<br><br>
## 일반 함수에서의 this
```
var data = 10;       // 1 

function outer() { 
    this.data = 20;  // 2 
    data = 30;       // 3 

    console.log("1. data = " + data); 
    console.log("2. this.data = " + this.data); 
    console.log("3. window.data = " + window.data); 
} 

outer();
```
일반 함수 내부에서 this는 전역 객체인 window가 저장된다.

2의 data는 window.data와 동일하기 때문에, 1의 data에 20이 저장된다.

3이 실행되는 경우 먼저 지역 변수 내에서 data를 찾고 없으면 outer()를 호출한 영역에서 찾기 때문에 3의 data도 1의 data에 30이 저장된다.

위 1, 2, 3의 data 모두 전역 변수인 data를 나타낸다. (1의 data)


<br><br>
## 일반 중첩 함수에서의 this
```
var data = 10;            // 1

function outer() {
    function inner() {
        this.data = 20;   // 2
        data = 30;        // 3

        console.log("1. data = " + data);
        console.log("2. this.data = " + this.data);
        console.log("3. window.data = " + window.data);
    }
    inner();
}

outer();
```

일반 중첩 함수에서 this 역시 window가 된다

이에 따라 2의 this.data는 1의 전역 변수인 data와 동일하기 때문에 1의 data에 20이 저장된다.

3의 data는 우선 지역 변수에서 data를 찾고, 없으면 outer()를 호출한 영역에서 data를 찾는다.

inner() 와 outer() 모두 data가 없기 때문에, 3의 data 역시 1의 전역 변수인 data에 저장된다.

따라서 1의 전역 변수인 data는 30이 된다.


<br><br>
## 이벤트 리스너에서의 this
```
var data = 10;         // 1
$("#myButton").click(function () {
    this.data = 20;    // 2
    data = 30;         // 3

    console.log("1. data = " + data);
    console.log("2. this.data = " + this.data);
    console.log("3. window.data = " + window.data);
});
```
이벤트 리스너에서 this는 이벤트를 발생시킨 객체가 된다.

그래서 this는 `#myButton`이 된다.

이에 따라 2는 `#myButton` 객체에 data라는 프로퍼티를 동적으로 추가하는 구문이 된다.

`#myButton`의 data 프로퍼티에 20을 저장한다.

3의 data는 우선 지역 변수 내에서 data를 찾고, 없으면 상위 영역에서 data를 찾게 된다.

3의 data는 1의 전역변수 data가 되어 30이 저장된다.

2의 data는 `#myButton` 객체의 data, 3의 data는 전역 변수인 data(1의 data)를 나타낸다.
 
<br><br>
## 메소드에서의 this
```
var data = 10;              // 1

function MyClass () {
    this.data = 0;
}

MyClass.prototype.method1 = function () {
    this.data = 20;         // 2
    data = 30;              // 3

    console.log("1. data = " + data);
    console.log("2. this.data = " + this.data);
    console.log("3. window.data = " + window.data);
}

var my1 = new MyClass();
my1.method1();
```
메소드에서의 this는 메소드를 호출한 객체가 된다.

2의 data는 객체의 프로퍼티가 되어 의 my1.data에 20이 저장된다.

3의 data는 우선 지역 변수 내에서 data를 찾고, 없으면 상위 영역에서 data를 찾는다.

결국 3의 data는 1의 전역 변수 data가 되어 30을 저장한다.

2의 data는 my1 객체의 data, 3의 data는 전역 변수인 1의 data를 나타낸다.

<br><br>
## 메소드 내부의 중첩 함수에서의 this
```
var data = 10;                  // 1

function MyClass () {
    this.data = 0;
}

MyClass.prototype.method1 = function () {
    function inner() {
        this.data = 20;         // 2
        data = 30;              // 3

        console.log("1. data = " + data);
        console.log("2. this.data = " + this.data);
        console.log("3. window.data = " + window.data);
    }
    inner();
}

var my1 = new MyClass();
my1.method1();
```
객체의 메소드 내부에서 만들어지는 중첩 함수에서 this는 객체가 아닌 window가 된다.

이에 따라 2의 data는 전역 변수인 1의 data에 20이 저장된다.

3의 data는 우선 지역 변수에서 data를 찾은 뒤, 없으면 상위 영역에서 data를 찾는다.

결국 3의 data는 1의 전역 변수인 data에 30이 저장된다.

1, 2, 3의 data 모두 1의 전역변수인 data에 저장된다.

<br><br>

---------
## 정리

> 일반 함수에서 this는 <strong>window</strong>
>
> 중첩 함수에서 this는 <strong>window</strong>
>
> 이벤트에서 this는 <strong>이벤트 객체</strong>
>
> 메소드에서 this는 <strong>메소드 객체</strong>
>
> 메소드 내부의 중첩 함수에서 this는 <strong>window</strong>

<br><br>
---------
## 참조
[참조1_버미노트](https://beomy.tistory.com/6)

[참조2_공부혜옹](https://hae-ong.tistory.com/14)