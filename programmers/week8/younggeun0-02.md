# [폰켓몬](https://programmers.co.kr/learn/courses/30/lessons/1845)

## 1차 시도

* 그냥 중복된 수만 제거한다음에 비교하면 되는 문제라 쉽게 풂

```javascript
function solution(nums) {
    var answer = 0;
    // 1. 선택 가능한 폰켓몬의 수를 구함, 선택가능한 폰켓몬의 수 = N/2
    var nSelectable = nums.length / 2;
    
    // 2. Set 자료구조에 모두 넣어 중복된 종류 제거
    var setSpecies = new Set();
    for (var i=0; i<nums.length; i++) {
        setSpecies.add(nums[i]);
    }
    
    // 3. 선택가능한 폰켓몬 수를 종류의 수와 비교
    // 선택가능한 수 보다 종류가 많으면 선택가능한 수 리턴
    if (nSelectable < setSpecies.size)
        answer = nSelectable;
    else // 선택가능한 폰켓몬 수 보다 종류가 적으면 종류 수 리턴
        answer = setSpecies.size;
    
    return answer;
}
```