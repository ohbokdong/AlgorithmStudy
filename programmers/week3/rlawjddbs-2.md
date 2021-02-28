# [3차] 기능개발
```java
public static int[] solution(int[] progresses, int[] speeds) {
		
    List<Integer> result = new ArrayList<Integer>();
    
    int numberOfDist = 1;
    int prev = calc(progresses[0], speeds[0]);
    
    for(int i = 1, n = progresses.length; i < n; i++) {
            
        if (calc(progresses[i], speeds[i]) <= prev) {
            numberOfDist++;
        } else {
            result.add(numberOfDist);
            numberOfDist = 1;
            prev = calc(progresses[i], speeds[i]);
        }
        
    }
    result.add(numberOfDist);
    
    int[] answer = new int[result.size()];
    for(int i = 0, n = result.size(); i < n; i++) {
        answer[i] = result.get(i);
    }

    // System.out.println(Arrays.toString(answer));
    
    return answer;
    
}

private static int calc(int progress, int speed) {
    return (100 - progress) % speed > 0 ? (100 - progress) / speed + 1 : (100 - progress) / speed;
}
```