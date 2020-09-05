# 숫자 정확도 검증

설계 명세서에 의하면 자바스크립트에서 수는 "**이중정밀도 64비트 형식 IEEE 754 값**"으로 정의된다. 

#### IEEE 754의 부동소수점 방식 

IEEE 754의 부동 소수점 표현은 크게 세 부분으로 구성되는데, 최상위 비트는 부호를 표시하는 데 사용되며, 지수 부분\(exponent\)과 가수 부분\(fraction/mantissa\)이 있다.

\[예시\] −118.625 \(십진법\)을 IEEE 754 \(32비트 단정밀도\)로 표현해 보자. 

* 음수이므로, 부호부는 1이 된다. 
* 그 다음, 절댓값을 이진법으로 나타내면 1110110.101이 된다. 
* 소수점을 왼쪽으로 이동시켜, 왼쪽에는 1만 남게 만든다. 예를 들면 1110110.101=1.110110101×2⁶ 과 같다. 이것을 정규화된 부동소수점 수라고 한다. 
* 가수부는 소수점의 오른쪽 부분으로, 부족한 비트 수 부분만큼 0으로 채워 23비트로 만든다. 결과는 11011010100000000000000이 된다. 
* 지수는 6이므로, Bias를 더해야 한다. 32비트 IEEE 754 형식에서는 Bias는 127이므로 6+127 = 133이 된다. 이진법으로 변환하면 10000101이 된다. 

이 결과를 정리해서 표시하면 다음과 같다.

![](../.gitbook/assets/600px-float_point_example_frac.svg.png)

{% hint style="info" %}
자바스크립트는 이중정밀도 64비트 형식이므로 63번째가 부호 비트, 다음 열한 개의 비트\(62번째부터 52번째 비트\)는 지수 값 e를 나타낸다. 마지막으로 나머지 52비트가 분수값을 나타낸다.
{% endhint %}

## 정확도 문제 해결하기

자바스크립트에는 정수와 같은 것이 존재하지 않고, 모든 숫자를 부동소수점을 사용하기 때문에 산술할 때 주의해야 한다. 부동 소수점으로 표현한 수가 실수를 정확히 표현하지 못하고 부동 소수점 연산 역시 실제 수학적 연산을 정확히 표현하지 못하기 때문에 여러가지 문제를 낳는다. 이 같은 문제를 해결하는 데 도움이 되는 Number 객체의 내장 속성들이 있다.

### Number.EPSILON

Number.EPSILON은 두 개의 표현 가능한 숫자 사이의 가장 작은 간격을 반환한다. 이는 부동소수점 근사치를 활용해 분수가 제대로 표현되지 않는 문제를 해결하는 데 유용하다. ES2015에서 최초 정의되었다.

```javascript
function numberEquals(x, y) {
    return Math.abs(x - y) < Number.EPSILON;
}

numberEquals(0.1 + 0.2, 0.3); // true
```

### 최대치

Number.MAX\_SAFE\_INTEGER는 가장 큰 정수를 반환하고, 부동소수점과 같이 사용하면 제대로 동작하지 않는다. Number.MAX\_VALUE는 가능한 가장 큰 부동소수점을 반환한다. Number.MAX\_VALUE를 사용하자.

```javascript
Number.MAX_SAFE_INTEGER; // 9007199254740991
Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2; //true
Number.MAX_SAFE_INTEGER + 1.111 === Number.MAX_SAFE_INTEGER + 2.022; //false

Number.MAX_VALUE; // 1.7976931348623157e+308
Number.MAX_VALUE + 1 === Number.MAX_VALUE + 2; //true
Number.MAX_VALUE + 1.111 === Number.MAX_VALUE + 2.022; //true
```

### 최소치

Number.MIN\_SAFE\_INTEGER는 가장 작 정수를 반환하고, 부동소수점과 같이 사용하면 제대로 동작하지 않는다. Number.MIN\_VALUE는 가능한 가장 작은 부동소수점을 반환한다. Number.MIN\_VALUE는 0에 가장 가까운 부동소수점이므로 **음수가 아니다**. 따라서 실제로 Number.MIN\_VALUE는 NUMBER\_MIN\_SAFE\_INTEGER보다 크다. 

```javascript
Number.MIN_SAFE_INTEGER; // -9007199254740991;
Number.MIN_SAFE_INTEGER - 1 === Number.MIN_SAFE_INTEGER - 2; //true
Number.MIN_SAFE_INTEGER - 1.111 === Number.MIN_SAFE_INTEGER - 2.022; //false

Number.MIN_VALUE; // 5e-324
Number.MIN_VALUE -1 == -1 // true;
```

### 크기 순서 

자바스크립트 숫자의 크기를 가장 작은 것부터 가장 큰 순서로 나열하면 다음과 같다.

```javascript
-Infinity 
< Number.MIN_SAFE_INTEGER 
< 0 
< Number.MIN_VALUE 
< Number.MAX_SAFE_INTEGER 
< Number.MAX_VALUE 
< Infinity;
```

