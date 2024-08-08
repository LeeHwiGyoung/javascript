## this 키워드
객체는 =  프로퍼티 + 메서드

메서드 = 자신이 속한 객체의 상태 == 프로퍼티를 참조하고 변경할 수 있어야 함

메서드가 객체의 프로퍼티를 참조하려면 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

#### 객체 리터럴 방식
메서드 내부에서 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할  수 있다.

객체 리터럴 =  circle 변수에 할당되기 직전에 평가

getDiameter 메서드 =  함수가 호출되는 시점

객체 리터럴의 평가가 완료 되었기에 circle 식별자에 객체가 할당된 이후이다. 따라서 참조 할 수 있다.

=> 일반적이지 않으며 바람직하지도 않다.

```
const circle = {
    radius : 5,
    getDiameter() {
        return 2 * circle.radius; //자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조
    }
};

console.log(circle.getDiameter()); // 10

```

#### 생성자 함수
```
function Circle(radius) {
    //이 시점에는 생성자 함수 자신이 생설할 인스턴스를 가리키는 식별자를 알 수 없다. 
    ????.radius = radius;

    Circle.prototype.getDiameter = function (){
        return 2 * ????.radius;
    }
}

const circle = new Circle(5);
```
생성자 함수를 정의하는 시점에는 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다.

생성자 함수 내부에서는 프로퍼티 또는 메서드를 추가하기 위해 자신이 생성할 인스턴스를 참조할 수 있어야 한다.

이를 위해 자바스크립트에서는 `this` 식별자를 제공한다.

#### this
this : 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수 이를 통해 생성할 인스턴스의 method나 property에 접근이 가능하다.

- 자바스크립트 엔진에 의해 암묵적으로 생성된다.
- 함수를 호출하면 암묵적으로 함수 내부에 전달된다.
- 함수 내부에서 this도 지역 변수처럼 사용이 가능하다.
- this가 가리키는 값은 함수 호출 방식에 의해 동적으로 결정된다.

#### this를 사용한 객체 리터럴
```
const circle = { 
    radius : 5,
    getDiameter() {
        return this.radius * 2; //객체 리터럴 메서드 내부의 this는 메서드를 호출한 객체를 가리킨다.
    }
}
```
#### this를 사용한 생성자 함수
```
function Circle(radius) {
    this.radius = radius; //this는 생성자 함수가 생성할 인스턴스를 가리킨다.
}

Cirlce.prototype.getDiamter = function () {
    return 2 * this.radius;
};

const circle = new Circle(5);
console.log(circle.getDiameter()); //10
```

#### 전역 함수 내부의 this

전역에서 this는  전역 객체를 가리킨다.

strict mode에서는 undefined가 바인딩 된다.

```
console.log(this); //window

function square(number){
    console.log(this); //window
    return number * number;
}
```

## 함수 호출 방식과 this 바인딩
#### 일반 함수 호출
 - this = 객체의 프로퍼티나 메서드를 참조하기 위한 자기 변수 => 일반 함수의 this는 의미가 없다 
 - this 바인딩 값은 window이다.
 - 메서드 내부와 콜백함수 등 어던 함수라도 일반 함수로 호출되면 this 에 전역객체가 바인딩된다.


```
function foo() {
    console.log("foo's this : " , this); //window
    function bar() {
        console.log("bar's this : " , this); //window
    }
    bar();
}

var value = 1;

const obj = {
    value : 100,
    foo() {
        console.log("foo's this: ", this); //{value : 100 , foo : f}
    }

    function bar() { //메서드 내의 중첩 함수
        console.log("bar's this: ", this); //window 전역 객체가 바인딩된다
    }

    setTimeout(function () { //콜백함수의 this
        console.log("callback's this : " , this); //window
    } , 100);

    bar();
}
 ``` 

 #### 콜백 함수와 메서드내의 중첩 함수의 this를 바인딩 하는 법
 ```
 var value = 1;

 const obj = {
    value : 100,
    foo() {
        const that = this; // this바인딩을 변수에 할당
    

        setTimeout(function () {
            console.log(that.value) //this가 아닌 this의 값을 참조하는 변수를 참조
        }, 100);
    }
 }


 //bind() 메서드를 통한 this의 명시적 바인딩

 const obj1 = {
    value : 100,
    foo() {
        const that = this; // this바인딩을 변수에 할당

        setTimeout(function () {
            console.log(this.value) //this가 아닌 this의 값을 참조하는 변수를 참조
        }.bind(this), 100);
    }
 }

 //화살표 함수를 사용한 this 바인딩을 일치시키기
 //화살표 함수의 this는  상위스코프의 this를 가리키기 때문에
 const obj2 = {
    value : 100, 
    foo() {
        setTimeout(() => console.log(this.value), 100); //100
    }
 }
 
 ```
#### 메서드 호출
1. 객체리터럴 메서드의 this

    메서드 내부의 this 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다.

    ```
    const person = {
        name : 'Lee',
        getName() {
            return this.name;
        }
    }
    console.log(person.getName()) //Lee

    ```

    getName 메서드는 프로퍼티에 바인딩 된 함수로 person 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체이며 person의 getName 프로퍼티가 함수 객체를 가리키고 있는 것이다.

    => getName 메서드는 다른 객체의 프로퍼티에 할당할 수도 있고 일반 함수로 호출 될 수도 있다.
    ```
    const other = {
        name : "Kim"
    }

    other.getName = person.getName;

    console.log(other.getName); //Kim

    const getName = person.getName;

    console.log(getName()); // '' window.name의 default 값은 ""이다.
    ```
2. 프로토타입 메서드 내부의  this

    메서드를 호출한 객체에 바인딩된다.
    ```
        function Person(name) {
            this.name  = name;
        }

        Person.prototype.getNam = function () {
            return this.name;
        }

        const me = new Person('Lee');

        console.log(me.getName()); //'Lee'

        Person.prototype.name = 'kim';

        console.log(Person.prototype.getName());// 'Kim' 프로토타입도 하나의 객체이므로 메서드를 호출 할 수 있다.
    ```
3. 생성자 함수 호출
    생성자 함수 내부의 this는 미래에 생성될 인스턴스가 바인딩된다.
    ```
        function Circle(radius) {
            this.radius = radius;
        
            this.getDiameter = function () { //메서드를 중복해서 생성하게 된다.
                return 2* this.radius;
            }
        }

        const circle1 = new Circle(1);

        const circle2 = new Circle(10);

        console.log(circle1.getDiamter()); //2 
        console.log(circle1.getDiamter()) //20

    ```
4. Function.prototype.apply/call/bind를 통한 간접 호출
    - Function.prototype.apply(thisArg[ , argsArray]); 
    
        => 유사 배열 객체 또는 배열객체를 인수로 전달
    
    - Function.prototype.call(thisArg[ , arg1[ ,arg2[ ,...]]]);
    
        => 쉼표로 구분한 리스트 형식으로 전달

    - Function.prototype.bind(thisArg) : 인수로 전달한 값으로 this 바인딩

        => 함수를 호출하지는 않음 따로 호출 해줘야한다.

| 함수 호출 방식 | this 바인딩 | 
| --- | --- |
| 일반 함수 호출  | 전역 객체 |
| 메서드 호출 | 메서드를 호출한 객체 |
| 생성자 함수 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 | 
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | 메서드에 첫 번째 인수로 전달한 객체|