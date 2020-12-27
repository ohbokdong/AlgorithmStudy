# 예산
(https://programmers.co.kr/learn/courses/30/lessons/12982)

```JAVASCRIPT
function solution(d, budget) {
    var answer = 0;
    var sum = 0;
    
    d.sort(function(a, b) {
        return a-b;
    });
    
    for (const item of d) {
        if(budget >= sum+item) {
            sum = sum+item;
            answer++;
        }
    }
    return answer;
}
```
- 분명히 d.sort()했는데 자꾸 케이스에서 틀린다고 나와서 뭐지?!? 했는데, 생각해보니 그냥 sort()를 쓰는 경우 아래처럼 나오는 거였다.

```JAVASCRIPT
var arr = [1, 10, 3, 5, 2, 22];
arr.sort();
(6) [1, 10, 2, 22, 3, 5]
```

- d.sort((a, b) => a - b); 로 써도 된다.
