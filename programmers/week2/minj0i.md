# 실패율
https://programmers.co.kr/learn/courses/30/lessons/42889

```JAVASCRIPT
function solution(N, stages) {
    var answer = [];
    var level = [];
   
    // 자리만큼 초기화
    for(var i=0; i<N; i++) {
        level.push({index: i+1, count: 0, pass: 0})
    }
    
    for(var i=0; i<stages.length; i++) {
        // 도달한 사람
        level.filter(v=> {return stages[i] - v.index >= 0}).map(value => {return value.pass++})
        // 실패한 사람의 수
        if (stages[i] != N+1) {
            level.find(v => v.index == stages[i]).count++
        }
        
    }
    
    // (실패한사람/도달한사람)으로 정렬하는 부분
    level.sort((a,b) => {
        // 같으면 작은수가 앞으로
        if (a.count/a.pass == b.count/b.pass) {
            return a.index - b.index
        } else {
            return b.count/b.pass - a.count/a.pass
        }
    })
    
    //index만 모아서 answer로
    answer = level.map(v => v.index)

    return answer;
}
```

지난 번에 map 쓰고싶다고 했다가 map과 find와 filter를 원없이 테스트해봤다(^^...)  
문제 제대로 안 읽고 풀었다가 나중에서야 실패율이 `실패한사람 / 도달한 사람`이라는 걸 깨달았다.  
문제 잘 읽기....  
다 쓰고 보니 level이라는 배열을 굳이 만들 필요는 없었음


## 인상깊은 풀이
```JAVASCRIPT
function solution(N, stages) {
    let ans = []

    for (let i = 1; i <= N; ++i) {
        let usersReachedCurrentStage   = stages.reduce((acc, curStage) => acc + ((curStage >= i) ? 1 : 0), 0)
        let usersStagnatedCurrentStage = stages.reduce((acc, curStage) => acc + ((curStage == i) ? 1 : 0), 0)
        if (usersReachedCurrentStage === 0) {
            ans.push({ 'stage': i, 'failRate': 0 })
            continue
        }

        ans.push({ 'stage': i, 'failRate': (usersStagnatedCurrentStage / usersReachedCurrentStage) })
    }

    return ans.sort((a, b) => {
        if (a.failRate > b.failRate) return -1
        if (a.failRate < b.failRate) return 1
        return a.stage - b.stage
    }).map(entry => entry.stage)
}
```
- reduce 사용해서 조건에 해당하면 하나씩 ++해줄 수 있음
- ans 배열에 아예 failRate로 계산한 값 넣어서 처리해줘서 연산 계속 안해도 됨