## ES6 이전의 함수

자바스크립트에서 함수는 별다른 구분 없이 다양한 목적으로 사용 됨
- new 연사자와 함께 사용하여 생성자 함수
- 객체에 바인딩되어 메서드로 호출
- 일반 함수로 호출

=> 편해보이나 실수를 유발시킬 수 있으며 성능면에서 손해

why ? 사용 목적의 구분이 되지 않아 모든 함수가 일반 함수로도 호출 될 수 있고 생성자 함수로도 호출 될 수 있어 불필요하게 prototype 프로퍼티를 가졌다.

## ES6 이후의 함수

함수를 사용 목적에 따라 세 가지 종류로 명확히 구분

|ES6 함수의 구분 | constructor | prototype | super | arguments | 
|---| :---: |:---:|:---:|:---:|
| 일반 함수(Normal) | O  | O | X | O |
| 메서드(Method) | X | X | O | O |
|화살표함수(Arrow) | O | O | O |O |

## 메서드

객체에 바인딩 된 함수가 아닌 ES6 사양에서으이 메서드 축약 표현으로 정의된 함수만을 의미

ES6 사양에서 정의한 메서드는 인스턴스를 생성할수 없는 non-constructor로 생성자 함수로 호출 불가능

자신을 바인당한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖기 때문에 super 참조가 가능

```
const base = {
    name : 'Lee',
    sayHi () {
        return `Hi! ${this.name}`;
    }
}

const derived = { 
    __proto__:base , 
    sayHi() { //메서드 축약 표현
        return `${super.sayHi()}. how are you doing?`;
    }
}

console.log(derived.sayHi()); //Hi! Lee. how are you doing?
```

=> 메서드에 본연의 기능 (super) , 맞지 않는 기능 (constructor) 삭제 따라서 ES6 이전의 방식 사용 x

## 화살표 함수

- 화살표 함수는 function 키워드 대신 => 를 사용하여 기존의 함수 정의 방식보다 간단히 함수 정의 가능
- 콜백 함수의 내부 this가 전역 객체를 가리키는 문제 해결을 위한 대안으로 유용

### 화살표 함수 정의
1. 함수 정의
    
     함수 선언문으로 정의할 수 없고 함수 표현식으로 정의 해야 하며 함수 호출 방식은 기존과 동일하다
     ```
     const mul = (x,y) => x*y;
     mul(2,3); //6
     ```

2. 매개변수 선언
    - 매개변수가 여러개인 경우 소괄호 () 안에 선언
    ```
    const arrow = (x,y) => {...};
    ```
    - 매개변수가 한 개인 경우 소괄호 () 생략
    ```
    const arrow = x => {...};
    ``` 
    - 매개변수가 없으면 소괄호 생략 불가
    ```
    const arrow = () => {...};
    ```
3. 함수 몸체 정의
    - 함수 몸체가 하나의 문으로 구성되고 표현식인 경우 {} 생략 가능
    ```
    const power =  x => x**2; 
    power(2); //4
    ```
    - 함수 몸체가 하나의 문으로 구성되고 표현식이아닌 경우 {} 생략 불가 
    ```
    const arrow = () => const x = 1;
     // 암묵적으로 반환할 값이 없기 때문
    ```
    - 객체 리터럴을 반환하는 경우 객체 리터럴을 () 로 감싸줘야함
    ```
    const create = (id , content) => ({id , content}) 
    // 감사주지 않으면 함수 몸체를 감싸는 중괄호로 인식
    ```
    - 함수 몸체가 여러 개의 문으로 구성되면 {} 생략 불가
    - 즉시 함수로 ㅅ용 가능
    ```
        const person = (name => ({
            sayHi() {return `Hi? My name is ${name}.`}
        }))('Lee');
    ```
    - 화살표 함수도 일급 객체이다.
### 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.

2. 인스턴스를 생서할 수 없으므로 prototype 프로퍼티가 없고 프로토 타입도 생성하지 않는다.

3. 중복되 매개 변수 이름을 선언할 수 없다.

4. 화살표 함수는 함수 자체의  this , arguments , super , new.target 바인딩을 갖지 않는다.

    => 참조시 스코프 체인을 통해 상위 스코프를 참조하며 상위 스코프가 화살표 함수인 경우 화살표 함수가 아닌 가장 가까운 함수를 참조

### this

화살표 함수와 일반 함수가 구별되는 가장 큰 특징

콜백 함수 내부의 this가 외부 함수의 this와 다르기 때문에 발생하는 문제를 해결하기 위해 의도적으로 설계

함수를 호출 할 때 함수가 어떻게 호출되었는지에 따라 this에 바인당할 객체가 동적으로 결정

#### 콜백함수 내부 this 문제

```
class Prefixer {
    constructor(prefix) {
        this.prefix = prefix
    }

    add(arr) {
        //add 메서드는 인수로 전달된 arr을 순회하며 배열의 모든 요소애 prefix 추가
        return arr.map(function (item) {
            // 일반함수로 호출되서 this가 전역 객체를 가리키고 class 내여서 undefiend 가 바인딩됨
            return this.prefix + item; 
         })
    }
}

const prefixer = new Prefixer('-webkit-');

console.log(prefixer.add(['transition', 'user-select']));
```

##### ES6 이전의 콜백 함수 내 this 문제를 해결하기 위한 방법들

1. this를 회피 시킨 후 콜백함수에서 사용
    ```
    add(arr) {
        const that = this;
        return arr.map(function (item) {
            return that.prefix + ' ' + item;
        })
    }
    ```

2. Array.prototype.map 두 번째 인수로 add 메서드를 호출한 prefixer 객체를 가리키는 this를 전달한다.
    ```
      add(arr) {
        const that = this;
        return arr.map(function (item) {
            return that.prefix + ' ' + item;
        }, this)
    }
    ```
3. Function.prototype.bind 메서드를 사용하여 add 메서드를 호출한 prefixer 객체를 가리키는 this 바인딩하다.

    ```
      add(arr) {
        const that = this;
        return arr.map(function (item) {
            return that.prefix + ' ' + item;
        }.bind(this)); //this 바인딩된 값이 콜백 함수 내부의 this에 바인딩된다.
    }
    ```

##### ES6 이후의 화살표 함수

```
class Prefixer {
    constructor(prefix) {
        this.prefix = prefix
    }

    add(arr) {
        return arr.map(item => this.prefix + item);
    }
}

const prefixer = new Prefixer('-webkit-');

console.log(prefixer.add(['transition', 'user-select']));
```

 - 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다 . 따라서 화살표 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다
 
    => this가 함수가 정의된 위치에 의해 결정되므로 lexical this라고 한다.

- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 Function.prototype.call , Function.prototype.apply ,Function.prototype.bind 메서드를 사용해도 내부 this를 교체할 수없다.

- 메서드를 화살표 함수로 정의하는 것을 피해야 한다.
 => this가 상위 스코프의 this를 참조하기 때문에 클래스가 생성할 인스턴스를 참조하게 되고 프로토 타입 메서드가 아닌 인스턴스 메서드가 된다.

 ### super

화살표 함수는 함수 자체의 super를 갖지 않으며 상위 스코프의 super를 참조

### arguments

this와 마찬가지로 상위 스코프의 arguments를 참조한다. 

이는 상위 함수의 인수목록이므로 도움이 되지 않으며 화살표 함수로 가변인자 함수를 구현해야 할 땐 Rest 파라미터를 사용

## Rest 파라미터
매개변수이름 앞에 ...을 붙여서 정의한 매개 변수를 의미한다.

Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.

Rest 파라미터는 기본 매개변수와 같이 사용될 수 있지만 항상 마지막 파라미터이어야만 하며 단 하나만 선언 가능하다.

함수의 length 프로퍼티에 영향을 주지 않는다.

## 매개변수 기본값
ES6에서 도입된 매개 변수 기본값으로 함수내에서 수행하던 인수 체크 및 초기화를 간소화 할 수 있따.

```
function sum(x = 0 , y = 0) {
    return x + y ;
}
```

=> 매개변수에 인수를 전달하지 않거나 undefined를 전달한 경우에만 유효한다.

함수 객체의 length 프로퍼티와 arguments 객체에 아무런 영향을 주지 않는다.