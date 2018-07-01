# 6장 Symbol과 Symbol 프로퍼티

### 6.1 Symbol 만들기

- symbol은 원시 값 이기 때문에, new Symbol()은 에러가 발생한다.
```JS
let firstName = Symbol();
let lastName = new Symbol(); //ERROR!
let Name = new Object(yourSymbol);
```

- Symbol의 서술 문자열은 `[[Description]]` 프로퍼티에 저장되고 toString() 메서드 호출 시 사용된다.
이 외에 코드에서 `[[Description]]` 에 직접 접근하는 것은 불가능하다.

- Symbol 타입을 확인 하기 위해서는 typeof를 사용한다.

```JS
let symbol = Symbol("test symbol");
console.log(typeof symbol); // "symbol"
```

###6.3 전역 심벌 저장소

- Symbol 을 전역 저장소에서 공유되게 하려면 Symbol() 대신 Symbol.for() 메서드를 사용하면 된다.
- Symbol.for()로 생성하면 전역 심벌 저장소를 검색하고 존재하면 해당값을 반환 존재하지 않으면 새로 등록한다.

```JS
let uid = Symbol.for("uid");
let object = {};

object[uid] = "12345"

console.log(object[uid]); // "12345"
console.log(uid); // "Symbol(uid)"

let uid2 = Symbol.for("uid");

console.log(uid === uid2); // true
console.log(object[uid2]); // "12345"
console.log(uid2) // "Symbol(uid)"
```

- Symbol.keyFor() 로 전역 심벌 저장소에서 Symbol과 관련된 키를 탐색할 수 있다.

```JS
let uid = Symbol.for("uid");
console.log(Symbol.keyFor(uid)); // "uid"

let uid2 = Symbol.for("uid");
console.log(Symbol.keyFor(uid2)); // "uid"

let uid3 = Symbol("uid");
console.log(Symbol.keyFor(uid3)); // undefined
```

- 모든 Symbol 은 논리연산에서 `true` 이다.

###6.6 상용 심벌(Well-known symbols)

- Symbol.hasInstance
- Symbol.isConcatSpreadable
- Symbol.iterator
- Symbol.match
- Symbol.replace
- Symbol.search
- Symbol.species
- Symbol.split
- Symbol.toPrimitive
- Symbol.toStringTag
- Symbol.unscopables

### Symbol의 사용성

Symbol은 Awesome 하다 <br />
반박시 트윗 <br />
https://www.keithcirkel.co.uk/metaprogramming-in-es6-symbols/







