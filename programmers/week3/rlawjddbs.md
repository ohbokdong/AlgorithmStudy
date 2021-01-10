# [3차] 예산
```java
import java.util.Arrays;

class Solution {
    public int solution(int[] d, int budget) {
        
        Arrays.sort(d);
        
        int refVal = 0;
        for(int i = 0; i < d.length; i++) {
            
            if (refVal + d[i] <= budget)
                refVal += d[i];
            else
                return i;
            
        }
        
        return d.length;
        
    }
}
```

## 정확성 테스트에서 실패뜨는 코드
- 원인 파악 필요
```java
import java.util.Arrays;
import java.util.Collections;

class Solution {

    public int solution(int[] d, int budget) {
        
        Integer[] dCntArr = new Integer[d.length];
        
        for(int i = 0; i < d.length; i++) {
            
            int refVal = d[i];
            
            if (refVal > budget) {
            	dCntArr[i] = 0;
            	continue;
            }
            
            int dCnt = 1;
            
            for(int j = 0; j < d.length; j++) {
                
                if (j == i) 
                    continue;
                
                if (refVal + d[j] <= budget) {
                    refVal += d[j];
                    dCnt++;
                }
                
            }
        
            dCntArr[i] = dCnt;
            
        }
        
        Arrays.sort(dCntArr, Collections.reverseOrder());
        
        return dCntArr[0];
        
    }

}
```