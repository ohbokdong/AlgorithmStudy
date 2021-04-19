# [스킬트리](https://programmers.co.kr/learn/courses/30/lessons/49993#)
- 정규식으로 풀어보려 시도해본 결과물
```JS
function solution(skill, skill_trees) {
    
    // 정규식 패턴 만들기
    var regPattern = "[^";
    skill.split("").forEach(function(v, i) {
        regPattern += v;
    });
    regPattern += "]"
    
    // 정규표현식 인스턴스 생성
    var regExp = new RegExp(regPattern, "g"); // 전역 검색

    var matchList = skill_trees.map(function(ele) {
        return ele.replace(regExp, ""); // skill을 제외한 다른 스킬(문자열) 제거
    }).filter(function(ele) {
        return skill.indexOf(ele) === 0 || ele === ""; // 순서가 맞는지 확인 올바른 스킬트리만 반환
    });

    return matchList.length; // 길이 반환

}
```