# [1차] 비밀지도 문제풀이

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

* map을 이용, arr1의 아이템과 index를 바로 전달
  * arr2의 아이템과 arr1의 아이템(v)를 OR 연산(|)
  * 연산 결과를 바로 이진화(toString(2))
  * addZero함수에서 repeat함수를 사용, 앞에 부족한 '0' 추가
  * 이진수를 replace함수를 사용하여 전역으로 1,0값을 '#', ' '로 치환

```javascript
function solution(n, arr1, arr2) {
    return arr1.map((v, i) => addZero(n, (v | arr2[i]).toString(2)).replace(/1|0/g, a => +a ? '#' : ' '));
}

// +1 ? '#' : ' '   == '#'
// +0 ? '#' : ' '   == ' ' 

const addZero = (n, s) => {
    return '0'.repeat(n - s.length) + s;
}
```

## **추가로 배운 JS api**

* [replace](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/replace)
  * 어떤 패턴에 일치하는 일부 또는 모든 부분이 교체된 새로운 문자열을 반환
  * 패턴은 문자열이나 정규식(RegExp)이 될 수 있으며, 교체 문자열은 문자열이나 모든 매치에 대해서 호출된 함수일 수 있음

```js
const p = 'The quick brown fox jumps over the lazy dog. If the dog reacted, was it really lazy?';
const regex = /dog/gi;

console.log(p.replace(regex, 'ferret'));
// expected output: "The quick brown fox jumps over the lazy ferret. If the ferret reacted, was it really lazy?"
console.log(p.replace('dog', 'monkey'));
// expected output: "The quick brown fox jumps over the lazy monkey. If the dog reacted, was it really lazy?"
```

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
