# 정규표현식

정규 표현식은 검색 패턴을 정의한 문자열들의 집합이다. 자바스크립트는 정규식에 사용할 수 있는 기본 객체 [RegExp](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/RegExp)가 있다.

### RegExp 객체

RegExp 객체는 생성자에서 필수 매개변수인 정규표현식과 선택 매개변수인 일치 관련 설정을 받는다.

{% hint style="info" %}
일치 관련 설정

* i \(case Insensitive\): 대소문자를 구분하지 않는다.
* g \(global\): 일치하는 문자열을 처음 발견한 후 멈추지 않고 모든 문자열을 찾는다.
* m \(multi line\): 여러줄 문자열에 대해서도 검색한다.
{% endhint %}

RegExp에는 다음 두 가지 함수가 있다.

* RegExp.prototype.exec\(\): 문자열 내에 일치하는 문자열을 찾고, 일치하는 첫 번째 문자열을 반환한다.
* RegExp.prototype.test\(\): 문자열 내에 일치하는 문자열을 찾고, true 또는 false를 반환한다.

자바스크립트 String 객체에서 정규 표현식과 관련된 두가지 함수가 있는데 두 함수 모두 RegExp 객체를 인자로 받는다.

* String.prototype.search\(\): 문자열 내에 일치하는 문자열을 찾고, 일치하는 문자열의 인덱스를 반환한다.
* String.prototype.match\(\): 일치하는 문자열을 찾고, 모든 일치하는 문자열을 반환한다.

### 자주 사용하는 정규 표현식

```javascript
// 숫자를 포함하는 문자
var reg = /\d+/;
reg.test("123a"); // true
reg.test("abcd"); // false

// 숫자만 포함하는 문자
var reg = /^\d+$/;
reg.test("123"); // true
reg.test("123a"); // false

// 부동소수점 문자
var reg = /^[0-9]*.[0-9]*[1-9]+$/;
reg.test("12"); // true
reg.test("12.2"); // true

// 숫자와 알파벳만을 포함하는 문자 
var reg = /[a-zA-Z0-9]/;
reg.test("abcdABCD1234"); // true
reg.test("^"); // false

// 쿼리스트링
var uri = "http://www.google.com/test?category=1&id=1234";
var queryString = {};
uri.replace(
    new RegExp("([^?=&]+)(=([^&]*))?", "g"),
    function($0, $1, $2, $3) { queryString[$1] = $3; }
);
console.log('category: ', queryString['category']); // 1
console.log('id: ', queryString['id']); // 1234
```

{% hint style="info" %}
기본 정규 표현식

* ^: 문자열/줄의 시작\(매칭이 처음부터 되어야 한다.\) 
* $: 문자열의 끝\(문자열 끝에 매칭되어야 한다.\) 
* \d: 모든 숫자. \[0-9\]와 같음
* \D: 숫자가 아님. \[^0-9\]와 같음
* \[\]: 문자열 셋
  * \[abc\]: 괄호 내의 모든 문자
  * \[^abc\]: 괄호 내의 문자들을 제외한 모든 문자
  * \[0-9\]: 문자열 범위. 괄호 내의 모든 숫자
* \(x\|y\): x 또는 y
* \*: 0번 이상 반복
* +: 1번 이상 반복
* ?: 0 또는 1회
* {}: 횟수 표시. \[a\]{2}이면 aa이고 \[a\]{2,}이면 a가 2개 이상, \[a\]{2, 4}이면 aa, aaa, aaaa 이다.
* .: 뉴라인\(\n\)제외한 한 문자
{% endhint %}

