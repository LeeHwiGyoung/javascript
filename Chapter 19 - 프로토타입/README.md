## 객체지향 프로그래밍

자바스크립트에서는 프로토타입 기반의 객체지향 프로그래밍 언어이다.

객체지향 프로그래밍은 여러개의 독립전 단위 즉 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 말한다.

실세계의 실체(사물의 개념)를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작한다 .

실체는 특징이나 성질을 나타내는 속성을 가지고 있고 이를 통해 실체를 인식하거나 구분할 수있다.

예를 들어, 사람은 이름, 주소 , 성별, 나이 등 다양한 속성을 갖는다.

다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것을 `추상화`라고 한다.

```
const person = {
  name : 'Lee',
  address : 'Incheon'
}; //사람이라는 객체를 추상화 

const circle = { // 원이라는 객체를 추상화
  radius : 5, // 반지름 , 원의 상태를 나타내는 데이터

  getDiameter() { // 원의 지름을 구하는 동작
    return 2 * this.radius;
  }
}
```

객체는 `상태`를 나타내는 데이터와 상태데이터를 조작할 수 있는 `동작`을 하나의 논리적인 단위로 묶은 복합적인 자료구조라고 할 수 있다.

상태데이터 : 프로퍼티 , 동작 : 메서드

## 상속과 프로토타입

상속은 객체지향 프로그래밍의 핵심 개념으로 , 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것이다.

자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거하고 , 불필요한 중복을 제거하는 방법은 기존의 코드를 재사용하는 것이다.

```
 function Circle(radius) {
  this.radius = radius;

  this.getDiameter = function () {
    return this.radius * 2;
  }
}

const circle1 = new Circle(1);
const circle2 = new Circle(1);

console.log(circle1.getDiameter === circle2.getDiameter);// false
// 동일한 동작을 하는 메서드를 중복생성하게 된다.
```

위의 생성자 함수로 생성된 객체의 경우 생성된 모든 객체가 동일한 메서드를 중복 생성하여 메모리를 불필요하게 낭비한다.

이를 프로토타입 기반의 상속으로 중복을 제거할 수 있다.

```
function Circle(radius) {
  if(!(this instanceof Circle)){ // 스코프 세이프 생성자 패턴
    return new Circle(radius)
  }
  this.radius = radius; // 각 인스턴스는 고유의 상태 값을 갖는다.  
}

//프로토타입은 Circle 생성자 함수의 prototype에 바인딩 되어 있다.
Circle.prototype.getDiameter = function () {  //Circle 생성자 함수로 생성한 모든 인스턴스가 getDiameter 메서드를 공유한다.
  return 2 * this.radius;
}

const circle1 = new Circle(1)
const circle2 = new Circle(2)

console.log(circle1.getDiameter === circle2.getDiameter); // true

```

Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입 , 즉 상위 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속 받는다.

getDiameter 메서드는 단 하나만 생성되어 Circle.prototype의 메서드로 할당되어 있어 Circle 생성자 함수로 생성한 모든 인스턴스가 getDiameter 메서드를 상속받아 사용할 수 있다.

## 프로토타입 객체
프로토타입 객체는 객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용된다.

어떤 객체의 상위(부모) 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티를 제공한다.

하위(자식) 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.

모든 객체는 하나의 프로토타입을 갖으며 모든 프로토 타입은 생성자 함수와 연결되어 있다.

생성자 함수는 prototype 프로퍼티를 통해 프로토타입에 접근이 가능하며 생성자의 프로토타입은 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있다.

## __proto__ 접근자 프로퍼티
모든 객체는 __proto__  접근자 프로퍼티를 통해 자신의 프로토타입 , 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.

[[Prototype]]은 내부 슬롯으로 직접적으로 접근하거나 호출을 할 수는 없으며 __proto__ 접근자 프로퍼티를 통해 프로토타입에 접근이 가능하다.

__proto__ 접근자를 통해 프로토타입에 접근하는 이유는 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서이다.

프로토 타입 체인은 단방향 링크드 리스트로 구성되어야 하며 프로토 타입 체인의 종점은 Object.prototype이다.

## __proto__ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.

직접 상속을 통해 Object.prototype을 상속 받지 않는 객체를 생성할 수도 있기 때문에 __proto__ 접근자 프로퍼티를 사용할 수 없는 경우가 있다.

따라서 프로토타입의 참조를 취득하고 싶을 때는 Object.getPrototypeOf 메서드를 사용하고 , 프로토 타입을 교체하고 싶을 때는 Object.setPrototypeOf 메서드를 사용하는 것을 권장한다.

## 함수 객체의 prototype 프로퍼티
함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

| 구분 | 소유 | 값 | 사용 주체 | 사용 목적 |
| --- | --- | --- | --- | --- |
|__proto__ | 모든 객체 | 프로토타입의 참조 | 모든 객체 | 객체가 자신의 프로토 타입에 접근 또는 교체하기 위해 사용 |
| prototype | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 인스턴스의 프로토타입을 할당하기 위해 사용| 

> 프로토타입의 constructor 프로퍼티와 생성자 함수
모든 프로토타입은 constructor 프로퍼티를 갖는다. 이 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.

## 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖는데 프로토타입은 생성자 함수와 더불어 생성되며 prototype, constructor 프로퍼티에 의해 연결되어 있기 때문이다.

프로토타입과 생성자 함수는 단독으로 존재할 수 없고 항상 쌍으로 존재한다.

|리터럴 표기법| 생성자 함수 | 프로토 타입 |
| --- | --- | --- |
| 객체 리터럴 | Object | Object.prototype | 
| 함수 리터럴 | Function | Function.prototype | 
| 배열 리터럴 | Array | Array.prototype | 
| 정규 표현식 리터럴 | RegExp | RegExp.prototype |

## 프로토타입의 생성 시점

프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.

1. 사용자 정의 생성자 함수와 프로토 타입 생성 시점
=> 생성자 함수로서 호출할 수 있는 함수 , 즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해서 먼저 실행된다.

함수 선언문으로 정의된 생성자 함수는 먼저 평가되어 함수 객체가 되며 이 때 프로토타입도 더불어 생성된다.

생성된 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩 된다.

prototype은 consturctor 프로퍼티 만을 갖는 객체로 객체이기 때문에 프로토타입을 갖는다.

prototype의 프로토타입은 Object.prototype이다.

2. 빌트인 생성자 함수와 프로토타입 생성 시점

Object, String, Number, Function, Array,RegExp,Date, Promise 등과 같은 빌트인 생성자 함수도 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.

모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성이 되며 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩 된다.

이처럼 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화 되어 존재하며 이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면

프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당되며 객체는 프로토타입을 상속받는다.

## 프로토타입 체인

자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 

자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다 . 이를 프로토타입 체인이라고 한다.

자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.

✨ 모든 객체의 프로토 타입 체인 최상단은 Object.prototype이며 이를 프로토타입 체인의 종점이라고 한다. 프로토 타입의 종점에서도 프로퍼티를 검색할 수 없을 경우 오류가 아닌 undefined를 반환한다.

> 프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘이고 스코프 체인은 식별자 검색을 위한 메커니즘이다. 스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용한다.

## 오버라이딩과 프로퍼티 섀도잉

프로토타입 프로퍼티 : 프로토타입이 소유한 프로퍼티

인스턴스 프로퍼티 : 인스턴스가 소유한 프로퍼티

인스턴스에 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 추가하면 프로토타입 체인을따라 프로토 타입 

프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰는 것이 아니라 인스턴스 프로퍼티로 추가한다.

이 때 가려지는 메서드를 오버라이딩 했다고 하며 상속관계에 의해 프로퍼티가 가려지는 현상을 프로퍼티 

섀도잉이라고 한다.

프로퍼티 삭제 : 인스턴스와 프로토타입 프로퍼티가 같은 경우에 인스턴스의 프로퍼티가 삭제되며

오버라이딩 되어 있지 않은 프로퍼티를 프로토타입 체인을 통해 삭제하는 것을 불가능하다.

즉 get은 가능하나 set은 허용되지 않는다.

```
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  Person.prototype.sayHello = function() {
    console.log(`Hi, my name is ${this.name}`);
  }

  return Person;
}

const me = new Person('Lee');

me.sayHello = function () {
  console.log(`Hey, my name is ${this.name}`);// 인스턴스 메서드 및 오버라이딩
}


delete me.sayHello; //인스턴스 메서드 삭제

me.sayHello(); //'Hi my name is Lee'

delete me.sayHello; //프로토 타입 체인을 통해서 삭제는 되지 않는다.

me.sayHello(); //'Hi my name is Lee'

delete Person.prototype.sayHello;

me.sayHello();// TypeError: me.sayHello is not a function 삭제를 위해서는 프로토타입에 직접 접근해야한다.

```

## 프로토타입의 교체
프로토타입은 임의로 다른 객체로 변경할 수 있다. 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미한다.
```
const Person = (function () {
  function Person(name) {
    this.name;
  }

  Person.prototype = { //constructor 를 명시하지 않아 생성자 함수가 Object로 교체된다. 
    sayHello() {
      console.log(`Hi! my name is ${this.name});
    }
  }

  return Person;
}());

const me = new Person('Lee');

console.log(me.constructor === Person);//false
console.log(me.constructor === Object);//true
```

프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없으며 constructor 프로퍼티는 자바스크립트 엔진이 암묵적으로 추가한 프로퍼티다.

프로토 타입에 constructor를 명시하여 프로토타입의 constructor의 프로퍼티를 되살릴 수 있다.

## 인스턴스에 의한 프로토타입의 교체
인스턴스의 __proto__ 접근자 프로퍼티(또는 Object.getPrototypeOf 메서드)를 통해 접근할 수 있다.

따라서 __proto__ 접근자 프로퍼티 또는 Object.setPrototypeOf 메서드를 통해 프로토타입을 교체할 수 있다.
```
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

const parent = {
  sayHello() {
    console.log(`Hi! My name is ${this.name});
  }
};

Object.setPrototypeOf(me , parent); //me 객체를 프로토타입을 parent 객체로 교체한다.

me.sayHello(); //'Hi! My name is Lee' 

console.log(me.constructor === person); //false
```
프로토타입의 교체와 마찬가지로 프로토타입으로 교체한 객체는 consturctor 프러퍼티가 없으므로 생성자 함수간의 연결이 파괴된다.

> 인스턴스에 의한 프로토타입 교체와 생성자 함수로 인한 프로토타입 교체의 차이
> 생성자 함수에 의한 프로토타입 교체는 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가르키나 인스턴스는 그렇지 않다.

=> 프로토타입 교체를 통해 객체간의 상속 관계를 동적으로 변경하는 것은 꽤나 번거롭기 때문에 직접 교체하지 않는 것이 좋다. 인위적으로 설정하려면 `직접 상속`을 통해 변경하는 것이 더 안전하고 편리하다.

## instanceof 연산자

객체 instanceof 생성자 함수 : 우변의 생성자 함수의  prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인상에 존재하면 true , 그렇지 않은 경우에는 false를 반환한다.

```
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

console.log(me instanceof Person); //true

console.log(me instanceof Object); //true

const parent = {};

object.setPrototypeOf(me , parent);

console.log(me instanceof Person); //false

console.log(me instanceof Object); //true

console.log(Person.prototype === parent); // false

```
instanceof 연산자는 프로토타입의 constructor  프로퍼티가 가리키는 생성자 함수를 찾는 것이 아니라

생성자 함수의  prototype에 바인딩 된 객체가 프로토타입 체인상에 존재하는지 확인한다.

따라서 생성자 함수에 의해 프로토타입이 교체되어 consturctor와 프로퍼티와 생성자 함수간의 연결이 파괴되어도 

prototype 프로퍼티와 프로토타입 간의 연결은 파괴되지 않으므로 instanceof는 아무런 영향을 받지 않는다.

```
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  Person.prototype = { //프로토타입의 constructor 는 Object가 된다.
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    }
  };

  return Person;
}());

const me = new Person('Lee');

console.log(me instanceof Person); //true

console.log(me instanceof Object); //true
```

## 직접 상속 
1. Object.create에 의한 직접 상속
   Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성하며 다른 생성 방식과 마찬가지로 추상연산 OrdinaryObjectCreate를 호출한다.
   매개변수로 생성할 객체의 프로토타입으로 지정할 객체 , 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이루어진 객체를 전달한다.
   두번째 인수는 옵션으로 생략이 가능하다.
```
let obj = Object.create(null);

console.log(Object.getPrototypeOf(obj)=== null); // true Object.prototype을 상속받지 못함

obj = Object.create(Object.prototype);

console.log(Object.getPrototypeOf(obj)=== Object.prototype); // true

const myProto = {x : 10};

obj = Object.create(myProto);
console.log(obj.x); //10
console.log(obj.getPrototypeOf(obj) === myProto) // true

function Person(name) {
  this.name = name;
}

obj = Object.create(Person.prototype);
obj.name = 'Lee';
console.log(obj.name);//Lee
console.log(obj.getPrototypeOf(obj) === person.prototype);//true
```

✨ Object.create 메서드의 장점
1. new  연산자 없이도 객체를 생성할 수 있다.
2. 프로토타입을 지정하면서 객체를 생성할 숟 ㅗ있다.
3. 객체리터럴에 의해 생성된 객체도 상속을 받을 수 있다.

2. 객체리터럴 내부에서 __proto__에 의한 직접 상속
   Object.create 메서드에 의한 직접 상속은 장점이 많으나 두 번째 인자로 프로퍼티를 정의하는 것은 번거롭다 또한 객체 생성 이후 프로퍼티를 추가하는 방법도 있으나

   이또한 깔끔하지 않다. ES6부터는 객체 리터럴 내부에 __proto__ 접근자 프로퍼티를 사용하여 직접 상속할 수 있다.

```
const myProto  = { x : 10}

const obj = {
  y : 20,
  __proto__: myProto
};

console.log(obj.x, obj.y); //10 20
console.log(Object.getPrototypeOf(obj) === myProto)// true
```

## 정적 프로퍼티/메서드
생성자 함수로 인스턴스를 생성하지 않아도 참조/호출 할 수 있는 프로퍼티/메서드이다.

```
function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = function () {
  return console.log(`Hi! My name is ${this.name}`);
};

Person.staticProp = 'staticProp'; //정적메서드

Person.staticMethod = function () {
  console.log(`staticMethod`);
}

const me = new Person(`Lee`);

Person.staticMethod(); //staticMethod 정적 메서드는 생성자 함수로 참조/호출한다.

me.staticMethod(); //인스턴스로는 참조/호출이 불가능하다 . 프로토타입 체인 상에 존재해야 가능하다.

```

인스턴스/프로토타입 메서드 내에서 this를 사용하지 않는다면 정적메서드로 변경이 가능하다.

```
function Foo() {
Foo.prototype.x = function () {
  console.log('x');
}

Foo.x = function() {
  console.log('x');
};

Foo.x(); // x

```

>프로토타입 프로퍼티/메서드를 표기할 때 prototype을 #으로 표기하는 경우를 알아두자.
>ex) Object.prototype.isPrototypeOf === Object#isPrototypeOf

## 프로퍼티 존재 확인
1. in 연산자
객체 내부에 특정 프로퍼티가 존재하는지 여부를 확인한다.
```
 const person = {
   name : 'Lee',
   address: 'Seoul'
 };

 console.log('name' in person); // true
 console.log('age' in person); // false
 ```
> in 연산자는 객체가 속한 프로토타입 체인상에 존재하는 모든 프로토타입에서 프로퍼티를 검색하기 때문에 이를 주의해야한다.
 같은 연산자로 ES6에서 도입된 Reflect.has 메서드가 있다.

2. Object.prototype.hasOwnProperty 메서드
in 연산자와 다르게 객체의 고유한 프로퍼티인 경우에만 true를 반환한다.
```
console.log(person.hasOwnProperty('toString'); //false
```

## 프로퍼티 열거
1.for...in 문
```
for (변수선언문 in 객체) {} //객체의 프로퍼티 키가 변수에 할당되어 순회된다.

const person = {
  name : 'Lee',
  address : 'Seoul'
};

for (const key in person) {
  console.log(key + ' : ' + person[key]);
}

//name : Lee
//address : Seoul

```
in  연산자와 동일하게 상속받은 프로퍼티또한 열거한다.
그러나 프로토타입 체인상에 존재하는 프로토타입의 프로퍼티중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 경우에 열거한다.

프로퍼티 키가 Symbol인 경우에도 열거하지 않는다.

자신의 고유한 프로퍼티만 열거하려면 `Object.prototype.hasOwnProperty` 메서드를 사용하여 객체 자신의 프로퍼티인지 확인해야한다.

## 배열의 열거

배열인 경우에는 for문이나 for...of문 또는 Array.prototype.forEach 메서드를 사용하기를 권장한다.

```
const arr = [1,2,3];
arr.x = 10; //배열도 객체이므로 프로퍼티를 가질 수 있다.

for(const i in arr) {
  console.log(arr[i]); // 1 2 3 10 프로퍼티x도 출력된다.
}

for(let i = 0 ; i < arr.length ; i++) {
  console.log(arr[i]); //1 2 3
}

arr.forEach(v => console.log(v)); // 1 2 3

for(const value of arr) { //of 메서드는 변수에 키가 아닌 값을 할당한다.
  console.log(value); // 1 2 3 
}
```
## Object.keys/ values/ entries 메서드
객체 고유의 프로퍼티를 반환하기 위해서 사용한다.
1. Object.keys : 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.
2. Object.values : 객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환한다.
3. Object.entries : 객체 자신의 열거가능한 프로퍼티 키와 프로퍼티 값의 쌍의 배열을 배열에 담아 반환한다. 
```
