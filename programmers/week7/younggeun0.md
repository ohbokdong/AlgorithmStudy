# [체육복](https://programmers.co.kr/learn/courses/30/lessons/42862)

## 1차 시도

* 무식하게 풀기

```javascript
// n - 전체 학생 수 (2 <= n <= 30)
// lost - 체육복을 도난당한 학생들의 번호가 담긴 배열 
// reserve - 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열
function solution(n, lost, reserve) {
    var answer = 0;
    
    // 1. 여분 가져온 학생들도 도난 당했을 수 있음(reserver에서 lost와 곂치는 번호는 reserve에서 제거)
    var enableReserve = [];
    var bReserveLost = false;
    var i,j, nNum, nNum2;
    for (i=0; i<reserve.length; i++) {
        nNum = reserve[i];
        for (j=0; j<lost.length; j++) {
            nNum2 = lost[j];
            if (nNum == nNum2) {
                bReserveLost = true;
                break;
            }
        }
        if (bReserveLost) {
            bReserveLost = false;
            continue;
        }
        enableReserve.push(nNum);
    }
    
    // 2. 체육복 잃은 수 제거(n - lost.length 체육수업 참여 학생 수)
    answer = n - lost.length;
    
    // 3. 체육복 빌려줄 수 있는 인원 수 추가
    for (i=0; i<enableReserve.length; i++) {
        nNum = enableReserve[i];
        var nBeside1 = nNum--;
        var nBeside2 = nNum++;
        
        for (j=0; j<lost.length; j++) {
            nNum2 = lost[j];
            
            if (nNum2 == nBeside1 || nNum2 == nBeside2) {
                answer++;
                lost.splice(j, 1);
                console.log(lost);
                break;
            }
        }
    }
    // 체육수업을 들을 수 있는 학생의 최댓값
    return answer;
}
```

* **reserve에 있고 lost에도 있는 경우, 둘 다 삭제해야 함**
  * 두 벌 있었지만 한 벌 있게 돼 체육복 한 벌만 갖게됐기 때문

```javascript
// n - 전체 학생 수 (2 <= n <= 30)
// lost - 체육복을 도난당한 학생들의 번호가 담긴 배열 
// reserve - 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열
function solution(n, lost, reserve) {
    var answer = 0;
    
    // 1. 여분 가져온 학생들도 도난 당했을 수 있음(reserve에서 lost와 곂치는 번호는 reserve, lost에서 제거)
    var enableReserve = [];
    var bLost = false;
    var i,j, nNum;
    for (i=0; i<reserve.length; i++) {
        nNum = reserve[i];
        for (j=0; j<lost.length; j++) {
            if (nNum == lost[j]) {          // reserve인데 도난당한 경우
                bLost = true;
                lost.splice(j, 1);
                break;
            }
        }
        if (bLost) {
            bLost = false;
            continue;
        }
        
        enableReserve.push(nNum);
    }
    
    // 2. 체육복 잃은 수 제거(n - lost.length 체육수업 참여 학생 수)
    answer = n - lost.length;
    
    // 3. 체육복 빌려줄 수 있는 인원 수 추가
    for (i=0; i<enableReserve.length; i++) {
        nNum = enableReserve[i];
        var nBeside1 = nNum - 1; // 겁나 댕청.. --나 ++ 연산 쓰게되면 값이 변경되는 걸 눈치 못 챔..
        var nBeside2 = nNum + 1;
        
        for (j=0; j<lost.length; j++) {
            if (lost[j] == nBeside1 || lost[j] == nBeside2) {
                answer++;
                lost.splice(j, 1);
                break;
            }
        }
    }
    
    // 체육수업을 들을 수 있는 학생의 최댓값
    return answer;
}
```