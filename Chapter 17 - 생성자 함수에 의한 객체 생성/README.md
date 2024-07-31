## Object 생성자 함수
new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환하며 프로퍼티 또는 메서드를 추가해 객체를 완성할 수 있다.
> 생성자 함수 : new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수
```
  const person = new Object();

  person.name = "Lee";
  person.sayHi = function () {
    console.log('Hi! my name is ' + this.name);
  }

  console.log(person); // { name : "Lee" , sayHi : f }
  person.sayHi(); // 'Hi! my name is Lee'
 
```

## 생성자 함수
동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 객체 리터럴에 의한 객체 생성 방식은 비효율적이다.

생성자 함수에 의해 객체 생성을 하면 인스턴스를 생성하기 위한 템플릿처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.  

클래스 기반 객체지향 언어와다르게 일반함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.

```
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  }
}

const circle1 = new Circle(1); // 반지름이 1인 Circle 객체
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체
```

✨ this 바인딩
> this 는 객체 자신의프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이다. this가 가리키는 값 ,
  즉 this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.

| 함수 호출 방식 | this가 가리키는 값(this 바인딩) |
| --- | --- |
| 일반 함수로서의 호출 | 전역 객체 |
| 메서드로의 호출 | 메서드를 호출한 객체 (마침표 앞의 객체) |
| 생성자 함수로서 호출 | 생성자 함수가 생성할 인스턴스(객체) | 


## 생성자 함수의 인스턴스 생성 과정
생성자 함수의 역할은 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿으로 동작하여 인스턴스를생성하는 것과

생성된 인스턴스를 초기화 (인스턴스 프로퍼티 추가 및 초기값 할당)을 하는 것이다.
1. 인스턴스 생성과 this 바인딩
   
   생성자 함수를 호출하면 암묵적으로 빈 객체가 생성된다.

   암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩이 된다.
   
   이 처리는 런타임 이전에 실행된다.
   
2. 인스턴스 초기화
   런타임 시 코드가 실행되어 this 바인딩이 되어 있는 인스턴스를 초기화한다.

   this에 바인딩 되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달 받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정 값을 할당한다.

3. 인스턴스 반환
   생성자 함수의 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

   this가 아닌 다른 객체를 명시한다면this가 반환되지 못하고 명시한 객체가 반환된다. 그러나 원시 값을 반환하면

   원시 값 반환은 무시되고 this가 반환된다.

   명시적으로 객체를 반환하는 것은 생성자 함수의 기본 동작을 훼손하므로 생성자 함수 내부에서 return문을 반드시 생략해야한다.

```
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  }
}

const circle = new Circle(1);
/*
  1. 인스턴스 생성과 this 바인딩
  암묵적으로 빈 객체가 생성된다. circle = {};
  this 바인딩이되며 this는 circle을 가리킨다.
  2. this에 바인딩  된 인스턴스를 초기화 시키다.
  { radius : 1 , getDiameter : f }
  3. 완성된 인스턴스가 바인딩된 this가 반환된다.
  console.log(circle) // Circle {radius : 1 , getDiameter : f }
*/

```

## 내부 메서드 [[Call]] , [[Construct]]
함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수로 호출될 수 있는 것은 물론 생성자 함수로도 호출이 가능하다.

함수는 객체이므로 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 가지고 있으며 일반 객체와 다르게 호출할 수 있으므로

[[Environment]] , [[FormalParameters]] 등의 내부 슬롯과 [[Call]] , [[Construct]] 같은 내부 메서드를 갖고 있다. 

[[Call]] 내부 메서드를 갖는 함수 객체를 callable이라 하며 
[[Construct]] 를 갖는 함수 객체를 constructor , 갖지 않는 함수 객체를 non-constructor라 한다.

| 구분 | 종류 | 
|---|---|
| constructor | 함수 선언문 , 함수표현식 , 클래스 |
| non-constructor | 메서드(ES6 메서드 축약 표현) , 화살표 함수 |

> ECMAScript 사양에서 메서드로인정하는 범위가 일반적인 의미의 메서드보다 좁다는 것을 주의해야 한다.

```
function foo() {} //함수 선언문

const bar = function () {} //함수 표현식

const baz = {
  x : function () {} //프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수이다 . 이는 메서드로 인정하지 않는다.  
};

new foo(); // {} 일반함수도 생성자 함수처럼 동작할 수 있음을 주의해야한다.
  
new baz.x(); // x { }

const arrow = () => {};

new arrow(); //non-constructor 함수로 타입 에러가 발생

const obj = {
  x() {} //ES6 메서드 축약 표현
};

new obj.x(); // non-constructor 함수로 타입 에러
```

## new 연산자
new 연산자와 함께 함수를 호출하면 생성자 함수로 동작하게 된다.

단, non-constructor 함수가 아니어야 한다.

new 연산자와 함께 사용하지 않을 시 일반 함수로 호출이 된다.

```
function Circle(radius) { //생성자 함수 파스칼 케이스를 사용해서 구분한다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  }
}

const circle  = Circle(5);
console.log(circle); //undefined 일반함수로 동작했기 때문에

// 일반 함수 내부의 this는 전역객체로 바인딩된다.
console.log(radius); //5
console.log(getDiameter()); //10

console.log(circle.getDiameter())// typeError : Cannot read property 'getDiameter' of undefined

```

## new.target
파스칼 케이스로 생성자 함수를 구분한다 하더라도 생성자 함수가 new 연산자 없이 호출하는 실수가 발생할 수 있다.

이 위험성을 회피하기 위해 ES6에서 new.target 을 지원한다.

단, IE에서는 지원하지않는다.

함수 내부에서 new.target을 사용하면 new 연산자와 함께 생성자 함수로 호출되었는지 확인할 수 있게 된다.

함수 내부에서 new 연산자 함수와 함께 생성자 함수로서 호출되면 new.target 연산자는 함수 자신을 가리킨다.

일반함수로 호출되면 new.target은 undefined의 값을 갖는다.

```
function Circle(radius) {
  if(!new.target) { //일반 함수로 호출 되면
    return new Circle(radius); //생성자 함수로 재귀호출하여 반환
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  }
}

const circle = Circle(5);
console.log(circle.getDiameter()); //10 
```

✨ 스코프 세이프 생성자 패턴
> new.target을 사용할 수 없는 상황에서 생성자 함수의 일반함수 호출을 방지하는 패턴
```
function Circle(radius) {
  // 일반 함수로 호출되면 this가 전역 객체에 바인딩 되는 것을 이용한다.
  if(!(this instanceof Circle)){
    return new Circle(radius);
  }

  this.radius =  radius;
  this.getDiameter = function () {
    return 2 * radius;
  }
}

const circle = Circle(5);
console.log(circle.getDiameter()); // 10  new 키워드가 없어도 생성자 함수로 호출된다.

```   
