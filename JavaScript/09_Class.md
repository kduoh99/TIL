## 클래스

<img src="./img/book_img.jpg" width="400" height="250"/>

<br/>

**클래스 개념**

자바스크립트는 프로토타입 기반 언어이므로, 다른 객체 지향 언어에서 흔히 사용하는 클래스 및 상속 개념이 직접적으로 존재하지 않았다. 그러나 ES6에서 `class` 문법이 도입되면서 클래스와 유사하게 동작하는 기법들이 보다 간편하게 구현될 수 있게 되었다.

- 클래스(Class)
  
  객체를 생성하기 위한 템플릿으로, 객체의 초기 속성 및 메서드를 정의한다.

- 인스턴스(Instance)
  
  클래스로부터 생성된 객체로, 클래스의 구조를 가지고 구체적인 데이터를 담고 있다.

- 상속(Inheritance)
  
  상위 클래스의 속성과 메서드를 하위 클래스가 물려 받는 기능이다. 자바스크립트에서는 프로토타입 체인을 통해 상속을 구현할 수 있다.

- 추상화(Abstraction)
  
  공통된 속성과 동작을 모아 새로운 개념을 만드는 것이다. 자바스크립트에서는 프로토타입을 활용하여 추상화를 구현할 수 있다.

<br/>

**프로토타입 기반 클래스 구현 방법**

ES6 이전에는 프로토타입을 활용하여 클래스와 유사한 구조를 구현했다.

- 프로토타입 메서드
  
  클래스의 `prototype` 내부에 정의된 메서드를 프로토타입 메서드라고 한다. 이 메서드는 인스턴스가 마치 자신의 것처럼 호출할 수 있다.

- 스태틱 메서드
  
  클래스(생성자 함수)에 직접 정의한 메서드를 스태틱 메서드라고 한다. 이 메서드는 인스턴스가 직접 호출할 수 없고, 클래스(생성자 함수)에 의해서만 호출할 수 있다.

  ```javascript
  // 생성자
  var Rectangle = function (width, height) {
    this.width = width;
    this.height = height;
  };
  // (프로토타입) 메서드
  Rectangle.prototype.getArea = function () {
    return this.width * this.height;
  };
  // 스태틱 메서드
  Rectangle.isRectangle = function (instance) {
    return instance instanceof Rectangle &&
        instance.width > 0 && instance.height > 0;
  };

  var rect1 = new Rectangle(3, 4)
  console.log(rect1.getArea()); // 12
  console.log(rect1.isRectangle(rect1)); // Error
  console.log(Rectangle.isRectangle(rect1)); // true
  ```

<br/>

**클래스 상속 및 추상화 방법**

- 인스턴스 생성 후 프로퍼티 제거
  
  상위 클래스의 인스턴스를 생성한 후 불필요한 프로퍼티를 제거하는 방식으로 상속을 구현할 수 있다.

  ```javascript
  var extendClass1 = function (SuperClass, SubClass, subMethods) {
    SubClass.prototype = new SuperClass();
    for (var prop in SubClass.prototype) {
        if (SubClass.prototype.hasOwnProperty(prop)) {
            delete SubClass.prototype[prop];
        }
    }
    SubClass.prototype.constructor = SubClass;
    if (subMethods) {
        for (var method in subMethods) {
            SubClass.prototype[method] = subMethods[method];
        }
    }
    Object.freeze(SubClass.prototype);
    return SubClass;
  };
  ```

- 빈 함수(Bridge)를 활용
  
  상위 클래스의 `prototype`을 상속받는 빈 함수를 만들어 활용하는 방식으로 상속을 구현할 수 있다.

  ```javascript
  var extendClass2 = (function () {
    var Bridge = function () {};
    return function (SuperClass, SubClass, subMethods) {
        Bridge.prototype = SuperClass.prototype;
        SubClass.prototype = new Bridge();
        SubClass.prototype.constructor = SubClass;
        if (subMethods) {
            for (var method in subMethods) {
                SubClass.prototype[method] = subMethods[method];
            }
        }
        Object.freeze(SubClass.prototype);
        return SubClass;
    };
  })();
  ```

- Object.create를 활용
  
  Object.create 메서드를 사용하여 상위 클래스의 `prototype`을 상속받는 방식으로 상속을 구현할 수 있다.

  ```javascript
  var extendClass3 = function (SuperClass, SubClass, subMethods) {
    SubClass.prototype = Object.create(SuperClass.prototype);
    SubClass.prototype.constructor = SubClass;
    if (subMethods) {
        for (var method in subMethods) {
            SubClass.prototype[method] = subMethods[method];
        }
    }
    Object.freeze(SubClass.prototype);
    return SubClass;
  }
  ```

*세 가지 방법 모두 constructor 프로퍼티가 원래의 생성자 함수를 바라보도록 조정해야 한다.*

<br/>

**ES6의 클래스 상속**

ES6에서는 `class` 키워드를 도입하여 더욱 간결하게 클래스와 상속을 구현할 수 있게 되었다.

```javascript
var Rectangle = class {
    constructor (width, height) {
        this.width = width;
        this.height = height;
    }
    getArea () {
        return this.width * this.height;
    }
};
var Square = class extends Rectangle {
    constructor (width) {
        super(width, width);
    }
    getArea () {
        console.log('size : ', super.getArea());
    }
};
```