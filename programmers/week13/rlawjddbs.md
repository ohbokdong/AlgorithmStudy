# [방금 그 곡](https://programmers.co.kr/learn/courses/30/lessons/17683)
- 3, 6, 7, 8, 9, 12, 15, 18, 19 에러 발생
```JS
function solution(m, musicinfos) {
    var answer = [];

    const regExp = /\S\#/g; // 영문 + # 에 해당되는 문자를 찾아내는 정규식

    m = m.replace(regExp, function(v) { return v.substring(0, 1).toLowerCase(); });

    musicinfos.forEach(function(info, i) {

        const splitInfo = info.split(",");
        
        let start = parseInt(splitInfo[0].substring(0, 2)) * 60 + parseInt(splitInfo[0].substring(3, 5));
        let end = parseInt(splitInfo[1].substring(0, 2)) * 60 + parseInt(splitInfo[1].substring(3, 5));

        let diff = end - start; // 재생 시간 계산
        splitInfo[4] = end - start; // 재생 시간 별도 저장

        let melody = "";

        do {
            melody += splitInfo[3].substring(0, diff); // 멜로디 입력
            diff -= splitInfo[3].length; // 재생시간에서 멜로디 입력된 만큼 차감
        } while (diff > 0); // 재생시간이 소진되기 전까지 반복

        melody = melody.replace(regExp, function(v) { return v.substring(0, 1).toLowerCase(); });

        if (melody.indexOf(m) != -1) { // 재생된 멜로디가 찾고자 하는 멜로디와 같다면
            answer.push(splitInfo); // 배열에 저장
        }

    });
    
    if (answer.length == 0) { // 일치하는 멜로디가 없는 경우
        return "(None)";
    } else {
        answer.sort(function(a, b) { // 재생 시간이 큰 순으로 정렬
            if (a[4] < b[4]) { return 1; }
            else if (a[4] > b[4]) { return -1; }
            else { return 0; }
        });
    }
    
    return answer[0][2]; // 첫 번째 곡의 이름을 반환
}
```