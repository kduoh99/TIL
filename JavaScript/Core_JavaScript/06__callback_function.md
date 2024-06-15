## 콜백 함수

<img src="./img/core_javascript.jpg" width="400" height="250"/>

<br/>

콜백 함수는 **다른 코드(함수 또는 메서드)에게 인자로 넘겨줌으로써 그 제어권도 함께 위임한 함수**이다. 이를 통해 비동기 작업, 이벤트 처리, 배열의 반복 작업 등에서 유연한 프로그래밍이 가능해진다.

<br/>

**제어권 위임**

- 호출 시점
  
  콜백 함수는 제어권을 넘겨받은 코드에 의해 호출 시점이 결정된다.<br/>
  예를 들어, setInterval 함수는 지정된 시간 간격마다 콜백 함수를 실행한다.

    ```javascript
    var count = 0;
    var cbFunc = function () {
        console.log(count);
        if (++count > 4) clearInterval(timer);
    };
    var timer = setInterval(cbFunc, 300);   // 0.3초마다 cbFunc 함수를 반복 실행
    ```

<br/>

- 인자 전달
  
  콜백 함수를 호출할 때 전달되는 인자의 값과 순서는 제어권을 가진 코드에 의해 결정된다.<br/>
  예를 들어, Array.prototype.map 메서드는 콜백 함수에 현재 요소, 인덱스, 배열 자체를 인자로 전달한다.

    ```javascript
    var newArr = [10, 20, 30].map(function (currentValue, index) {
        console.log(currentValue, index);
        return currentValue + 5;
    });
    console.log(newArr);
    ```

    ```javascript
    // Array의 prototype에 담긴 map 메서드는 다음과 같은 구조로 이루어져 있다.
    Array.prototype.map(callback[, thisArg])
    callback: function(currentValue, index, array)
    ```

<br/>

- this 바인딩
  
  콜백 함수 내부에서의 this는 기본적으로 전역 객체를 바라보지만, bind 메서드를 통해 명시적으로 바인딩할 수 있다.

    ```javascript
    // 콜백 함수 내부의 this에 다른 값을 바인딩하는 방법 - bind 메서드 활용
    var obj1 = {
        name: 'obj1',
        func: function () {
            console.log(this.name);
        }
    };
    setTimeout(obj1.func.bind(obj1), 1000);

    var obj2 = { name: 'obj2' };
    setTimeout(obj1.func.bind(obj2), 1500);
    ```

<br/>

- 비동기 제어
  
  복잡한 비동기 로직을 처리하기 위해 중첩된 콜백 함수를 사용하면, 코드의 가독성과 유지보수성이 저하된다.<br/>
  Promise, Generator, async/await 등 콜백 지옥에서 벗어날 수 있는 방법들이 등장하고 있다.

    ```javascript
    // 콜백 지옥 예시
    setTimeout(function (name) {
        var coffeeList = name;
        console.log(coffeeList);

        setTimeout(function (name) {
            coffeeList += ', ' + name;
            console.log(coffeeList);

            setTimeout(function (name) {
                coffeeList += ', ' + name;
                console.log(coffeeList);

                setTimeout(function (name) {
                    coffeeList += ', ' + name;
                    console.log(coffeeList);
                }, 500, '카페라떼');
            }, 500, '카페모카');
        }, 500, '아메리카노');
    }, 500, '에스프레소');
    ```

    <br/>

    *Promise와 async/await를 사용하여 비동기 작업을 동기적으로 표현한다. 이 방식을 사용하면 코드의 가독성과 유지보수성이 크게 향상된다.*

    ```javascript
    // Promise를 반환하는 비동기 함수 정의
    var addCoffee = function (name) {
        return new Promise(function (resolve) {
            setTimeout(function () {
                resolve(name);  // 비동기 작업 완료 후 결과 반환
            }, 500);
        });
    };

    // async 함수에서 비동기 작업을 동기적으로 처리
    var coffeeMaker = async function () {
        var coffeeList = '';
        var _addCoffee = async function (name) {
            coffeeList += (coffeeList ? ', ' : '') + await addCoffee(name); // await으로 비동기 작업 완료 대기
        };
        await _addCoffee('에스프레소'); // 각 비동기 작업을 순차적으로 실행
        console.log(coffeeList);
        await _addCoffee('아메리카노');
        console.log(coffeeList);
        await _addCoffee('카페모카');
        console.log(coffeeList);
        await _addCoffee('카페라떼');
        console.log(coffeeList);
    };
    coffeeMaker();  // 함수 실행
    ```