### TDZ

```js
let x = 'outer scope';
(function() {
    console.log(x); // reference error
    let x = 'inner scope';
}());
```

참조링크 : http://2ality.com/2015/10/why-tdz.html

### IIFE 패턴

### Lexical scope vs Dynamic scope

- Lexical scope: use environment where function [and variable] is defined

- Dynamic scope: use environment where function [and variable] is called

### const로 선언한 객체 member는 수정이 가능하다.

```js
const a = {
    b: 10
}
a.b = 20;
console.log(a.b); // 20
```

- freeze를 써서 멤버를 얼릴 수도 있지만 멤버의 멤버는 수정을 할 수도 있다.
    - freeze는 개발의 실수를 줄이기 위한 것, 성능상에 문제가 발생할 수 있으므로 production모드로 낼 때는 해제하고

### var 를 재정의 하면 새로운 참조를 할당한다

```js
var a = {
  b: 10
};
var b = a;
var a = {
  b: 10
};
console.log(window.a === b); // false
```

### typeof가 안정적인 것의 의미

```js
typeof let; // 'undefined'
​
'use strict';
typeof let; // Uncaught SyntaxError: Unexpected strict mode reserved word
```

참조링크 : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof#Examples

### let의 반복분 내부에서의 동작은 특별한 명세로 되어있다

```js
for(let i=0; i < 10; i += 1) {
    console.log(i);
}
```

### const, for - in, for - of 수행 가능, for loop error

```js
let obj = {
    a: 1,
    b: 2,
    c: 3
};
for(const a in obj) {
    console.log(obj[a]); // 1, 2, 3
}
​
​
for(const a = 1; a < 10; i += 1) {
    console.log(a); // 1, error
}
```