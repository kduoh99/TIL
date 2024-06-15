## 실행 컨텍스트

<img src="./img/core_javascript.jpg" width="400" height="250"/>

<br/>

실행 컨텍스트는 **코드 실행에 필요한 환경 정보를 모아놓은 객체**이다.
코드 실행 시 전역 컨텍스트와 함수 실행에 의한 컨텍스트가 생성되며, 각 컨텍스트는 활성화되는 시점에 VariableEnvironment, LexicalEnvironment, ThisBinding의 세 가지 정보를 수집한다.

<br/>

**VariableEnvironment와 LexicalEnvironment**
   
*초기에는 동일한 내용으로 구성되지만, LexicalEnvironment는 변경 사항이 즉시 반영되는 반면 VariableEnvironment는 초기 상태를 유지한다.*

*매개변수명, 변수의 식별자, 선언한 함수의 함수명 등을 수집하는 environmentRecord와 바로 직전 컨텍스트의 LexicalEnvironment 정보를 참조하는 outerEnvironmentReference로 구성되어 있다.*

<br/>

**자바스크립트는 어떤 실행 컨텍스트가 활성화되는 시점에 선언된 변수를 호이스팅하고, 외부 환경 정보 구성과 this 값을 설정하는 등의 동작을 수행한다.**

*코드 해석을 수월하게 하기 위해 environmentRecord의 수집 과정을 추상화한 개념이다.*

```javascript
// 함수 선언문과 함수 표현식 - 원본 코드
console.log(sum(1, 2));
console.log(multiply(3, 4));

function sum (a, b) {      // 함수 선언문 sum
   return a + b;
}

var multiply = function (a, b) {      // 함수 표현식 multiply
   return a * b;
}
```

```javascript
// 함수 선언문과 함수 표현식 - 호이스팅을 마친 상태
var sum = function sum (a, b) {      // 함수 선언문은 전체를 호이스팅한다.
   return a + b;
};
var multiply;      // 함수 표현식은 변수 선언부만 호이스팅한다.

console.log(sum(1, 2));
console.log(multiply(3, 4));

multiply = function (a, b) {      // 변수 할당부는 원래 자리에 남겨둔다.
   return a * b;
};
```

<br/>

**스코프는 식별자의 유효범위를 나타내며, LexicalEnvironment의 outerEnvironmentReference를 통해 스코프 체인이 형성된다.** 전역 컨텍스트의 LexicalEnvironment에 담긴 변수를 전역변수라 하고, 그 외의 함수 내부에서 생성된 변수는 지역변수이다.

*안전한 코드 구성을 위해 가급적 전역변수의 사용은 최소화하는 것이 좋다.*