# [8차] 체육복
```java
import java.util.List;
import java.util.LinkedList;

class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int answer = 0;
		
		List<Integer> studentList = new LinkedList<Integer> ();
		
		for(int i = 0; i < n; i++) {
			studentList.add(i, 1); // 기본 1벌 씩
			for(int num : reserve) {
				if (num - 1 == i) {
					studentList.set(i, studentList.get(i) + 1); // 여벌 있는 학생은 +1
				} 
			}
		}
		
		for(int num : lost) {
			studentList.set(num - 1, studentList.get(num - 1) - 1); // 분실 -1
		}
		
		for (int i = 0; i < n; i++) {
			
			Integer prev = i - 1 > -1 ? i - 1 : null;
			Integer next = i + 1 < n ? i + 1 : null;
			
			if (studentList.get(i) == 0) { // 분실한 학생
				if (prev != null && studentList.get(prev) > 1) { // 이전 번호의 학생이 여벌이 있는 경우
					studentList.set(prev, studentList.get(prev) - 1); // 여벌 체육복 -1
					studentList.set(i, studentList.get(i) + 1); // 분실한 학생 체육복 +1
				} else if (next != null && studentList.get(next) > 1) { // 이후 번호의 학생이 여벌이 있는 경우
					studentList.set(next, studentList.get(next) - 1);
					studentList.set(i, studentList.get(i) + 1);
				}
			}
			
		}
		
		for (Integer num : studentList) { // 카운트
			if (num != 0)
				answer++;
		}
		
        return answer;
    }
}
```

### 간결한 코드 두 가지
```java
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int[] people = new int[n];
        int answer = n;

        for (int l : lost) 
            people[l-1]--;
        for (int r : reserve) 
            people[r-1]++;

        for (int i = 0; i < people.length; i++) {
            if(people[i] == -1) {
                if(i-1>=0 && people[i-1] == 1) {
                    people[i]++;
                    people[i-1]--;
                }else if(i+1< people.length && people[i+1] == 1) {
                    people[i]++;
                    people[i+1]--;
                }else 
                    answer--;
            }
        }
        return answer;
    }
}
```
   
```java
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int answer = 0;
        answer = n;

        for(int i = 0; i < lost.length; i++) {
            boolean rent = false;
            int j = 0;
            while(!rent) {
                if(j == reserve.length)                   break;
                if(lost[i] == reserve[j])                {reserve[j] = -1; rent=true;}
                else if(lost[i] - reserve[j] == 1 )      {reserve[j] = -1; rent=true;}
                else if(lost[i] - reserve[j] == -1)      {reserve[j] = -1; rent=true;}
                else                                     {j++;                      }
            }
            if(!rent) answer--;
        }
        return answer;
    }
}
```