# [폰켓몬](https://programmers.co.kr/learn/courses/30/lessons/1845)

```JAVASCRIPT
function solution(nums) {
    var answer = 0;
    
    // 폰켓몬의 종류 - 지난 번에 array에서 중복 제거하는 set 쓰는 법이 인상깊었어서 적용해봄
    const typeCount = [...new Set(nums)].length
    // 데려가야하는 폰켓몬 수
    const needPokeCount = nums.length / 2
    
    // 폰켓몬 종류가 데려가야하는 수보다 크다면 최대 다양성 = 데려가야하는 수
    // 그게 아니라면 종류별로 하나씩 다 뽑고 중복되는 종류로 더 뽑아서 데려갈테니 어차피 최대 다양성 = 종류의 수
    answer = typeCount > needPokeCount ? needPokeCount : typeCount
    
    return answer;
}
```

다양성에 대한 문제가 아니라 경우의 수를 뽑아야 하는 건가 싶어서 처음에 nCr 의 공식을 적용해야 하는 건가 싶었다.
근데 문제를 다시 읽으니 '최대 고를 수 있는 폰켓몬 종류'라서 쉬워졌다.

