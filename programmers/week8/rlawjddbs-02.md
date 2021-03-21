# [폰켓몬](https://programmers.co.kr/learn/courses/30/lessons/1845)
- 중복 방지를 위해 `Set` 사용
- `Set`의 크기가 가져갈 수 있는 폰켓몬의 최대 수를 넘어서면 `최대수 / 2`를 반환
```java
public int solution(int[] numbers) {
    Set<Integer> bag = new TreeSet<Integer>();
		
    for(int num : nums) {
        bag.add(num);
    }
    
    // 고를 수 있는 폰켓몬의 최대 수를 넘어선 경우
    if (bag.size() > numbers.length / 2) {
        return numbers.length / 2;
    }
		
    return bag.size();
}
```

### 비슷한 정답
- `ArrayList`의 `contains` 메서드로 값이 없는 경우에만 `add`
```java
public int solution(int[] nums) {
    //1. 기존 length를 구한다.
    //2. 중복값을 제거한 length를 구한다.
    //3. 두 값중 최소값이 정답.
    List<Integer> list = new ArrayList<Integer>();
    for(int i = 0 ; i < nums.length; i++){
        if(!list.contains(nums[i])){
            list.add(nums[i]);
        }
    }

    return nums.length/2 > list.size()?list.size():nums.length/2;
}
```