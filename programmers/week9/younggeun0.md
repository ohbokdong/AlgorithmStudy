# [프린터](https://programmers.co.kr/learn/courses/30/lessons/42587)

## 1차 시도

* 우선순위 값이 높은 프린트 항목들 먼저 계산 (order)
  * location에 있는 우선순위 값이 가장 큰 경우(order == 0) return 1
  * 우선순위 값이 높은 프린트가 될 때 그 앞에 뒤로 밀리 수를 모두 location에 빼서 이동시킴
  * 우선순위 값이 높은 프린트 처리 후 이동돼 있는 location 값 +1 리턴
  * 뭐가 문제지..

```javascript
function solution(priorities, location) {
    let order = 0;
    const locaPriority = priorities[location];
    let printQueLen = priorities.length;
    let moveCnt = 0;
    for (let i=0; i<priorities.length; i++) {
        const priority = priorities[i];

        moveCnt++;
        if (priority > locaPriority) {
            order++;
            printQueLen--;
            location -= moveCnt;
            if (location < 0) location += printQueLen;
            moveCnt = 0;
        }
    }

    if (order == 0) return 1;
    order += location + 1;
    return order;
}
// 망..
```

## 바닐라 자바스크립트 뻐큐머겅..

* 민정이 풀이를 보며 역시 사람은 배워야되는구나.. ES6 공부해야겠다 다짐합니다.😭

```js
function solution(priorities, location) {
    let order = 0;
    let printed = false;

    while(!printed) {
        const first = priorities.shift();

        if (priorities.filter(v => v > first).length > 0) {
            priorities.push(first);
        } else {
            order++;

            if (location == 0) {
                printed = true;
                continue;
            }
        }

        if (location == 0) {
            location = priorities.length - 1;
        } else {
            location--;
        }
    }

    return order;
}

```