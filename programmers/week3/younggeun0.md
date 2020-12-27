# 예산

```javascript
function solution(d, budget) {
    var nCnt = 0;
    
    // 오름차순 정렬
    d.sort((a, b) => a - b);
    
    // budget보다 작을 때 cnt
    var nTmpBudget = 0;
    for(var i=0; i<d.length; i++) {
        nTmpBudget += d[i];
        if (nTmpBudget > budget) break;
        nCnt++;
    }
    
    return nCnt;
}
```

## 인상깊은 다른사람 풀이

* 추천수가 가장 많던 풀이
  * 코드는 깔끔하나 누적합 반복이 너무 많은듯함(누적합 후 budget보다 크면 맨 뒤에서 하나 빼는 동작 반복)

```js
function solution(d, budget) {
    d.sort((a, b) => a - b);

    while (d.reduce((a, b) => (a + b), 0) > budget) d.pop();

    return d.length; // 답을 저장할 변수없이 바로 length로 반환
}
```



## **추가로 배운 JS api**

* `sort` 함수 사용 시 원 배열이 정렬되는 걸 주의