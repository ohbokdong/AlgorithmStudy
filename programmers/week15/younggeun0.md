# [키패드 누르기](https://programmers.co.kr/learn/courses/30/lessons/67256)

```js
// 2, 5, 8, 0 일 때 거리를 미리 정의
// 시작이라면, #이나 *에서도 출발 가능
const dist = {};
        // 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
dist[2] = [3, 1, 0, 1, 2, 1, 2, 3, 2, 3, 4];
dist[5] = [2, 2, 1, 2, 1, 0, 1, 2, 1, 2, 3];
dist[8] = [1, 3, 2, 3, 2, 1, 2, 1, 0, 1, 2];
dist[0] = [0, 4, 3, 4, 3, 2, 3, 2, 1, 2, 1];

function getDist(number, curHand) {
    if (curHand == "*" || curHand == "#") curHand = 10;
    return dist[number][curHand];
}

function solution(numbers, hand) {
    var answer = [];
    let leftHand = "*";
    let rightHand = "#";

    numbers.forEach(number => {
        switch(number) {
            case 1:
            case 4:
            case 7:
                leftHand = number;
                answer.push("L");
                break;
            case 3:
            case 6:
            case 9:
                rightHand = number;
                answer.push("R");
                break;
            default: { // 2,5,8,0
                // number, hand, leftHand, rightHand
                const leftDist = getDist(number, leftHand);
                const rightDist = getDist(number, rightHand);

                if (leftDist < rightDist) {
                    leftHand = number;
                    answer.push("L");
                } else if (rightDist < leftDist) {
                    rightHand = number;
                    answer.push("R");
                } else { // == 
                    if (hand == "left") {
                        leftHand = number;
                        answer.push("L");
                    } else { // right
                        rightHand = number;
                        answer.push("R");
                    }
                }
                break;
            }
        }
    });

    return answer.join(''); // Array to String
}
```
