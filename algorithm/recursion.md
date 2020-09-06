# recursion \(재귀\)

recursion은 자기 자신을 호출하는 함수이다. recursion은 base case\(기저 조건\)와 recursive case\(재귀 조건\)로 구성된다.

```javascript
function func(n) {
	// Base case: 적어도 하나의 recursion에 빠지지 않는 경우가 존재해야 한다.
	if (n<=0) 
		return 0;
	else {
		console.log("Hello");
		// Recursive case: recursion을 반복하다보면 결국 Base case로 수렴해야 한다.
		func(n-1); 
	}
}
```

올바른 base case 가지지 못하면 함수가 무한 호출되어 스택 오버플로를 초래한다.

재귀 알고리즘을 구현할 때 재귀함수 호출의 콜 스택으로 인한 추가적인 메모리를 고려해야 한다. 각 재귀 호출은 base case가 해결될 때까지 운영체제의 메모리 스택에 축적되어 추가적인 공간 복잡도 비용을 지닌다.  콜 스택이 n 호출된 만큼 축적된 경우 해당 함수의 공간복잡도는 O\(n\)이다. 이러한 이유로 인해 재귀보다 반복 루프를 활용한 해결책을 선호하기도 한다.

{% hint style="info" %}
재귀 알고리즘에 대한 빅오 분석은 [**마스터 정리**\(master theorem\)](https://ko.wikipedia.org/wiki/%EB%A7%88%EC%8A%A4%ED%84%B0_%EC%A0%95%EB%A6%AC)라는 개념을 통해서 수행된다. 마스터 정리는 T\(n\) = aT\(n/b\) + O\(n의 c승\)의 형태의 점화식을 필요로 하고, a와 b, c를 식별하여 O\(n³\), O\(nlog\(n\)\), O\(n²\) 세 가지 중 어디에 속하는지 결정한다.
{% endhint %}

## 문제풀이

### 1~n까지의 합

```javascript
function sumOfIntegers(n) {
	// base case: n=0인 경우 0을 반환한다.
	if (n === 0) {
		return 0;
	} else {
		return n + sumOfIntegers(n - 1);
	}
}
```

### Factorial: n!

그 수보다 작거나 같은 모든 양의 정수의 곱이다. n이 하나의 자연수일 때, 1에서 n까지의 모든 자연수의 곱을 n에 상대하여 이르는 말이다.

```javascript
function factorial(n) {
  // base case: n=0인 경우 1을 반환한다. 
	if (n === 0) {
		return 1;
	} else {
		return n * factorial(n - 1);
	}
}
```

### 피보나치 수열

피보나치 수열은 무한한 숫자들의 목록이다. 이 때 각 수는 이 전 두 수의 합이다. \(1부터 시작된다.\)

> 1, 1, 2, 3, 5, 8, 13, 21...

#### 반복 루프를 활용한 해결책

```javascript
function getNthFibo(n) {
	if ( n <= 1 ) return n;
	var sum = 0,
			last = 1,
			lastlast = 0,
	for (var i = 1; i < n; i++) {
		sum = lastlast + last;
		lastlast = last;
		last = sum;		
	}
	return sum;
}
```

#### 재귀: O\(2ⁿ\)

```javascript
function getNthFibo(n) {
	// base case: T(n) = O(1)
	if ( n <= 1 ) {
		return n;
	}
	// recursive case: T(n) = T(n-1) + T(n-2) + O(1)
	// 함수를 호출할 때마다 각 함수 호출에 대해 두 개의 함수 호출이 더 일어난다. 
	return getNthFibo(n-1) + getNthFibo(n-2);
}
```

#### 꼬리 재귀: O\(n\)

```javascript
function getNthFiboBetter(n, lastlast, last) {
	if ( n === 0 ) {
		return lastlast;
	}
	if ( n === 1 ) {
		return last;
	}
	return getNthFiboBetter(n-1, last, lastlast + last);
}
```

getNthFiboBetter의 시간 복잡도는 O\(n\)이다. 재귀 호출이 일어날 때마다 n-1씩 감소하기 때문이다. 공간복잡도 역시 O\(n\)이다.

### 파스칼의 삼각형

파스칼의 삼각형은 어떤 항목의 값이 해당 항목의 위쪽 두 개 항목 값의 합인 삼각형이다.

            1  
          1  1  
         1 2 1  
        1 3 3 1  
       1 4 6 4 1  
   1 5 10 10 5 1

```javascript
function pascalTriangle(row, col) {
	// base case: 최상위 항목(행=1, 열=1)인 1이다.
	// 나머지 모든 수는 해당 항목으로부터 파생된 것이다.
	// 따라서 열이 1이면 1을 반환하고, 행이 0이면 0을 반환한다.
	if (col == 0) {
		return 1;
	} else if (row == 0) {
		return 0;
	} else {
		return pascalTriangle(row - 1, col) + pascalTriangle(row - 1, col - 1);
	}
}
pascalTriangle(5, 2); // 10
```

### 십진수를 이진수로 변환하기

```javascript
function base10ToString(n) {
    var binaryString = "";

    function base10ToStringHelper(n) {
				// base case: n이 2보다 작다는 것은 n이 0 또는 1이라는 의미이다.
        if (n < 2) {
            binaryString += n;
            return;
        } else {
            base10ToStringHelper(Math.floor(n / 2));
            base10ToStringHelper(n % 2);
        }
    }
    base10ToStringHelper(n);

    return binaryString;
}

console.log(base10ToString(232)); // 11101000
```

* 시간 복잡도: O\(log₂\(n\)\)
* 공간 복잡도: O\(log₂\(n\)\)

### 배열의 모든 순열 출력하기

![&#xCD9C;&#xCC98;: https://www.geeksforgeeks.org/write-a-c-program-to-print-all-permutations-of-a-given-string/](../.gitbook/assets/newpermutation.gif)

```javascript
function swap(strArr, index1, index2) {
    var temp = strArr[index1];
    strArr[index1] = strArr[index2];
    strArr[index2] = temp;
}

function permute(strArr, begin, end) {
    if (begin == end) {
        console.log(strArr);
    } else {
        for (var i = begin; i < end + 1; i++) {
            swap(strArr, begin, i);
            permute(strArr, begin + 1, end);
            swap(strArr, begin, i);
        }
    }
}

function permuteArray(strArr) {
    permute(strArr, 0, strArr.length - 1);
}

permuteArray(["A", "B", "C"]);
// ["A", "B", "C"]
// ["A", "C", "B"]
// ["B", "A", "C"]
// ["B", "C", "A"]
// ["C", "B", "A"]
// ["C", "A", "B"]
```

* 시간 복잡도: O\(n!\)
* 공간 복잡도: O\(n!\)

### 객체 펼치기

```javascript
var dictionary = {
    'Key1': '1',
    'Key2': {
        'a' : '2',
        'b' : '3',
        'c' : {
            'd' : '3',
            'e' : '1'
        }
    }
}
function flattenDictionary(dictionary) {
    var flattenedDictionary = {};

    function flattenDitionaryHelper(dictionary, propName) {
				// base case: 객체가 아닐 때
        if (typeof dictionary != 'object') {
            flattenedDictionary[propName] = dictionary;
            return;
        }
        for (var prop in dictionary) {
            if (propName == ''){
                flattenDitionaryHelper(dictionary[prop], propName+prop);
            } else {
                flattenDitionaryHelper(dictionary[prop], propName+'.'+prop);
            }
        }
    }

    flattenDitionaryHelper(dictionary, '');
    return flattenedDictionary;
}
flattenDictionary(dictionary);
// {Key1: "1", Key2.a: "2", Key2.b: "3", Key2.c.d: "3", Key2.c.e: "1"}
```

* 시간 복잡도: O\(n\)
* 공간 복잡도: O\(n\)

### 문자열이 거꾸로 읽어도 동일한지 여부 결정하기

```javascript
function isPalindromeRecursive(word) {
    return isPalindromeHelper(word, 0, word.length - 1);
}

function isPalindromeHelper(word, beginPos, endPos) {
    if (beginPos >= endPos) {
        return true;
    }
    if (word.charAt(beginPos) != word.charAt(endPos)) {
        return false;
    } else {
        return isPalindromeHelper(word, beginPos + 1, endPos - 1);
    }
}

isPalindromeRecursive('hi'); // false
isPalindromeRecursive('iii'); // true
isPalindromeRecursive('ii'); // true
isPalindromeRecursive('aibohphobia'); // true
isPalindromeRecursive('racecar'); // true
```

* 시간 복잡도: O\(n\)
* 공간 복잡도: O\(n\)

