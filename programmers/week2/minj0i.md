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

---
## JAVA
- 제출후 채점했을 때 테스트 3, 10, 11에서 에러남
- CompareTo 잘 몰라서 인터넷 찾아봄
```JAVA
import java.util.*;

// compareTo를 사용
class Level implements Comparable<Level> {
    private int index;
    private Double rate;
    
    public Level(int index, double rate) {
        this.index = index;
        this.rate = rate;
    }
    
    public int getIndex() {
        return index;
    }
    
    @Override
    public int compareTo(Level level) {
        if(this.rate == level.rate) {
            return this.index < level.index ? -1 : 1;
        } else {
            return this.rate > level.rate ? -1 : 1;
        }
    }
}

class Solution {
    public int[] solution(int N, int[] stages) {
        int[] answer = new int[N];
        int[] people = new int[N+1];
        int[] count = new int[N+1];
        double[] rate = new double[N];
        Level[] arr = new Level[N];
        
        //초기화
        for(int i=0; i<N+1; i++) {
            people[i] = 0;
            count[i] = 0;
        }
        
        for(int stage: stages) {
            // 도달한 사람
            for(int j=0; j<stage; j++) {
                people[j]++;
            }
            // 아직 클리어하지 못한 플레이어의 수
            if (stage < N+1) {
                count[stage]++;
            }
        }
        
        // 실패율 구하기
        for(int i=0; i<N; i++) {
            if(count[i+1] == 0) {
                rate[i] = 0;
            } else {
                rate[i] =(double)count[i+1]/people[i];
            }
            arr[i] = new Level(i, rate[i]);
        }
        
        // 실패율의 정렬
        Arrays.sort(arr);
        
        // answer에 해당 값 넣기
        for(int i=0; i<N; i++) {
            answer[i] = arr[i].getIndex()+1;
        }
        
        return answer;
    }
}
```

```
테스트 1 〉	통과 (0.43ms, 52.2MB)
테스트 2 〉	통과 (0.67ms, 52.6MB)
테스트 3 〉	실패 (19.88ms, 53.9MB)
테스트 4 〉	통과 (20.64ms, 57.4MB)
테스트 5 〉	통과 (32.18ms, 62.2MB)
테스트 6 〉	통과 (1.44ms, 52.6MB)
테스트 7 〉	통과 (6.94ms, 52.9MB)
테스트 8 〉	통과 (18.37ms, 56.4MB)
테스트 9 〉	통과 (27.52ms, 61.3MB)
테스트 10 〉	실패 (24.77ms, 55.9MB)
테스트 11 〉	실패 (런타임 에러)
테스트 12 〉	통과 (26.35ms, 59.3MB)
테스트 13 〉	통과 (29.16ms, 59.9MB)
테스트 14 〉	통과 (0.58ms, 53.2MB)
테스트 15 〉	통과 (11.99ms, 54.8MB)
테스트 16 〉	통과 (3.92ms, 54.2MB)
테스트 17 〉	통과 (11.97ms, 54.1MB)
테스트 18 〉	통과 (3.45ms, 53.8MB)
테스트 19 〉	통과 (1.21ms, 53.1MB)
테스트 20 〉	통과 (4.21ms, 55MB)
테스트 21 〉	통과 (5.35ms, 55.9MB)
테스트 22 〉	통과 (28.07ms, 62MB)
테스트 23 〉	통과 (10.20ms, 59.2MB)
테스트 24 〉	통과 (11.83ms, 58.8MB)
테스트 25 〉	통과 (0.55ms, 51.8MB)
테스트 26 〉	통과 (0.44ms, 52MB)
테스트 27 〉	통과 (0.49ms, 52.4MB)

채점 결과
정확성: 88.9
합계: 88.9 / 100.0
```

- 안되는데 왜 안되는지 모르겠어서 compareTo같은건 갖다버리고 그냥 for문 돌려서 직접 비교하는 거로 바꿔봄
```JAVA
class Solution {
    public int[] solution(int N, int[] stages) {
        int[] answer = new int[N];
        double[] rates = new double[N];
        
        for(int i=1; i<=N; i++) {
            // 도달한 사람, 패스한 사람
            int count = 0;
            int pass = 0;
            
            for(int stage: stages) {
                if (i <= stage) pass++;
                if (i == stage) count++;
            }
            
            if (count == 0) {
                rates[i-1] = 0;
                continue;
            }
            rates[i-1] = count*1.0 / pass;
        }
        
        for(int i=0; i<N; i++) {
            System.out.println(rates[i]);
        }
        
        for(int i=0; i<rates.length; i++) {
            //0보다 작은 값
            double max = -1;
            int maxIdx = 0;
            
            for(int j=0; j<rates.length; j++) {
                if (max < rates[j]) {
                    max = rates[j];
                    maxIdx = j;                        
                }
            }
             rates[maxIdx] = -1; //다음 for문 돌때 무시할 수 있도록
             answer[i] = maxIdx + 1;
        }
        return answer;
    }
}
```
- 내 로직이 느릴 수 밖에 없는 건지.. 속도가 신경쓰임
- 곧 시간복잡도 배우겠지(^^)?
