## 내부 슬롯과 내부 메서드
[[...]] 이중 대괄호로 감싼 이름들이 내부 슬롯과 내부 메서드이며 내부 슬롯과 내부 메서드는 자바스크립트 엔진 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드 이다.

자바스크립트 엔진에서 실제로 동작하지만 직접 접근할 수 있도록 외부로 공개된 객체의 프로퍼티는 아니다.

## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체
자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본 값으로 자동 정의한다.
프로퍼티는 데이터 프로퍼티와 접근자 프로퍼티로 구분 할 수 있다.
### 데이터 프로퍼티
   키와 값으로 구성된 일반적인 프로퍼티이며 다음과 같은 프로퍼티 어트리뷰트를 갖는다.
   | 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 |  설명 |
   | --- | --- | --- |
   | [[Value]] | value |  프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값이다. <br/> 프로퍼티 키를 통해 프로퍼티 값을 변경하면 [[Value]] 에 값을 재할당한다. <br/> 이 때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장한다.|
   | [[Writable]] | writable | 프로퍼티 값의변경 가능 여부를 나타내며 불리언 값을 갖는다. <br /> [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값은 읽기 전용 프로퍼티가 된다.|
   | [[Enumerable]] | enumerable | 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다 <br /> false 인 경우 해당 프로퍼티는for...in 문이나 object.keys 메서드 등으로 열거 할 수 없다.|
   | [[Configurable]] | configurable | 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다 <br /> false 인 해당 프로퍼티의 삭제 , 프로퍼티 어트리뷰트 값의 변경이 금지 된다. <br/> 단 [[Writable]]의 값이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경은 가능하다 |   

```
const person = {
   name : 'Lee',
   age  : 20
};

console.log(Object.getOwnPropertyDescriptor(person , 'name'); // 매개 변수 : 객체의 참조 , 프로퍼티의 키
// { value : "Lee" , writable : true , enumerable : true , configurable : true}
console.log(Object.getOwnPropertyDescriptors(person); //객체의 참조
/*
name : { value : "Lee" , writable : true , enumerable : true , configurable : true}
age : { value : 20 , writable : true , enumerable : true , configurable : true}
*/
```

### 접근자 프로퍼티
   자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티이다.
   접근자 함수는 getter/setter 함수라고 부르며  getter 와 setter함수 모두를 정의 할 수도 있고 하나만 정의 할 수도 있다.
   
   | 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 |  설명 |
   | --- | --- | --- |
   | [[Get]]| get |  접근자 프로퍼티 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수이다. <br/> [[Get]]의 값, getter 함수가 호출되고 결과가 프로퍼티 값으로 반환된다.|
   | [[Set]] | set | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수이다. <br /> [[Set]]의 값 , 즉 Setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다.|
   | [[Enumerable]] | enumerable | 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다 <br /> false 인 경우 해당 프로퍼티는for...in 문이나 object.keys 메서드 등으로 열거 할 수 없다.|
   | [[Configurable]] | configurable | 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다 <br /> false 인 해당 프로퍼티의 삭제 , 프로퍼티 어트리뷰트 값의 변경이 금지 된다. |


```
const person = {
  firstName : 'HwiGyoung',
  lastName : 'Lee',

  get fullName() { //getter 함수
    return `${this.firstName} ${this.lastName} `; 
  }

  set fullName() { //setter 함수
    [this.firstName , this.lastName ] = name.split(' ');
  }
};

console.log(person.firstName + ' ' + person.lastName); //데이터 프로퍼티를 통한 프로퍼티 값의 참조 'HwiGyoung Lee'

person.fullName('HwiGyoung2 Lee'); //접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.

console.log(person.fullName); //getter 함수 호출 'HwiGyoung2 Lee'

console.log(Object.getOwnPropertyDescriptor(person , 'fullName'); //{get : f , set : f , enumerable : true , configurable : true } 
```
메서드 앞에 get, set 키워드가 붙으면 getter , setter 함수가 되며 getter/setter 함수의 이름 fullName이 접근자 프로퍼티가 된다.

#### 접근자 프로퍼티의 동작 (get)
1. 프로퍼티의 키가 유효한지 확인한다. 'fullName'은 문자열로 유효한 프로퍼티의 키이다.
2. 프로토타입 체인에서 프로퍼티를 검색한다. person 객체에 fullName 프로퍼티가 존재한다.
3. fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다.
4. 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값 , getter 함수를 호출하여 그 결과를 반환 한다.

## 프로퍼티의 정의
프로퍼트의 정의란 새로운 프로퍼티를 추가하면서 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼트의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.

Object.defineProperty 메서드를 사용하여 프로퍼티의 어트리뷰트를 정의할 수 있다. 인수로는 객체의 참조, 데이터 프로퍼티의 키인 문자열 , 프로퍼티 디스크립터 객체를 전달한다.

Object.defineProperties 메서드를 통해 여러 개의 프로퍼티를 한 번에 정의 할 수 있으며 두 메서드 모두 프로퍼티 디스크립터 객체에서 생략된 값은 다음과 같다.

| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을 때의 기본값 |
| --- | ---| --- |
| value | [[Value]] | undefined | 
| get | [[Get]] | undefined |
| set | [[Set]] | undefined | 
| writable | [[Writable]] | false |
| enumerable | [[Enumerable]] | false | 
| configurable | [[Configurable]] | false | 

## 객체 변경 방지
객체는 변경이 가능한 값으로 재할당 없이 직접 변경이 가능하다.
즉, 프로퍼티를 추가하거나 삭제할 수 있고 , 프로퍼티의 값을 갱신할수 있으며 , 프로퍼티 어트리뷰트도 재정의가 가능하다.
자바스크립트에서는 객체의 변경을 방지하는 다양한 메서드를 제공한다.
| 구분 | 메서드 | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| --- | --- | --- | --- | --- | --- | --- |
| 객체 확장 금지 | Object.preventExtensions | X | O | O | O | O |
| 객체 밀봉 | Object.seal | X | X | O | O | X |
| 객체 동결 | Object.freeze | X| X| O | X | X |

단 , 이는 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지 못한다.

## 불변 객체
객체의 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 객체

값을 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 한다.
```
function deepFreeze(target) {
  if(target && typeof target === 'object' && !Object.isFrozen(target)) { //target이 존재하며 target이 객체이고 동결되지 않은 객체이면
    Object.freeze(target); // target을 동결
    Object.keys(target).forEach(key => deepFreeze(target[key])); //target의 프로퍼티 key에 대하여 재귀적으로 deepFreeze 함수를 호출
  }
  return target;
}
```

