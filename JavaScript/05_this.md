## this

<img src="./img/book_img.jpg" width="400" height="250"/>

<br/>

자바스크립트에서 this는 실행 컨텍스트에 따라 동적으로 결정된다.<br/>
실행 컨텍스트는 함수가 호출될 때 생성되므로, this는 함수 호출 시에 결정된다고 할 수 있다.<br/>
함수 호출 방식에 따라 this의 값이 달라진다.

<br/>

**명시적 this 바인딩이 없는 경우**

다음의 규칙은 명시적 this 바인딩이 없는 한 늘 성립한다.

- 전역공간에서의 this는 전역객체(브라우저 환경에서는 window, Node.js 환경에서는 global)를 참조한다.

    ```javascript
    console.log(this === window); // 브라우저 환경에서 true
    console.log(this === global); // Node.js 환경에서 true
    ```

<br/>

- 어떤 함수를 메서드로서 호출한 경우, this는 메서드 호출 주체를 참조한다.
- 어떤 함수를 함수로서 호출한 경우, this는 전역객체를 참조한다.

    *어떤 함수를 호출할 때, 그 함수 이름(프로퍼티명) 앞에 객체가 명시되어 있는 경우에는 메서드로 호출한 것이고, 그렇지 않은 모든 경우에는 함수로 호출한 것이다.*

- 콜백 함수 내부에서의 this는 해당 콜백 함수의 제어권을 넘겨받은 함수가 정의한 바에 따르며, 정의하지 않은 경우에는 전역객체를 참조한다.
- 생성자 함수에서의 this는 생성될 인스턴스를 참조한다.

<br/>

**명시적 this 바인딩**

위 규칙에 부합하지 않는 경우, 다음 내용을 바탕으로 this를 예측할 수 있다.

- call, apply 메서드는 this를 명시적으로 지정하면서 함수 또는 메서드를 호출한다.

    ```javascript
    // call 메서드
    Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])

    // apply 메서드
    Function.prototype.apply(thisArg[, argsArray])
    ```

    *call, apply 메서드는 메서드의 호출 주체인 함수를 즉시 실행하도록 하는 명령이며, 기능적으로 완전히 동일하다.*<br/>
    *call 메서드는 첫 번째 인자를 this로 바인딩하고, 이후의 인자들을 호출할 함수의 매개변수로 지정하는 반면, apply 메서드는 두 번째 인자를 배열로 받아 그 배열의 요소들을 호출할 함수의 매개변수로 지정한다.*

<br/>

- bind 메서드는 this 및 함수에 넘길 인수를 일부 지정해서 새로운 함수를 만든다.

    ```javascript
    // bind 메서드
    Function.prototype.bind(thisArg[, arg1[, arg2[, ]]])
    ```

    *bind 메서드는 ES5에서 추가된 기능으로, call과 비슷하지만 즉시 호출하지는 않고 넘겨받은 this 및 인수들을 바탕으로 새로운 함수를 반환하기만 하는 메서드이다. 이는 함수에 this를 미리 적용하는 것과 부분 적용 함수를 구현하는 두 가지 목적을 모두 지닌다.*

<br/>

- 요소를 순회하면서 콜백 함수를 반복 호출하는 내용의 일부 메서드는 별도의 인자로 this를 받기도 한다.
  *이 밖에도 thisArg를 인자로 받는 메서드는 많다.*

  - [Array.prototype.forEach](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
  - [Set.prototype.forEach](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set/forEach)
  - [Map.prototype.forEach](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map/forEach)

<br/>

**화살표 함수의 예외사항**

ES6에 새롭게 도입된 화살표 함수는 실행 컨텍스트 생성 시 this를 바인딩하는 과정이 제외되었다.

- 함수 내부에는 this가 없으며, 접근하고자 하면 스코프체인상 가장 가까운 this에 접근하게 된다.

    ```javascript
    // 화살표 함수 내부에서의 this
    var obj = {
        outer: function () {
            console.log(this);
            var innerFunc = () => {
                console.log(this);
            };
            innerFunc();
        }
    };
    obj.outer();
    ```