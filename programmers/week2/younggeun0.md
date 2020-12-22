# [2차] 실패율

* ~~속도 때문에 실패한 코드(Object 사용)~~
  * `Divide By Zero Exception` 예외처리를 안해서 실패했던 것..

```javascript
function solution(N, stages) {
    var answer = [];
    var nAllUsers = stages.length;  // 전체 유저 수
    
    // 1. 스테이지 별 유저수 계산 (스테이지 : 인원수)
    var objStageCnt = {};
    for (var i=0; i<nAllUsers; i++) {
        var nStage = parseInt(stages[i]);
        if (objStageCnt[nStage] === undefined) {
            objStageCnt[nStage] = 1;    
        } else {
            objStageCnt[nStage] = objStageCnt[nStage] + 1;
        }
    }
    // console.log(objStageCnt);
    
    // 2. 스테이지 별 실패율 계산
    var objStageFailure = {};
    var nClearCnt = 0;
    for (var i=0; i < N; i++) {
        var nUserOnStage = nAllUsers - nClearCnt;
        var nUserOnStageNotClear = objStageCnt[i + 1];
        if (!nUserOnStageNotClear || nUserOnStage == 0) {
            objStageFailure[i + 1] = 0;
            continue;
        }
        objStageFailure[i + 1] = nUserOnStageNotClear / nUserOnStage;
        nClearCnt += nUserOnStageNotClear;
    }
    // console.log(objStageFailure);
    
    // 3. 값 크기에 따라 내림차순 정렬
    var nHighest;
    var nStageIdx;
    var nAllChecked;
    while (true) {
        nHighest = -1
        nStageIdx = -1;
        nAllChecked = true;
        for (const nStage in objStageFailure) {
            if (objStageFailure[nStage] > nHighest) {
                nHighest = objStageFailure[nStage];
                nStageIdx = parseInt(nStage);
                nAllChecked = false;
            }
        }
        if (nAllChecked) break;
        answer.push(nStageIdx);
        delete objStageFailure[nStageIdx];
    }
    
    return answer;
}
```

![try1](https://github.com/ohbokdong/AlgorithmStudy/blob/main/programmers/week2/image/young_try1.png?raw=true)

* OBject보단 배열이 빠르다고 판단되어 배열로 리팩토링

```js
function solution(N, stages) {
    var answer = [];
    var nAllUser = stages.length;
    
    // 1. 스테이지 별 유저수 계산
    var arrCnt = new Array(N).fill(0);
    for (var i=0; i<nAllUser; i++) {
        var nStage = stages[i];
        if (nStage > N) continue;
        arrCnt[nStage - 1] += 1;
    }
    // console.log(arrStageCnt);
    
    // 2. 스테이지 별 실패율 계산
    var arrFailure = [];
    var nClear = 0;
    for (var i=0; i < N; i++) {
        if (arrCnt[i]  == 0 || (nAllUser - nClear) == 0) {
            arrFailure.push(0);
            continue;
        }
        arrFailure.push(arrCnt[i] / (nAllUser - nClear));    
        nClear += arrCnt[i];
    }
    // console.log(arrFailure);
    
    // 3. 값 크기에 따라 내림차순 정렬
    var nHighest;
    var nStageIdx;
    var nAllChecked;
    
    while (true) {
        nHighest = -1
        nStageIdx = -1;
        nAllChecked = true;
        for (var i=0; i<arrFailure.length; i++) {
            if (arrFailure[i] == -1) continue;
            if (arrFailure[i] > nHighest) {
                nHighest = arrFailure[i];
                nStageIdx = i;
                nAllChecked = false;
            }
        }
        if (nAllChecked) break;
        
        answer.push(nStageIdx + 1);
        arrFailure[nStageIdx] = -1;
    }
    
    return answer;
}
```

## 인상깊은 다른사람 풀이

* 

```javascript

```

## **추가로 배운 JS api**

* 
