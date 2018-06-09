### 3.1 매개변수 기본값

```js
function makeRequest (url, timeout=2000, callback = function() {}) {
    // ES6의 초기화 구문
}
```

https://github.com/cannalee90/TIL/blob/master/js/blog/TLZ.md

### 3.2

- 나머지 매개변수
  - 세개의 점을 사용 (...)
  - 뒤에 전달되는 모든 매개변수

### 3.3 function을 이렇게도 만들 수 있습니다

```js
var add = new Function("first", "second", "return first+second");
console.log(add(1,1)); //2
```

### 3.4 전개 연산자
- 배열을 각각의 인자로 나누어 전달

```js
let values = [25, 50, 75, 100]
console.log(Math.max(...values)); //100
```

### 3.5 name 프로퍼티
- 디버깅을 위해서 name프로퍼티를 모든 함수에 추가했습니다.


## 3.6 함수의 두 가지 용도를 명확히 하기

ES5까지의 함수는 new 없이 호출괴거나 new와 함께 호출되는 두가지 용도로 사용되고 있었다. 이는 혼란을 일으킬 수 있기 때문에, ES6에서는 몇 가지 개선이 이루어졌다.

- 함수를 new 없이 호출할 때는 함수를 실행하는 `[[Call]]` 메서드가 실행
- 함수를 new로 호출할 때 `[[Constructor]]` 메서드를 실행(생성자)

### 3.6.1 ES5에서 함수 호출을 결정하는 요인

ES5에서 함수를 new와 호출했는지 판단하는 가장 일반적인 방법은 instanceof를 사용하는 것이다. 하지만 `call`이나 `apply`를 사용할 경우에는 정상적으로 작동하지 않는다.

```js
function Person(name) {
  if(this instanceof Person) {
    this.name = name;
  } else {
    throw new Error("You must use new with Person");
  }

  var person = new Person("Nicholas");
  var notAPerson = Person.call(person, "Michael");
}
```

이 예제에서는 person객체가 this에 바인딩되기 때문에 생성자로써 호출되었는지 정확히 판단할 수 없다.

### 3.6.2 new.target 메타 프로퍼티

ES6에서는 문제를 해결하기 위해 new.target 메타 프로퍼티가 도입되었다. 메타 프로퍼티 `new.target`에는 `[[Constructor]]`가 실행될 경우 new 연산자의 실행 대상이 할당된다. `[[Call]]`이 실행되면 `new.target`은 `undefined`이다.

```js
function Person(name) {
  if(typeof new.target !== "undefined") {
    this.name = name;
  } else {
    throw new Error("You muse use with Person.!");
  }
}

var person = new Person("Nicholas");
var notAPerson = Person.call(person, "Michael"); //Uncaught Error: You muse use with Person.!
```

```js
function Person(name) {
  if(new.target === Person) { //new.target undefined
    this.name = name;
  } else {
    throw new Error("You muse use with Person.!");    
  }
}

function AnotherPerson(name) {
  Person.call(this, name);
}

var Person = new Person("Nicholas");
var anotherPerson = new AnotherPerson("Nicholas"); //에러 발생
```
`new.target`을 추가하여서 함수 호출과 관련된 모호한 부분을 명확히 한다.

## 3.7 블록 레벨 함수

ES5 strict 모드는 블록 내부에서 함수 선언을 할 때마다 에러를 발생시킨다. ES6에서는 블록 내부에서 함수 선언이 가능하고 최상단으로 호이스팅된다.

```js
'use strict'
if(true) {
  console.log(typeof doSomething); // "function"

  function doSomething() {

  }

  doSomething();
}

console.log(doSomething) // "undefined"
```

### 3.7.1 블록 레벨 함수의 사용 시기

블록 레벨 함수는 let 함수 표현식과 유사하지만, 정의된 블록의 최상단으로 호이스팅 된다는 것이다. let 함수 표현식의 경우는 TDZ가 적용된다.

### 3.7.2 Non-strict 모드의 블록 레벨 함수

ES6에서의 non-strict 모드에서도 블록 레벨 함수를 허용하지만 동작이 약간 다르다. 블록 레벨 함수 선언을 블록의 상단으로 호이스팅하는 대신, 둘러싼 함수나 전역 환경의 최상단까지 호이스팅한다.

```js
if(true) {
  console.log(typeof doSomething); // "function"
  function doSomething() {

  }
  let doSomethingElse = function() {

  }
  doSomething();
}

console.log(typeof doSomething); // "function"
console.log(typeof doSomethingElse) // undefined
```

non-strict 모드에서는 전역 스코프로 호이스팅 되어 if 블록의 바깥 부분에도 존재한다. 하지만 let 함수 표현식은 호이스팅 되지 않는다.

### 3.8 화살표 함수

화살표 함수는 화살표를 사용하는 새로운 문법으로 정의한다.

- `this`, `super`, `arugment`, `new.target`의 값은 그 화살표 함수를 가장 근접하게 둘러 싸고 있는 일반함수에 의해 정의된다.
- `[[Constrcut]]` 메서드가 없으므로 생성자 함수로 사용할 수 없다.
- 프로토타입이 없다.
- 함수 내부에서 `this`를 변경할 수 없다.
- `arguments`가 바인딩 되지 않기 때문에, 명시한 매개변수와 나머지 매개변수에 의존해야 한다.
- 매개변수를 중복하여 사용할 수 없다.
- `name` 프로퍼티를 가진다.

3.8.1 화살표 함수 문법

```js
var reflect = value => value;

// 사실상 같은 코드

var reflect = function(value) {
  return value;
}
```

```js
let sum = (num1, num2) => num1 + num2;

// 사실상 같은 코드

let sum = function(num1, num2) {
  return num1 + num2;
}
```

```js
let getName = () => "Nicholas";

// 사실상 같은 코드

let getName = function() {
  return "Nicholas";
}
```

```js
let sum = (num1, num2) => {
  return num1 + num2;
}

// 사실상 같은 코드

let sum = function(num1, num2) {
  return num1 + num2;
}
```

```js
let doNothing = () => {};

// 사실상 같은 코드

let doNothing = function() {};
```

바깥에 객체 리터럴을 반환하는 화살표 함수는 괄호로 객체 리터럴을 감싸야만 한다.

```js
let getTempItem = id => ({id: id, name: "Temp"});

// 사실상 같은 코드

let getTempItem = function(id) {
  return {
    id: id, 
    name: "Temp",
  }
}
```

### 3.8.2 즉시 실행 함수 표현식 만들기

```js
let person = function(name) {
  return {
    getName: function() {
      return name;
    }
  }
}("Nicholas");

console.log(person.getName());

// 사실상 같은 코드

let person = ((name) => {
  return {
    getName: function() {
      return name;
    }
  }
})("Nicholas");

console.log(person.getName());
```

### 3.8.3 No this 바인딩

`bind()` 메서드를 사용해서 스코프에 명시적으로 this를 바인딩할 수 있습니다. 하지만 ES6에서는 화살표 함수를 사용해서 코드를 효과적으로 작성할 수 있습니다. 화살표 함수는 this 바인딩을 하지 않으므로, 화살표 함수 내 this 값은 스코프 체인을 통해서만 결정된다. 만약 화살표 함수가 일반 함수 안에 포함되는 형태이면 this 값은 화살표 함수를 감싸는 함수에서의 값과 같을 것이고, 그렇지 않으면 this는 전역의 this와 같다.

### 3.8.4 화살표 함수와 배열

```js
var result = values.sort(function(a, b) {
  return a - b;
});

// 사실상 같은 코드

var result = values.sort((a, b) => a - b);
```

### 3.8.5 No arguments 바인딩

화살표 함수는 `aruments` 객체를 가지고 있지 않지만, 둘러싼 함수의 `arguments` 객체에 접근할 수 있다. 그 `arguments` 객체는 나중에 화살표 함수가 실행되는 곳 어디든 이용할 수 있다.

```js
function createArrowFunctionReturningFirstArg() {
  return () => arguments[0];
}

var arrowFunction = createArrowFunctionReturningFirstArg(5);

console.log(arrowFunction()); // 5
```

### 3.8.6 화살표 함수 식별하기

화살표 함수 역시 함수이며 함수로 식별된다.

```js
var comparator = (a, b) => a - b;

console.log(typeof comparator); // "function"
console.log(comparator instanceof Function) // true
```

화살표 함수에도 여전히 `call()`과 `apply()`, `bind()`를 사용할 수 있다.

## 3.9 꼬리 호출 최적화

**꼬리 호출(tail call)** 함수가 다른 함수의 마지막에 호출될 때를 말한다. ES5 엔진에 구현된 꼬리 호출은 일반적인 함수 호출처럼 처리된다. 즉, 함수 호출을 나타내기 위하여 호출 스택에 새로운 스택 프레임을 만들고 추가한다. 그것은 이전의 모든 스택 프레임이 메모리에 유지되고, 호출 스택이 너무 커지면 메모리에 문제가 발생할 수 있다는 의미이다.

### 3.9.1 ES6 꼬리 호출의 차이점

ES6에서는 `strict` 모드에서 특성 꼬리 호출을 위한 호출 스택의 크기를 줄인다. 이러한 최적화로, 다음의 조건을 만족하면 꼬리 호출을 위한 새로운 스택 프레임을 만드는 대신 현재 스택 프레임을 지우고 재사용한다. 현재 V8 엔진에서는 구현되어 있지 않다.

https://stackoverflow.com/questions/42788139/es6-tail-recursion-optimisation-stack-overflow/42788286

https://www.chromestatus.com/feature/5516876633341952

- 꼬리 호출이 현재 스택 프레임의 변수에 접근하지 않는다.(No closure)
- 꼬리 호출을 만드는 함수가 꼬리 호출 반환 후에 남은 작업이 없음
- 꼬리 호출의 결과가 함수의 값으로 반환됨
```
"use strict"
function doSomething() {
  return doSomethingElse(); // O

"use strict"
function doSomething() {
  doSomethingElse();
  return; // X
}

"use strict"
function doSomething() {
  let a = 1;
  return doSomethingElse() + a;
}
```