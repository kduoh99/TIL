## 프로토타입

<img src="./img/core_javascript.jpg" width="400" height="250"/>

<br/>

자바스크립트는 프로토타입 기반 언어로, 클래스 기반 언어와 달리 **상속을 프로토타입을 통해 구현한다. 이를 통해 상속과 비슷한 효과를 얻을 수 있다.**

<br/>

**생성자 함수와 프로토타입**

자바스크립트에서 객체는 생성자 함수를 통해 생성된다. 생성자 함수를 `new` 연산자와 함께 호출하면, 생성자 함수에서 정의된 내용을 바탕으로 새로운 인스턴스가 생성된다. 이때 생성된 인스턴스에는 `__proto__`라는 프로퍼티가 자동으로 부여되며, 이는 생성자 함수의 `prototype` 프로퍼티를 참조한다.

```javascript
// constructor 프로퍼티
var arr = [1, 2];
Array.prototype.constructor === Array // true
arr.__proto__.constructor === Array // true
arr.constructor === Array // true

var arr2 = new arr.constructor(3, 4);
console.log(arr2); // [3, 4]
```

*생성자 함수의 프로퍼티인 `prototype` 객체 내부에는 `constructor`라는 프로퍼티가 있는데, 이는 해당 인스턴스를 생성한 생성자 함수를 참조한다. 이를 통해 인스턴스가 자신의 생성자 함수를 알 수 있다.*

```javascript
// 다음 각 줄은 모두 동일한 대상을 가리킨다.
[Constructor]
[instance].__proto__.constructor
[instance].constructor
Object.getPrototypeOf([instance]).constructor
[Constructor].prototype.constructor

// 다음 각 줄은 모두 동일한 객체(prototype)에 접근할 수 있다.
[Constructor].prototype
[instance].__proto__
Object.getPrototypeOf([instance])
```

<br/>

**프로토타입 체인**

객체의 `__proto__` 프로퍼티가 연쇄적으로 이어진 것을 프로토타입 체인이라고 하며, 이 체인을 따라가며 검색하는 것을 프로토타입 체이닝이라고 한다. 프로토타입 체인은 2단계로만 이루어지는 것이 아니라, 무한대의 단계를 가질 수 있다.

자바스크립트에서 모든 객체의 최상위 프로토타입은 `Object.prototype` 이다.
`Object.prototype` 에는 모든 데이터 타입에서 사용할 수 있는 범용적인 메서드가 포함되어 있다.

```javascript
// 프로토타입 체인 예시
var obj = {};
console.log(obj.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__); // null
```

*`__proto__` 프로퍼티는 학습 목적으로만 이해하고, 실무에서는 가급적 사용하지 않길 권장한다.*
*대신 `Object.getPrototypeOf()`와 `Object.create()` 등을 사용하자.*

- [`__proto__` in ECMAScript 6](https://2ality.com/2015/09/proto-es6.html)