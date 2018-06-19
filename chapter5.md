# 5장 구조분해

### 5.2 객체 구조분해

구조분해는 초기화가 필수

```JS
var {type, name};    // ERROR!
let {type, name};    // ERROR!
const {type, name};  // ERROR!
```

구조분해 할당문은 괄호로 감싸야함

```JS
({type, name} = node);
```

구조분해 표현식의 오른쪽이 null, undefined 나오면 error

```JS
let node = null;

const {type, name} = node; // ERROR!
```

Option의 사용성

각 언어의 lint에 따라 달라짐