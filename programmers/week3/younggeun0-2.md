# 기능개발

```javascript
function solution(progresses, speeds) {
    var answer = [];
    // 1. 각 기능 별 추가 개발이 몇일 필요한지 계산
    var arrProgressDays = [];
    for (var i=0; i<progresses.length; i++) {
        var nCurrentPer = progresses[i];
        var nProgressPerDay = speeds[i];
        
        var nDays = 0;
        while (nCurrentPer < 100) {
            nDays++;
            nCurrentPer += nProgressPerDay;
        }
        arrProgressDays.push(nDays);
    }
    // 2. 앞 기능이 개발완료 시 개발완료된 뒤 기능들을 더함
    var nFeatures = 0;
    while (arrProgressDays.length > 0) {
        var nDay = arrProgressDays[0];
        nFeatures++;
        
        for (var i=1;i<arrProgressDays.length; i++) {
            if (arrProgressDays[i] > nDay) {
                break;
            }
            nFeatures++;
        }
        answer.push(nFeatures);
        arrProgressDays.splice(0, nFeatures);
        nFeatures = 0;
    }
    
    return answer;
}
```

## 인상 깊은 다른사람 풀이

* `map`이랑 `Math.ceil` 이용하면 더 간단하게 기능별 필요한 개발일수를 구할 수 있었음

```js
function solution(progresses, speeds) {
    let answer = [0];
    let days = progresses.map((progress, index) => Math.ceil((100 - progress) / speeds[index]));
    let maxDay = days[0];

    for(let i = 0, j = 0; i< days.length; i++){
        if(days[i] <= maxDay) {
            answer[j] += 1;
        } else {
            maxDay = days[i];
            answer[++j] = 1;
        }
    }

    return answer;
}
```

* 다행히도 속도차이는 크게 안나는 듯
  * 내 풀이, 인상 깊은 다른 사람 풀이 : 평균 0.12초(소수점 세번째 자리에서 반올림)

![younggeun0_img1](https://github.com/ohbokdong/AlgorithmStudy/blob/main/programmers/week3/younggeun0_img1.png?raw=true)