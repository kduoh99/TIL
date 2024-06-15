## 불변 객체

<img src="./img/core_javascript.jpg" width="400" height="250"/>

<br/>

얕은 복사는 바로 아래 단계의 값만 복사하는 방법이고, 깊은 복사는 내부의 모든 값들을 복사하는 방법이다.

<br/>

```javascript
// 기존 정보를 복사해서 새로운 객체를 반환하는 함수 (얕은 복사)
var copyObject = function (target) {
   var result = {};
   for (var prop in target) {
      result[prop] = target[prop];
   }
   return result;
};


// 중천된 객체에 대한 얕은 복사
var user = {
   name: "K",
   urls: {
      protfolio: 'http://github.com',
      blog: 'http://blog.com',
   }
};

var user2 = copyObject(user);

user.name = 'J';
console.log(user.name === user2.name);      // false

user.urls.portfolio = 'http://portfolio.com';
console.log(user.urls.portfolio === user2.urls.portfolio);      // true

user2.urls.blog = '';
console.log(user.urls.blog === user2.urls.blog);      // true
```

<br/>

*중첩된 객체에서 참조형 데이터가 저장된 프로퍼티를 복사할 때 그 주솟값만 복사하므로, 해당 프로퍼티에 대해 원본과 사본이 모두 동일한 참조형 데이터의 주소를 가리키게 된다.*

<br/>

```javascript
// 중첩된 객체에 대한 깊은 복사
var user2 = copyObject(user);
user2.urls = copyObject(user.urls);

user.urls.portfolio = 'http://portfolio.com';
console.log(user.urls.portfolio === user2.urls.portfolio);      // false

user2.urls.blog = '';
console.log(user.urls.blog === user2.urls.blog);      // false
```

<br/>

*객체의 프로퍼티 중에서 그 값이 기본형 데이터일 경우에는 그대로 복사하면 되지만, 참조형 데이터는 다시 그 내부의 프로퍼티들을 복사해야 한다.*

<br/>

```javascript
// 객체의 깊은 복사를 수행하는 범용 함수
var copyObjectDeep = function(target) {
   var result = {};
   if (typeof target === 'object' && target !== null) {
      for (var prop in target) {
         result[prop] = copyObjectDeep(target[prop]);
         }
   } else {
      result = target;
   }
   return result;
};
```

<br/>

*위의 함수는 참조형 데이터가 있을 때마다, 재귀적으로 수행한다. 이는 원본과 사본이 서로 완전히 다른 객체를 참조하게 되어, 어느 쪽의 프로퍼티를 변경하더라도 다른 쪽에 영향을 주지 않는다.*