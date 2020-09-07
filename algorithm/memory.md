# 메모리 관리

자바스크립트에서 메모리는 개발자에 의해 할당되지 않지만 메모리 누수 문제를  줄이기 위한 다양한 방법이 있다. 

### 요약

* 객체의 일부 속성을 참조하더라도, 객체 전체가 메모리에 존재한다.
* HTML DOM객체들은 삭제된 이후에는 참조돼서는 안된다.
* 전역 변수 사용을 피하라.
* 함수에서 객체를 참조할 때 필요한 부분만 참조해야 한다.

### 객체 참조

객체에 대한 참조가 있다면 해당 참조는 메모리에 있는 것이다.

```javascript
// memory() 함수는 5kb 데이터와 함께 배열을 반환한다고 가정해보자.
var foo = {
    bar1: memory(), // 5kb
    bar2: memory(), // 5kb
}
function clickEvent() {
    alert(foo.bar1[0]);
}
```

clickEvent 함수가 foo객체의 bar1만 참조하기 때문에 5kb 메모리를 사용할 것이라고 기대할 수도 있지만, 실제로는 10kb의 메모리를 사용한다. bar1 속성에 접근하기 위해서는 foo 전체 객체를 clickEvent 함수의 함수 스코프에 로딩해야하기 때문이다. 함수에서 객체의 전체 범위가 아닌 필요한 속성만 전달하도록 해야 한다. 객체가 차지하는 메모리 공간이 매우 클 수도 있기 때문이다. \(예를 들어 데이터 시각화 프로젝트를 위한 100,000개의 정수로 구성된 배열\)

### DOM 메모리 누수

DOM 항목을 가리키는 변수가 이벤트 콜백 외부에 선언된 경우 해당 DOM 항목을 제거하더라도 해당 항목은 여전히 메모리에 남게 된다.

다음 두 개의 DOM 항목들이 있다고 생각해보자.

```javascript
<div id="one">One</div>
<div id="two">Two</div>
```

다음 자바스크립트 코드는 DOM 메모리 누수를 보여준다.

```javascript
var one = document.getElementById("one");
var two = document.getElementById("two");

one.addEventListener('click', () => {
    // DOM 메모리 누수가 발생한다.
    // two가 제거된 이후에도 one을 클릭하면 여전히 two를 참조하기 때문이다.
    var two = document.getElementById("two");
    two.remove();
});
```

다음과 같은 방법으로 메모리 누수를 해결할 수 있다.

```javascript
var one = document.getElementById("one");

function callbackOne() {
    // 콜백 함수 내부에서 DOM을 참조한다.
    var two = document.getElementById("two");
    two.remove();
    // 이벤트 핸들러를 사용한 뒤 핸들러를 해지한다.
    one.removeEventListener('click', callbackOne);
}

one.addEventListener('click', callbackOne);
```

### windows 전역 객체

객체가 windows 전역 객체에 포함되는 경우 해당 객체는 메모리에 존재하는 것이다. windows의 속성으로 선언된 추가적인 객체는 가비지 컬렉터에 의해 모두 제거할 수 없다. 전역변수를 사용하지 않음으로써 메모리를 절약할 수 있다.

### delete 연산자 활용하기

원치 않는 객체 속성을 제거하기 위해 delete 연산자를 잘 활용하자.

