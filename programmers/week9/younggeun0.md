# [프린터](https://programmers.co.kr/learn/courses/30/lessons/42587)

## 1차 시도

* 포인터 이동으로 인쇄되는 순서를 찾고자 했음
  * 우선순위 값이 높은 프린트 항목들을 먼저 인쇄한다고 계산
    * location 우선순위가 가장 큰 경우 order++가 되는 경우가 없어 0, return 1처리
  * 우선순위 값이 높은 프린트가 될 때 그 앞에 있던 항목들이 뒤로 이동한 것을 location에 빼서 이동시킴
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

## 레거시 뻐큐머겅..

* 민정이 풀이를 보며 역시 사람은 배워야되는구나.. ES6 공부해야겠다 다짐합니다.😭

```js
function solution(priorities, location) {
    let order = 0;
    let printed = false;

    while(!printed) {
        const priority = priorities.shift();

        let bMoreImportExists = false;
        for (let i=0; i<priorities.length; i++) {
            if (priorities[i] > priority) {
                bMoreImportExists = true;
            }
        }

        if (bMoreImportExists) {
            priorities.push(priority);
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

// 민정이 풀이
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