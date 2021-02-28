# [4차] 두 개 뽑아서 더하기
```java
public static int[] solution(int[] numbers) {
        
    TreeSet<Integer> set = new TreeSet<Integer>();
    
    for(int i = 0, n = numbers.length; i < n; i++) {
        for(int j = 0, m = numbers.length; j < m; j++) {
            if (i == j)
                continue;
            set.add(numbers[i] + numbers[j]);
        }
    }
    
    int[] answer = new int[set.size()];
    for(int i = 0, n = set.size(); i < n; i++) {
        answer[i] = set.pollFirst();
    }
    
    // System.out.println(Arrays.toString(answer));
    
    return answer;
}
```