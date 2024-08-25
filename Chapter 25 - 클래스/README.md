## 클래스
ES6에 도입된 문법으로 클래스 기반 객체 지향 프로그래밍 언어와 흡사한 새로운 객체 생성 매커니즘

### 생성자 함수와의 차이점
1. 클래스를 new 연산자 없이 호출하면 에러가 발생한다.

2. 클래스는 상속을 지원하는 extends 키워드와 super 키워드를 제공한다.

3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다. 호이스팅이 발생은 한다 TDZ가 존재함

4. 모든 코드에는 암묵적으로 strict mode 가 지정되어 실행되며 해체가 불가능하다.

5. 클래스의  constructor , 프로토타입 메서드 , 정적 메서드 모두 프로퍼티 어트리뷰트의 [[Enumerable]]의 값이 false로 열거가 불가능하다.

## 클래스의 정의

class 키워드를 사용해 정의하며 파스칼 케이스를 사용하는 것이 일반적이다.
```
class Person {}
```

일반적이지 않지만 표현식 정의가 가능하다. 

=>클래스가 값으로 사용 가능한 일급 객체라는 의미

```
const person = class {};

const person = class MyClass {};
```

클래스 몸체에는 construrtor , 프로토타입 메서드 , 정적 메서드 세가지가 있다.

```
class Person {
    constructor(name) {
        this.name = name;
    }
    //프로토 타입 메서드
    sayHi() {
        console.log(`Hi! My name is ${this.name}`);
    }

    static sayHello() {
        console.log('Hello!');
    }
}

const me = new Person('Lee');

console.log(me.name); //Lee

me.sayHi(); // Hi! My name is Lee

Person.sayHello(); // Hello!
```

## 클래스 호이스팅

소스코드 평가 과정에 먼저 평가되어 함수 객체를 생성한다.

클래스가 평가 되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수, 즉 constructor 다.

함수 객체를 생성하는 시점에 프로토 타입도 더불어 생성된다.

그러나 클래스는 클래스 정의 이전에 참조할 수 없다.

let, const로 선언한 변수처럼 호이스팅 된다.

## 인스턴스 생성

클래스는 인스턴스를 생성하는 것이 유일한 존재 이유이므로 반드시 new 연산자와 함께 호출해야한다.

기명 클래스 표현식인 경우 클래스 이름을 사용해 인스턴스를 생성하면 에러가 발생한다.

외부 코드에서 접근 불가능하기 떄문이다.

```
const Person = class MyClass {};

const me = new Person();

const you = new MyClass();//ReferenceError ;
```

## 메서드

### constructor
인스턴스를 생성하고 초기화하기 위한 특수한 메서드

constructor는 메서드로 해석되는 것이 아닌 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다.

즉 클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성된다.

생략하면 빈 constructor가 암묵적으로 정의되며 빈 객체를 생성한다.

인스턴스 프로퍼티의 초기 값을 전달하려면 매개변수를 선언하고 생성할 때 초기값을 전달한다.

```
class Person {
    constructor (name , address) {
        this.name = name;
        this.address = address;
    }
}

const me = new Person('Lee' , 'Seoul');
console.log(me); //Person {name : 'Lee' , address: 'Seoul'}

```

인스턴스를 초기화 하려면 constructor를 생략해서는 안 되며

별도의 반환문을 갖지 않아야한다. 생성자 함수와 동일하게 암묵적으로 this를 반환하기 때문이다.

### 프로토타입 메서드
클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토 타입 메서드가 되며

생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.

생성자 함수의 역할을 클래스가 할 뿐이며 클래스도 프로토타입 기반 객체 생성메커니즘이다.

```
class Person() {
    constructor(name) {
        this.name = name;
    }
    
    sayHi() {
        console.log(`Hi! My name is ${this.name}`);
    }
}

const me = new Person('Lee');
me.sayHi();

```


### 정적 메서드
정적 메서드는 인스턴스를 생성하지 않아도 호출 할 수 있는 메서드를 말한다.

클래스에서는 메서드에 static 키워드를 붙이면 정적 메서드가 된다.

```
Class Person {
    constructor(name){
        this.name = name;
    }

    static sayHi(){
        console.log('Hi!');
    }
}


Person.sayHi();//Hi!

```


#### 정적 메서드와 프로토 타입 메서드의 차이

1. 정적 메서드와 프로토 타입 메서드는 자신이 속해 있는 프로토 타입 체인이 다르다.

2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.

3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

클래스 또는 생성자 함수를 하나의 네임스페이스로 활용하여 정적 메서드를 모아놓으면 이름 충돌 가능성을 줄여주고 함수들을 구조화 할 수 있는 효과가 있다.

유틸리티 함수를 전역 함수로 정의하지않고 메서드로 구조화 할 때 정적메서드가 유용하다.

## 클래스의 인스턴스 생성 과정
1. 인스턴스 생성과 this 바인딩

    new 연산자와 함께 클래스를 호출하면 암묵적으로 빈 객체를 생성한다. 이 빈객체가 클래스가 생성한 인스턴스다.

    이 때 클래스가 생성한 인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가리키는 객체가 설정된다.

    빈 객체(인스턴스)는 this에 바인딩이 된다 .

2. 인스턴스 초기화
    constructor 내부의 코드가 실행되어 this에 바인딩 되어 있는 인스턴스를 초기화한다.
    
    consturctor가 생략되면 이 과정도 생략된다
3. 인스턴스 반환
    클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

## 프로퍼티

### 인스턴스 프로퍼티

인스턴스 프로퍼티는 constructor 내부에 정의해야한다.

constructor 내부에서 this에 인스턴스 프로퍼티를 추가하며 인스턴스가 초기화 된다.


constructor 내부에서 this에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스가 되며 항상 public 하다.

### 접근자 프로퍼티

다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는접근자 함수로 구성된 프로퍼티이다.

getter의 메서드 이름 앞에 get 을 사용해 정의하고 setter는 set 키워드를 사용해 정의한다.

getter setter의 이름은 인스턴스 프로퍼티처럼 사용되며 getter는 반드시 무언가를 반환해야하며 setter는 무언가를 프로퍼티에 할당해야 한다.

setter는 단 하나의 값만 할당 받기 때문에 단 하나의 매개변수만 선언 할 수 있다.

클래스의 메서드는 기본적으로 프로토 타입 메서드이므로 접근자 프로퍼티또한 프로토타입 프로퍼티가 된다

```
Class Person {
    constructor (firstName , lastName){
        this.firstName = firstName;
        this.lastName = lastName;
    }

    get fullName() { //getter 함수
        return `${this.firstName} ${this.lastName}`;
    }

    set fullName(name){
        [this.firstName , this.lastName] = name.split(' ');
    }
}
```

## 클래스 필드 정의 제안

클래스 필드 :  클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어

- 자바에서의 클래스필드
```
public class Person {
    private string firstName = ""; //클래스 필드
}
```

 - 자바스크립트에서 클래스필드

외부 초기값으로 클래스 필드를 초기화 해야 한다면 기존의 constructor 에서 정의하는 방식을 사용해야 한다.
```
class Person {
    //클래스 필드에 문자열 할당
    //constructor 내부가 아니면 this 바인딩 사용 x
    name = "Lee"; 

    //클래스 필드에 함수를 할당
    //인스턴스의 메서드가 됨
    getName = function () {
        return this.name;
    }
}
```
## private 필드 정의 제안

인스턴스 프로퍼티는 클래스 외부에서 언제나 참조 가능한 public인데 외부에서 참조가 불가능한 private 필드가 제안되었다.

선두에 #을 붙여 선언하며 참조할 때도 #을 붙여주어야 한다.

또한 반드시 클래스 몸체에 정의를 해야한다.
```
class Person {
    #name = "";

    constructor(name) {
        this.#name = name;
    }
    get  name() { //접근자 프로퍼티 
        return this.#name.trim();
    }
}


const me = new Person(' Lee ');
console.log(me.name)
```


## static 필드 제안

자바스크립트의 static 키워드는 정적 필드를 정의할 수 없었다.

이를 위해 static public 필드 , static private 필드, static private 메서드를 정의할 수 있는 표준이 제안되었다.

```
class MyMath { 
    //static public 필드
    static PI = 22 / 7;

    //static private 필드
    static #num = 10;
    
    static increment() {
        return ++Mymath.#num;
    }
}

console.log(MyMath.PI);
console.log(MyMath.increment());
```

## 상속에 의한 클래스 확장

상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의 하는 것

상속을 통해 부모 클래스의 속성을 그대로 사용하면서 자신만의 고유한 속성만 추가하여 확장이 가능하다.

상속에 의한 클래스 확장은 코드 재사용 관점에서 매우 유용하다.

기본적으로 제공하는 extends 키워드를 사용하여 확장한다.

```
class Base {} //슈퍼(베이스 / 부모) 클래스

class Derived extends Base {} //파생 클래스 ,자식 클래스, 서브 클래스

```

수퍼클래스와 서브클래스는 인스턴스의 프로토타입 체인 뿐만 아니라 클래스 간의 프로토타입 체인도 생성하여 정적 메서드 , 프로토타입 메서드 모두 상속이 가능하다.

### 동적 상속

extends 키워드는 생성자 함수 또한 상속 받아 클래스로 확장이 가능하다.

[[Construct]] 내부 메서드를 갖는 함수 객체로 평가되는 모든 표현식을 상속받을 수 있다.

```
function Base1 () {}

class Base2 {}

let condition = true;

class Derived extends (condtion ? Base1  : Base2) {} // 동적 상속
```

## 서브클래스의 constructor
super 키워드를 통해 수퍼 클래스의 constructor를 호출하여 인스턴스를 생성한다.

빈 객체가 아닌 프로퍼티를 소유하는 인스턴스를 생성하려면 constructor 내부에서 인스턴스에 프로퍼티를 추가해야한다.

### super 키워드
함수처럼 호출 할 수도 있고 this와 같이 식별자처럼 참조할 수 있는 특수 키워드

- super를 호출하면 수퍼클래스의 constructor를 호출한다.
- super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

수퍼 클래스에 constuctor를 정의한 후 subclass에서 constructor를 생략하면 암묵적으로 super를 불러와 superclass의 constructor에 전달된다.

subclass에서 추가적으로 인수를 생성한 경우에는 생략이 불가능하다.

```
class Base {
    constructor(a,b) {
        this.a=a;
        this.b=b;
    }
}

class Derived1 extends Base {
    //constructor(...args) {super(...args)} 가 암묵적으로 정의됨
}

class Derived2 extends Base {
    constructor(a, b, c){ //생략 불가
        super(a,b);
        this.c = c;
    }
}
```

#### super 호출 규칙
1. 서브클래스에서 constructor를 생략하지 않는 경우 서브클래스의 constructor에서  super를 반드시 호출해야한다.
2. 서브클래스의 constructor에서 super를 호출하기 전에 this를 참조할 수 없다.
3. super는 반드시 서브 클래스의 constructor 내부에서만 호출한다.

### super 참조
1. 메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.
```
class Base {
    constructor(name) {
        this.name = name;
    }

    sayHi() {
        return `Hi! ${this.name};
    }
}


 class Derived extends Base {
    sayHi() {
        return `${super.sayHi()}. how are you doing?`;
    }

    //아래와 같이 동작한다.
    //sayHi() {
       // const __super = Object.getPrototypeOf(Derived.prototype);
       //return `${__super.sayHi.call(this)}. how are you doing?`; 
    //}
 }

```

super는 자신을 참조하고 있는 메서드가 바인딩 되어 있는 객체의 프로토타입을 가리킨다.

call method를 통해 this를 전달하지 않고 Base.prototype.sayHi()를 호출하면 메서드 내부의 this는 Base.prototype을 가리키게 된다.

Base.prototype.sayHi 메서드는 프로토 타입 메서드로 인스턴스를 가리켜야 한다 . name이 인스턴스에 존재하기 때문이다.

이처럼 super 참조가 동작하기 위해선 super를 참조하고 있는 메서드가 바인딩 되어 있는 객체의 프로토타입을 찾을 수 있어야 하는데 메서드 내부 슬롯[[HomeObject]]에서 자신을 바인딩 하는 객체를 가리켜 참조가 가능하다.

[[HomeObject]]는 ES6 메서드 축약 표현으로 정의된 함수만이 가지며 [[HomeObject]]를 가지는 함수는 super 참조가 가능하다.

2. 서브클래스의 정적 메서드 내에서 super.sayHi는 수퍼클래스의 정적 메서드 sayHi를 가리킨다.
```
class Base {
    static sayHi() {
        return `Hi!`;
    }
}

class Derived extends Base {
    static sayHi() {
        return `{super.sayHi()} how are you doing?';
    }
}

console.log(Derived.sayHi()); // Hi! how are you doing?

```