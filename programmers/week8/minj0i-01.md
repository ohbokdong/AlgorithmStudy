# [가장 큰 수](https://programmers.co.kr/learn/courses/30/lessons/42746)

```JAVASCRIPT
function solution(numbers) {
    var answer = '';
    
    // 두개를 + 해도 문자처럼 합쳐질 수 있도록 먼저 문자열처럼 바꾸고
    // sort에서 (b+a) - (a+b) 형태로 해준다. (a-b, b-a 형태만 생각해서 자꾸 에러남)
    // forEach문으로 answer에다가 문자열로 더해준다
    let newArr = numbers.map(v => v+'').sort((a, b) => (b+a) - (a+b)).forEach(item => {
        answer = answer+item
    })
    
    // 테스트 마지막 케이스가 실패가 떠서
    // 만약에 [0, 0, 0]일때 "000"처럼 뜨는 것을 막아주기 위해 이런 경우 0만 나오도록 한다
    if ([...answer][0] === '0') answer = '0'

    return answer;
}
```

처음에 생각했던 방식이 왜 안될까 고민해봤다. (map 에서 ''를 안쓰고 바로 sort안에서 문자열로 만든 뒤 비교하는 법)   
MDN의 sort를 보니 해결 방식을 찾을 수 있었다.   
나는 계속 해서 return 값을 a - b 혹은 (a+''+b) - (b+''+a) 이런 식으로 고민을 했었는데   
1과 -1 만 가지고도 제대로 내가 원하는 대로 할 수 있었다.    
```JAVASCRIPT
function solution(numbers) {
    var answer = '';
 
    const newArr2 = numbers.sort((a, b) => {
        const fakeA = a+''+b
        const fakeB = b+''+a
        
        if(fakeA > fakeB) {
            return -1
        } else {
            return 1
        }
    })
  
    answer = newArr2.reduce((a, b) => a+''+b)
    if ([...answer][0] === '0') answer = '0'

    return answer;
}
```

[MDN에서 참고한 설명-Array.prototype.sort()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
compareFunction이 제공되지 않으면 요소를 문자열로 변환하고 유니 코드 코드 포인트 순서로 문자열을 비교하여 정렬됩니다. 예를 들어 "바나나"는 "체리"앞에옵니다. 숫자 정렬에서는 9가 80보다 앞에 오지만 숫자는 문자열로 변환되기 때문에 "80"은 유니 코드 순서에서 "9"앞에옵니다.   

compareFunction이 제공되면 배열 요소는 compare 함수의 반환 값에 따라 정렬됩니다. a와 b가 비교되는 두 요소라면,   

compareFunction(a, b)이 0보다 작은 경우 a를 b보다 낮은 색인으로 정렬합니다. 즉, a가 먼저옵니다.   
compareFunction(a, b)이 0을 반환하면 a와 b를 서로에 대해 변경하지 않고 모든 다른 요소에 대해 정렬합니다. 참고 : ECMAscript 표준은 이러한 동작을 보장하지 않으므로 모든 브라우저(예 : Mozilla 버전은 적어도 2003 년 이후 버전 임)가 이를 존중하지는 않습니다.   
compareFunction(a, b)이 0보다 큰 경우, b를 a보다 낮은 인덱스로 소트합니다.   
compareFunction(a, b)은 요소 a와 b의 특정 쌍이 두 개의 인수로 주어질 때 항상 동일한 값을 반환해야합니다. 일치하지 않는 결과가 반환되면 정렬 순서는 정의되지 않습니다.
따라서 compare 함수의 형식은 다음과 같습니다.   
```JAVASCRIPT
function compare(a, b) {
  if (a is less than b by some ordering criterion) {
    return -1;
  }
  if (a is greater than b by the ordering criterion) {
    return 1;
  }
  // a must be equal to b
  return 0;
}
```

