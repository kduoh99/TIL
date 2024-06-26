## undefined와 null

<img src="./img/core_javascript.jpg" width="400" height="250"/>

<br/>

자바스크립트에는 '없음'을 나타내는 값으로, undefined와 null 두 가지가 있다. 두 값의 의미는 미세하게 다르고, 사용 목적 또한 다르다.

<br/>

**undefined는 사용자가 명시적으로 지정할 수도 있지만, 값이 존재하지 않을 때 자바스크립트 엔진이 자동으로 부여하는 경우도 있다. 다음 세 경우가 이에 해당한다.**

- 값을 대입하지 않는 변수, 즉 데이터 영역의 메모리 주소를 지정하지 않은 식별자에 접근할 때
- 객체 내부의 존재하지 않는 프로퍼티에 접근하려고 할 때
- return 문이 없거나 호출되지 않는 함수의 실행 결과

<br/>

*'비어있음'을 명시적으로 나타내고 싶을 때는 null을 사용하자. <br/>
이 규칙을 따른다면 undefined는 오직 '값을 대입하지 않은 변수에 접근하고자 할 때, 자바스크립트 엔진이 반환해주는 값'으로서만 존재할 수 있다.*