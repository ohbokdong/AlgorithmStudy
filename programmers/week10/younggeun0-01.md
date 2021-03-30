# [H-Index](https://programmers.co.kr/learn/courses/30/lessons/42747)

```javascript
function solution(citations) {
    // citation example [3, 0, 6, 1, 5]
    let n = citations.length;
    // 인용 횟수가 h번 이상인 논문이 h개일 때 가능한 h의 최댓값
    citations = citations.sort((a,b) => b-a); // 내림차순 정렬
    // sorted citations : [6, 5, 3, 1, 0]
    //            index :  0  1  2  3  4

    for (let i=0; i<n; i++) {
        // 인덱스에 해당하는 배열 값이 인덱스보다 작거나 같으면 해당 인덱스가 정답
        if (citations[i] <= i) {
            return i;
        }
    }

    return n;
}
```
