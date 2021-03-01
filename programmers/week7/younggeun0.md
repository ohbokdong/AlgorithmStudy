# [체육복](https://programmers.co.kr/learn/courses/30/lessons/42862)

## 1차 시도

* 무식하게 풀기
  * 일단 되는대로 풀어보니 최댓값이 아닌 경우가 존재함

```javascript
// n - 전체 학생 수 (2 <= n <= 30)
// lost - 체육복을 도난당한 학생들의 번호가 담긴 배열 
// reserver - 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열
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
    
    // 위처럼 접근하면 최댓값은 못 구함
    // -> nBeside1이 아니고 nBeside2에게 빌려줘야 하는 최대값이 되는 경우가 존재하기 때문
    
    // 체육수업을 들을 수 있는 학생의 최댓값
    return answer;
}
```



