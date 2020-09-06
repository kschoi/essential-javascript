# 집합

프로그래밍에서 집합\(set\)은 정렬되지 않은 유일한 항목을 나타내는 근간이 되는 자료 구조다. 집합은 O\(1\) 상수 시간에 유일한 항목을 확인하고 추가할 수 있기 때문에 강력하다.

자바스크립트에서는 [Set](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set) 객체가 기본 지원되는데, 자료형에 관계 없이 원시값과 객체 참조 모두 유일한 값을 저장할 수 있다. 기본 Set 객체에는 속성이 하나만 존재하는데 집합 내 항목들의 개수를 나타내는 size라는 정수 속성이다.

```javascript
const set1 = new Set([1, 2, 3, 4, 5]);

set1.size; // 5

// 삽입: 중복되는 내용의 삽입을 허용하지 않는다.
set1.add(6); // set1: Set {6}
set1.add(6); // set1: Set {6}

// 삭제: 불리언을 반환한다. (존재해서 삭제했다면 true, 존재하지 않으면 false)
set1.delete(6); // true

// 포함: 유일한 값을 보장하기 때문에 O(1)의 시간으로 항목의 집합 내 존재 여부를 확인할 수 있다.
set1.has(1); // true
```

## 문제풀이

### 교집합\(A∩B\)

```javascript
function intersectSets (setA, setB) {
    var intersection = new Set();
    for (var elem of setB) {
        if (setA.has(elem)) {
            intersection.add(elem);
        }
    }
    return intersection;
}
var setA = new Set([1, 2, 3, 4]),
    setB = new Set([2, 3]);
intersectSets(setA, setB); // Set {2, 3}
```

### 상위 집합 여부 확인

```javascript
function isSuperset (setA, subset) {
    for (var elem of subset) {
        if (!setA.has(elem)) {
            return false;
        }
    }
    return true;
}

var setA = new Set([1, 2, 3, 4]),
    setB = new Set([2, 3]),
    setC = new Set([5]);
    
isSuperset(setA, setB); // true
isSuperset(setA, setC); // false
```

### 합집합\(A∪B\)

```javascript
function unionSet(setA, setB) {
    var union = new Set(setA);
    for (var elem of setB) {
        union.add(elem);
    }
    return union;
}

var setA = new Set([1, 2, 3, 4]),
    setB = new Set([2, 3]),
    setC = new Set([5]);
    
unionSet(setA, setB); // Set(4) {1, 2, 3, 4}
unionSet(setA, setC); // Set(5) {1, 2, 3, 4, 5}
```

### 차집합\(A-B\)

```javascript
function differenceSet(setA, setB) {
    var difference = new Set(setA);
    for (var elem of setB) {
        difference.delete(elem);
    }
    return difference;
}

var setA = new Set([1, 2, 3, 4]),
    setB = new Set([2, 3]);
    
differenceSet(setA, setB); // Set(2) {1, 4}
```

### 배열 중복 항목 확인하기

```javascript
function checkDuplicates(arr) {
    // 배열을 Set으로 변환함으로써 길이를 비교해 중복 여부를 확인할 수 있다.
    var mySet = new Set(arr);
    return mySet.size < arr.length;
}
checkDuplicates([1,2,3,4,5]); // false
checkDuplicates([1,1,2,3,4,5]); // true
```

* 시간 복잡도: O\(n\)
* 공간 복잡도: O\(n\)

### 여러 배열에서 유일한 값 배열 반환하기

```javascript
function uniqueList(arr1, arr2) {
    var mySet = new Set(arr1.concat(arr2));
    return Array.from(mySet);
}
uniqueList([1,1,2,2],[2,3,4,5]); // [1,2,3,4,5]
uniqueList([1,2],[3,4,5]); // [1,2,3,4,5]
uniqueList([],[2,2,3,4,5]); // [2,3,4,5]
```

* 시간 복잡도: O\(n+m\)
* 공간 복잡도: O\(n+m\)

