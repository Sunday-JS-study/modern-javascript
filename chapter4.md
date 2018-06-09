# 4장 확장된 객체 기능

### 4.1 객체 카테고리

ES6 명세에서는 표준 객체를 설명하기 위해 각 카테고리를 명확하게 정의한다.

- ordinary objects(일반객체): 자바스크립트 객체의 모든 기본 내부 동작을 가진다.
- exotic objects(이형 객체): 기본과 다른 내부 동작을 가진다
- standard objects(표준 객체): ES6에 정의되었으며, **Array**, **Date** 등이 있다. 표준 객체는 일반 객체이거나 이형 객체일 수 있다.
- built-in objects(내장 객체): 스크립트가 실행되는 자바스크립트 실행 환경에 존재한다. 모든 표준 객체는 내장 객체이다.

### 4.2 객체 리터럴의 문법 확장

ES6에서는 자바스크립트에서 가장 인기 잇는 개체 생성방식인 객체 리터럴을 좀 더 강력하고 간결하게 만들었다.

#### 4.2.1 프로퍼티 초기자 축약

ES6에서는 프로퍼티 초기자 축약 문법(property initializer)을 이용하여 프로퍼티 이름과 지역 변수의 중복을 제거할 수 있다.

```js
// ES5
function createPerson(name, age) {
  return {
    name: name,
    age: age,
  }
}

// ES6
function createPerson(name, age) {
  return {
    name,
    age
  };
}
```

#### 4.2.2 간결한 메서드(concise methods)

ES6에서는 객체 리터럴에서 메서드 할당을 위한 문법이 개선되었다. 

```js
var person = {
  name: "Nicholas",
  sayName: function() {
    console.log(this.name);
  }
}

var person = {
  name: "Nicholas",
  sayName() {
    console.log(this.name);
  }
}
```

`sayName()`에는 익명 함수 표현식이 할당되고 ES5에서의 `sayName()` 함수와 같은 특성을 가진다. 한가지 다른점은 간결한 메서드는 `super`를 사용할 수 있다.

```js
let obj1 = {
    name: "kangho",
    sayName(){
        console.log(this); // {name: kangho}
    }
}

let obj2 = {
    name: "kangho",
    sayName: () => {
        console.log(this); // undefined
    }
}
```

아래의 경우 `obj2.sayName()`의 함수에서 this는 undefined이다

#### 4.2.3 계산된 프로퍼티 이름

ES6에서는 대괄표 표기법을 사용하면서 계산된 프로퍼티 이름을 사용할 수 있다.

```js
let lastName = "last Name";
let person = {
  "first name": "Nicholas",
  [lastName]: "Zakas",
  ["first" + "last"]: "N Z",
};
```

### 4.3 새로운 객체 관련 메서드

#### 4.3.1 Object.is() 메서드

자바스크립트 엔진은 +0과 -0 값을 다르게 표현하지만 ===는 같은 것으로 간주한다. 또한, NaN === NaN 반환값은 false이며 NaN를 제대로 찾기 위해서는  isNaN을 사용해야 했다. 이러한 일치 연산자를 보완하기 위해서 `Object.is()` 메서드를 만들었다.

```js
console.log(+0 === 0) // true
console.log(+0 === -0) // true
console.log(Object.is(+0, -0)) // false

console.log(NaN == NaN) // false
console.log(NaN === NaN) // false
console.log(Object.is(NaN, NaN)) // true

console.log(5 == 5) // true
console.log(5 == "5") // true
console.log(5 === 5) // true
console.log(5 === "5") // fasle
console.log(Object.is(5, 5)) //true
console.log(Object.is(5, "5")) // false
```

#### 4.3.2 Object.assign() 메서드

ES5에서 쓰였던 믹스인 메서드는 다음과 같다

```js
//ES5
function mixin(receiver, supplier) {
  Object.keys(supplier).forEach(function(key) {
    receiver[key] = supplier[key];
  });

  return receiver;
}

//ES6
Object.assign(receiver, supplier, supplier1, supplier2)
```

다양한 라이브러리에서 위와 같은 mixin 패턴을 사용하엿다. 하지만 다른 메서드 이름을 사용했는데, 대체적으로 `extends()`,`mix()`와 같은 이름을 사용하였다. 

공급자가 접근자 프로퍼티를 가질때 `Object.assign()` 메서드는 수신자에 접근자 프로퍼티를 생성하지 않는다. `Object.assing()` 메서드는 할당 연산자를 사용하기 때문에 공급자의 접근자 프로퍼티는 수신자의 데이터 프로퍼티가 된다.

```js
var recevier = {};
var supplier = {
  get name() {
    return "file.js";
  }
};

Object.assign(receiver, supplier);

var descriptor = Object.getOwnPropertyDescriptor(receiver, "name");

console.log(descriptor.value) // "file.js"
console.log(descriptor.get) // undefined
console.log(receiver.name) // "file.js"
```

### 4.4 객체 리터럴 프로퍼티의 중복

ES5의 strict 모드는 객체 리터럴 프로퍼티를 검사하여 중복을 발견하면 에러를 발생시킨다

ES6에서는 strict 모드에서는 에러가 발생하지 않는다.

### 4.5 객체 내 프로퍼티 열거 순서

ES5에서는 객체 프로퍼티의 열거 순서를 정의하지 않았고, 자바스크립트 엔진 벤더들에게 맡겼다. 하지만 ES6에서는 객체의 프로퍼티를 열거할 때 반환하는 순서를 엄격하게 정의한다. 객체의 열거 순서는 `Object.getOwnPropertyNames()`와 `Reflect.ownKeys`, `Object.assign()`으로 처리하는 순서에도 영향을 준다.

기본적인 순서는 다음과 같다

- 숫자키는 오름차순
- 모든 문자열 키는 객체에 추가된 순서
- 모든 심벌 키는 객체의 추가된 순서

```js
obj.d = 1;

var obj = {
  a: 1,
  0: 1,
  c: 1,
  2: 1,
  b: 1,
  1: 1,
}

console.log(Object.getOwnPropertyNames(obj).join("")); //012abcd
```

### 4.6 프로토타입 개선

#### 4.6.1 객체의 프로토 타입 

일반적으로 객체의 프로토타입은 `Object.create()`나 생성자를 통해 객체를 생성할 때 지정된다. ES6에서는 주어진 객체의 프로토타입을 변경할 수 있는 `Object.setProrotypeOf()` 메서드를 추가하여 객체 프로토타입이 인스턴스화 된 후 변화지 않을 것이라는 가정을 변경했다.

```js
let person = {
  getGreeting() {
    return "hello";
  }
}

let dog = {
  getGreeting() {
    return "Woof";
  }
}

let friend = Object.create(person);
console.log(friend.getGreeting()); //Hello"
console.log(Object.getPrototypeOf(friend) === person) // true;

Object.setPrototypeOf(friend, dog);
console.log(friend.getGreeting()); //"woof"
console.log(Object.getPrototypeOf(friend) === dog) //true
```
`Object.getPrototypeOf()` 메서드는 [[Prototype]]에 저장된 값을 반환 한다. `Object.setPrototypeOf()` 메서드는 [[Prototype]]에 저장된 값을 변경한다.
 
person과 dog은 instanceof 연산자의 오른쪽에 올 수 없다

#### 4.6.2 Super 참조를 통한 쉬운 프로토타입 접근

```js
let person = {
  getGreeting() {
    return "hello";
  }
}

let dog = {
  getGreeting() {
    return "Woof";
  }
}

let friend = {
  getGreeting() {
    return Object.getPrototypeOf(this).getGreeting.call(this) + ", hi!";
  }
}
console.log(friend.getGreeting()); //Hello, hi"
console.log(Object.getPrototypeOf(friend) === person) // true;

Object.setPrototypeOf(friend, dog);
console.log(friend.getGreeting()); //"woof, hi"
console.log(Object.getPrototypeOf(friend) === dog) //true
```

위의 friend 객체는 다음과 같이 다시 쓸 수 있다.

```js
let friend = {
  getGreeting() {
    return super.getGreeting() + ", hi!";
  }
}
```

여러 단계에 걸쳐 상속하는 경우 `Object.getPrototypeOf()`가 모든 상황에서 정확히 동작하지 않기 때문에 `super` 참조는 유용하다.

```js
let person = {
  getGreeting() {
    return "hello";
  }
}

let friend = {
  getGreeting() {
    return Object.getPrototypeOf(this).getGreeting.call(this) + ", hi!";
  }
}

Object.setPrototypeOf(friend, person);
let relative = Object.create(friend);

console.log(person.getGreeting()); // "hello"
console.log(friend.getGreeting()); // "hello, hi!"
console.log(relative.getGreeting()); // 에러!
```

this가 relative이고 relative의 프로토타입은 friend 객체이기 때문에, this가 relative인 상태로 friend.getGreeting().call()이 호출되면 재귀적으로 반복 호출된다. 이는 super를 사용하면 쉽게 해결할 수 있다.

```js
let person = {
  getGreeting() {
    return "hello";
  }
}

let friend = {
  getGreeting() {
    return super.getGreeting() + ", hi!";
  }
}

Object.setPrototypeOf(friend, person);
let relative = Object.create(friend);

console.log(person.getGreeting()); // "hello"
console.log(friend.getGreeting()); // "hello, hi!"
console.log(relative.getGreeting()); 
```

super 참조는 동적이지 않기 때문에, 항상 올바른 객체를 참조한다. 이 경우, person.getGreeting()을 참조한다.

ES6이전에 메서드의 개념은 공식적으로 정의되지 않았다. 단지 데이터 대신에 함수를 가진 객체 프로퍼티였다. 하지만 ES6에서는 메서드가 속한 객체를 내부 [[HomeObject]] 프로퍼티가 가진 함수가 메서드라고 공식적으로 정의한다.

```js
let person = {
  // 메서드 [[HomeObject]]가 person
  getGreeting() {
    return "hello";
  }
}

// 메서드가 아님 [[HomeObject]]가 없다
function shareGreeting() {
  return "Hi!";
}

```