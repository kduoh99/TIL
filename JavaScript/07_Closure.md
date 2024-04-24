## 클로저

<img src="./img/book_img.jpg" width="400" height="250"/>

<br/>

클로저는 **외부 함수의 변수를 내부 함수가 참조할 때, 외부 함수의 실행 컨텍스트가 종료되어도 해당 변수가 메모리에서 유지되는 현상**을 말한다.

<br/>

**클로저 개념**

클로저는 다음 조건을 충족할 때 생성된다.

- 외부 함수 내에 내부 함수가 존재해야 한다.
- 내부 함수는 외부 함수의 변수를 참조해야 한다.
- 내부 함수가 외부로 전달되거나, 콜백 함수로서 전달되어야 한다.

    ```javascript
    // 외부 함수의 변수를 참조하는 내부 함수
    var outer = function () {
        var a = 1;
        var inner = function () {
            return ++a;
        };
        return inner;
    };
    var outer2 = outer();
    console.log(outer2());  // 2
    console.log(outer2());  // 3
    ```

    ```javascript
    // setInterval / setTimeout
    (function () {
        var a = 0;
        var intervalId = null;
        var inner = function () {
            if (++a >= 10) {
                clearInterval(intervalId);
            }
            console.log(a);
        };
        intervalId = setInterval(inner, 1000);
    })();
    ```

    *별도의 외부객체인 window의 메서드(setTimeout / setInterval)에 전달할 콜백 함수 내부에서 지역변수를 참조한다.*

    ```javascript
    // eventListener
    (function () {
        var count = 0;
        var button = document.createElement('button');
        button.innerText = 'click';
        button.addEventListener('click', function () {
            console.log(++count, 'times clicked');
        });
        document.body.appendChild(button);
    })();
    ```

    *별도의 외부객체인 DOM의 메서드(addEventListener)에 등록할 handler 함수 내부에서 지역변수를 참조한다.*

<br/>

**메모리 관리**

클로저는 메모리를 계속 차지하는 개념이므로, 더 사용하지 않게 된 클로저에 대해서는 메모리를 차지하지 않도록 관리해줄 필요가 있다.

- 사용하지 않는 클로저의 참조를 제거하여 메모리를 해제해야 한다.
- 필요성이 사라진 시점에는, 클로저가 참조하는 변수에 null을 할당하여 가비지 컬렉션을 유도해야 한다.

    ```javascript
    // return에 의한 클로저의 메모리 해제
    var outer = (function () {
        var a = 1;
        var inner = function () {
            return ++a;
        };
        return inner;
    })();
    console.log(outer());
    console.log(outer());
    outer = null;   // outer 식별자의 inner 함수 참조를 끊음
    ```

    ```javascript
    // setInterval에 의한 클로저의 메모리 해제
    (function () {
        var a = 0;
        var intervalId = null;
        var inner = function () {
            if (++a >= 10) {
                clearInterval(intervalId);
                inner = null;   // inner 식별자의 함수 참조를 끊음
            }
            console.log(a);
        };
        intervalId = setInterval(inner, 1000);
    })();
    ```

    ```javascript
    // eventListener에 의한 클로저의 메모리 해제
    (function () {
        var count = 0;
        var button = document.createElement('button');
        button.innerText = 'click';

        var clickHandler = function () {
            console.log(++count, 'times clicked');
            if (count >= 10) {
                button.removeEventListener('click', clickHandler);
                clickHandler = null;    // clickHandler 식별자의 함수 참조를 끊음
            }
        };
        button.addEventListener('click', clickHandler);
        document.body.appendChild(button);
    })();
    ```

<br/>

**클로저의 활용**

클로저는 데이터 캡슐화 및 정보 은닉, 이벤트 핸들러, 콜백 함수, 상태 유지 등 다양한 곳에서 활용된다.

- 콜백 함수를 중심으로 살펴보자.

    ```javascript
    // 콜백 함수를 내부함수로 선언해서 외부변수를 직접 참조하는 방법
    var fruits = ['apple', 'banana', 'peach'];
    var $ul = document.createElement('ul'); // 공통 코드

    fruits.forEach(function (fruit) {   // 클로저가 없음
        var $li = document.createElement('li');
        $li.innerText = fruit;
        $li.addEventListener('click', function () { // 클로저가 있음
            alert('your choice is ' + fruit);
        });
        $ul.appendChild($li);
    });
    document.body.appendChild($ul);
    ```

    ```javascript
    // bind 활용
    fruits.forEach(function (fruit) {
        var $li = document.createElement('li');
        $li.innerText = fruit;
        $li.addEventListener('click', alertFruit.bind(null, fruit));
        $ul.appendChild($li);
    });
    ```

    ```javascript
    // 콜백 함수를 고차함수로 변경
    var alertFruitBuilder = function (fruit) {
        return function () {
            alert('your choice is ' + fruit);
        };
    };
    fruits.forEach(function (fruit) {
        var $li = document.createElement('li');
        $li.innerText = fruit;
        $li.addEventListener('click', alertFruitBuilder(fruit));
        $ul.appendChild($li);
    });
    ```

<br/>

*클로저는 자바스크립트를 효율적이고 체계적으로 작성할 수 있게 하는 중요한 개념이다.* <br/>
*적절한 사용과 함께 메모리 관리에 주의를 기울이자.*