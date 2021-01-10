# [월간 코드 챌린지 시즌1(두 개 뽑아서 더하기)](https://programmers.co.kr/learn/courses/30/lessons/68644)

```javascript
function solution(numbers) {
    var answer = [];
    
    // 1. numbers에 서로 다른 인덱스에 있는 두 개의 수를 뽑아 더해 만들 수 있는 모든 수를 배열에 담음
    var nCurIdx = 0;
    var tmpObj = {}
    
    // 이번에 배운 재귀함수를 이용한 완전 탐색 활용
    var add2Numbers = function(numbers, nCurIdx, tmpObj) {
        // base case
        if (nCurIdx == numbers.length) { return tmpObj; }
        
        for (var next=nCurIdx + 1; next < numbers.length; next++) {
            var key = numbers[nCurIdx] + numbers[next]; 
            tmpObj[key] = key; // 중복여부 상관없이 object에 'key' : key 설정
        }
        return add2Numbers(numbers, nCurIdx + 1, tmpObj);
    }

    tmpObj = add2Numbers(numbers, nCurIdx, tmpObj); // 재귀호출로 완성된 obj가 반환됨
    
    for (var key in tmpObj) {
        // 2. 오름차순 정렬하려고 했으나.. key값이 순서대로 뽑힘(!??)
        answer.push(tmpObj[key]);    
    }

    // answer.sort((a, b) => a - b);
    
    return answer;
}
```

* `for in`으로 object에 key값 뽑아오는 순서는 숫자 오름차순인건가?
  * `for ... in` 반복문은 정수가 아닌 이름을 가진 속성, 삭속된 모든 열거 가능한 속성들을 반환
  * Object 속성은 순서가 없어 문제가 될 수 있는 코드지만 일단 풀림(...)
  * [for in mdn](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for...in)

```js
// 그래도 해 본
// for in 속성 참조 순서 확인
function forInTest() {
    var obj = {
        '3' : 3,
        '1' : 1,
        '5' : 5,
        '8' : 8,
        '2' : 2
    }

    for (var key in obj) {
        console.log(obj[key]);
    }
}
forInTest();
// IE, Chrome, FireFox에서 모두 key값을 1 2 3 5 8 오름차순 순서로 반환
```

## 인상깊은 다른사람 풀이

* JS에서 `set` 타입이 없는 줄 알고 obj로 풀었는데 `Set` 을 이용해서 배열의 중복을 제거함..(?!)
* 풀이 복붙한 사람이 많은지 줄 맨 뒤에 `;` 없는 사람이 많았음ㅋㅋㅋ

```js
function solution(numbers) {
    const temp = []

    for (let i = 0; i < numbers.length; i++) {
        for (let j = i + 1; j < numbers.length; j++) {
            temp.push(numbers[i] + numbers[j])
        }
    }

    const answer = [...new Set(temp)]
    return answer.sort((a, b) => a - b)
}
```

## **추가로 배운 JS api**

* **Set**
  * `Set` 객체는 자료형에 관계 없이 원시 값과 객체 참조 모두 유일한 값을 저장할 수 있음
  * [Set mdn](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Set)
  * **역시 IE는 지원안함**

```js
// Array 객체와의 관계
var myArray = ['value1', 'value2', 'value3'];
var mySet = new Set(myArray); // Array를 Set으로 변환하기 위해서는 정규 Set 생성자 사용
mySet.has('value1'); // true 반환

// set을 Array로 변환하기 위해 전개 연산자 사용함.
console.log([...mySet]); // myArray와 정확히 같은 배열을 보여줌
```


* **전개구문(Spread Syntax)**
  * [전개구문 mdn](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
  * 배열이나 문자열과 같이 반복 가능한 문자를 0개 이상의 인수 (함수로 호출할 경우) 또는 요소(배열 리터럴의 경우) 로 확장하여, 0개 이상의 키-값의 쌍으로 객체를 확장시킬 수 있음
  * **역시 IE는 지원 안함**

```js
// 함수 호출에서의 전개
function sum(x, y, z) {
  return x + y + z;
}
const numbers = [1, 2, 3];
console.log(sum(...numbers)); // expected output: 6

//  배열 리터럴에서의 전개
var parts = ['shoulders', 'knees'];
var lyrics = ['head', ...parts, 'and', 'toes']; // ["head", "shoulders", "knees", "and", "toes"]

// 객체 리터럴에서의 전개(ECMAScript 2018에서 추가)
var obj1 = { foo: 'bar', x: 42 };
var obj2 = { foo: 'baz', y: 13 };

var clonedObj = { ...obj1 };
// Object { foo: "bar", x: 42 }
var mergedObj = { ...obj1, ...obj2 };
// Object { foo: "baz", x: 42, y: 13 }
```
