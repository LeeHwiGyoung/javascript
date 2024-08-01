## 일급 객체
다음과 같은 조건을 만족하는 객체를 일급 객체라고 한다.

1. 무명의 리터럴로 생성할 수 있다. 즉 , 런타임에 생성이 가능하다.
2. 변수나 자료구조(객체, 배열) 등에 저장이 가능하다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

   
```
// 1. 함수는 무명의 리터럴로 생성할 수 있다.
// 2. 함수는 변수에 저장이 가능하다.
const increase = function (num) {
  return num ++;
}

const auxs = { increase }; //2.함수는 객체에 저장할 수 있다.

function makeCounter (aux) {
  let num = 0;

  return function () { // 4. 함수의 반환값으로 사용할 수 있다.
    num = aux(num);
    return num;
  }
}

const increaser = makeCount(auxs.increase); // 3.함수의 매개변수에 전달할 수 있다. 
```

함수가 일급 객체라는 것은 함수를 객체와 동일하게 사용할 수 있다는 의미이다.

일급 객체로서 함수가 가지는 가장 큰 특징은 일반 객체와 같이 함수의 매개변수에 전달할 수 있으며, 함수의 반환값으로 사용할 수 있다는 것이다.

이는 함수형 프로그래밍을 가능하게 하는 자바스크립트의 장점 중 하나이다.

## 함수 객체의 프로퍼티

### arguments 프로퍼티
함수 객체의 arguments 프로퍼티 값은 arguments 객체다.

arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며 함수 내부에서 지역 변수처럼 사용된다.

auguments 프로퍼티는 일부 브라우저에서 지원하고 있지만 ES3부터 표준에서 폐지되었으며 `arguments 객체`를 참조하도록 해야한다. 

함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언되고 undefined로 초기화된 이후 인수가 할당된다.

인수가 부족하면 매개 변수는 undefined로 초기화 된 상태를 유지하며 인수가 넘칠때는 무시된다.

argument 객체는 배열이 아닌 유사 배열 객체이므로 배열 메서드를 사용할 경우 에러가 발생한다.

Function.prototype.call , Function.prototype.apply 를 간접 호출해야 하는 번거로움이 있다.

이런 번거로움을 해결하기 위해 ES6에서 rest 파라미터가 도입되었다.

```
function sum() {
  const array = Array.prototype.slice.call(arguments); // 유사 배열 객체이므로
  return array.reduce(function (pre , cur) {
    return pre + cur;
  } , 0 );
}

//rest 파라미터 도입

function sum(...args) {
  return args.reduce((pre , cur) => pre + cur , 0);
}
```
### caller 프로퍼티
caller 프로퍼티는 함수 자신을 호출한 함수를 가리킨다.
ECMAScript 사양에 포함되지 않는 비표준 프로퍼티이다.

### length 프로퍼티
함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.
>arguments 객체의 length 프로퍼티는 인자의 개수를 , 함수 객체의 length 프로퍼티는 매개변수의 개수를 가리킨다.
```
function foo() {}
console.log(foo.length); // 0

function bar(x) {
  return x;
}
console.log(bar.length); // 1
```

### name 프로퍼티
name 프로퍼티는 ES6에서 정식 표준이 된 프로퍼티로 함수 객체를 가리키는 식별자를 값으로 갖는다.
> ES5와 ES6에서 동작을 다르게 하는 것에 주의를 해야한다.
> 익명 함수 표현식인 경우 ES5에서 name 프로퍼티는 빈 문자열을 값으로 갖지만 ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.

### __proto__ 접근자 프로퍼티
모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다.
[[Prototype]] 내부 슬롯은 객체지향프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.

__proto__ 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티이며 간접적인 접근 방법을 제공하는 경우에 한하여 접근할 수 있다.

```
const obj = { a : 1 };

console.log(obj.__proto__ === Object.prototype); //true

console.log(hasOwnProperty('__proto__');//false
```
>hasOwnProperty 메서드는 전달 받은 프로퍼티의 키가 객체 고유의 프로퍼티 키인 경우에만 true를 반환하고 상속 받은 프로토타입의 프로퍼티 키인 경우 false를 반환한다.

 ### prototype 프로퍼티
생성자 함수를 호출할 수 있는 함수 객체, 즉 constructor 만이 소유하는 프로퍼티이다.

prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 떄 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.

