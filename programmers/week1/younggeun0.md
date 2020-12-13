# [1차] 다트게임 문제풀이

```javascript
function solution(n, arr1, arr2) {
    var answer = [];
    // 1. arr1, arr2 정수로 들어옴 -> 이진수 배열로 변환
    var binaryArr1 = [];
    var binaryArr2 = [];
    for (var i=0; i<n; i++) {
        binaryArr1.push(getBinaryNum(arr1[i], n));
        binaryArr2.push(getBinaryNum(arr2[i], n));
    }
    // 2. binaryArr1, binaryArr2 지도 합치기
    // - 어느 하나라도 벽(1)인 부분은 벽, 모두 공백(0)인 부분은 공백
    console.log(binaryArr1);
    console.log(binaryArr2);
    
    var tmpVal = "";
    for (var i=0; i<n; i++) {
        for (var j=0; j<n; j++) {
            var map1 = binaryArr1[i].charAt(j);
            var map2 = binaryArr2[i].charAt(j);
            
            if (map1 == "0" && map2 == "0") {
                tmpVal = tmpVal + ' ';
            } else {
                tmpVal = tmpVal + '#';
            }
        }
        answer.push(tmpVal);
        tmpVal = "";
    }
    return answer;
}

function getBinaryNum(num, n) {
    var r = "";
    r = num.toString(2); // toString(2)를 이용하면 정수를 이진수로 변환가능
    
    // n 보다 짧으면 앞에 0을 붙여줌 
    if (r.length < n) {
        var nZero = n - r.length;
        for (var i=0; i<nZero; i++) {
            r = "0" + r;
        }
    }
    return r;
}
```

## 인상깊은 다른사람 풀이

```javascript
function solution(n, arr1, arr2) {
    return arr1.map((v, i) => addZero(n, (v | arr2[i]).toString(2)).replace(/1|0/g, a => +a ? '#' : ' '));
}

const addZero = (n, s) => {
    return '0'.repeat(n - s.length) + s;
}
```

## **추가로 배운 JS api**

* [map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
  * 배열 내 모든 요소 각각에 대해 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환

```js
const array1 = [1, 4, 9, 16];

// pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// expected output: Array [2, 8, 18, 32]
```

* [repeat](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)
  * 문자열을 주어진 횟수만큼 반복해 붙인 새로운 문자열을 반환

```js
'abc'.repeat(2);    // 'abcabc'
```

* [toString](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/toString)
  * toString 함수를 이용하면 특정 진수값으로 변환할 수 있음

```js
var x = 6;
console.log(x.toString(2));       // displays '110'
```

  * [padStart](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/padStart)
    * 현재 문자열의 시작을 다른 문자열로 채워, 주어진 길이를 만족하는 새로운 문자열을 반환
    * 채워넣기는 대상 문자열의 시작(좌측)부터 적용됨

```js
const str1 = '5';

console.log(str1.padStart(2, '0'));
// expected output: "05"
```