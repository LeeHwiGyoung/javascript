## 클로저
클로저는 함수와 그 함수가 선언덴 렉시컬 환경과의 조합이다.

### 렉시컬 스코프
렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조 값, 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경에 의해 결정된다.

이를 렉시컬 스코프라고 한다.


### 함수 객체의 내부 슬롯 [[Environment]]

렉시컬 스코프가 가능하려면 함수의 상위 스코프를 기억해야 한다.

함수 객체의 내부 슬롯 [[Enviornment]]에 함수가 평가되어 함수 객체가 생성될 때 현재 실행중인 실행 컨텍스트의 렉시컬 환경의 참조를 저장한다.

[[Environment]]의 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 상위 스코프이며 , 자신이 호출 되었을 때  생성 될 함수 렉시컬 환경의 

"외부 렉시컬 환경에 대한 참조"에 저장될 참조 값으로 , 자신이 존재하는 한 상위 스코프를 기억한다.

### 클로저와 렉시컬 환경

외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩함수는 이미 생명주기가 종료한 외부 함수의 변수를 참조 할 수 있는데 이러한 중첩 함수를 클로저라고 한다.

외부 함수의 실행 컨텍스트가 실행 컨텍스트 스택에서 제거 되어도 중첩 함수의 [[Enviornment]]에 의해 참조되고 있고 중첩 함수는 전역 변수에 의해 참조 되면서 

가비지 컬렉션의 대상이 되지 않고 메모리에 남아 있는다. 

중첩 함수라도 상위 함수의 어떤 식별자도 참조하지 않는 경우 모던 브라우저의 최적화를 통해 상위 스코프를 기억하지 않는다.

```
const x = 1;
function outer () {
    const x = 10;
    const inner = function() {
        console.log(x);
    }
    return inner;
}

const innerFunc = outer();
innerFunc();

```


### 클로저의 활용

클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.

상태를 안전하게 은닉하고 특정 함수에게만 상태를 변경하고 허용하기 위해서 사용된다.

```
const counter = (function () {
    let counter = 0; //자유 변수

    return function (aux) {
        counter = aux(counter);
        return counter;
    }
}());


function increase(n) {
    return ++n;
}

function decrease(n) {
    return --n;
}

console.log(counter(increase)); //1
console.log(counter(increase)); //2
console.log(counter(decrease)); //1
console.log(counter(decrease)); //0

```

### 캡슐화와 정보 은닉
캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조할 수 있는 동작인 메서드를하나로 묶는 것을 말한다.

정보은닉은 외부에공개할 필요가 없는 구현의 일부를 외부에 감추어 적절치 못한 접근으로 객체의 상태가 변경되는 것을 보호하고 

객체간의 상호 의존성, 즉 결합도를 낮춘다.

```
const person = (function () {
    let _age = 0; // private

    function  Person (name , age) {
        this.name = name;
        _age = age;
    }

    Person.prototype.sayHi = function () {
        console.log(`Hi, My name is ${this.name} . I am ${_age}`);
    }

    return Person;
}());

const me = new Person('Lee' , 20);
me.sayHi(); // Hi, My name is Lee . I am 20

const you = new Person('Kim' , 30);
you.sayHi(); //Hi, My name is kim . I am 30

console.log(you.name);//kim
console.log(you._age);//undefined 은닉화

me.sayHi();// Hi, My name is Lee . I am 30
```

person 생성자가 함수 여러개의 인스턴스를 생성할 경우 _age의 변수 상태가 유지 되지 않는 문제가 발생.