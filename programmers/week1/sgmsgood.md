# [1차] 비밀지도

## 끝까지 풀지는 못했음...
### 애석하게도? 풀려던 방향이랑 처음 해설이랑 방향이 비슷해서 아래 코드를 쓰게 되었음

1. 두 배열의 int를 toBinaryString로 바꾼 다음에 계산을 하려던 부분이 가장 큰 실수였음.
   (int를 그냥 or 연산자로 계산을 해도 되는구나... 깨달음)
```
for (int i = 0; i < n; i++) {
            result[i] = Integer.toBinaryString(arr1[i] | arr2[i]);
        }
``` 


```java
class Solution {
  public String[] solution(int n, int[] arr1, int[] arr2) {
        String[] result = new String[n];
        for (int i = 0; i < n; i++) {
            result[i] = Integer.toBinaryString(arr1[i] | arr2[i]);
        }

        for (int i = 0; i < n; i++) {
            result[i] = String.format("%" + n + "s", result[i]);
            result[i] = result[i].replaceAll("1", "#");
            result[i] = result[i].replaceAll("0", " ");
        }

        return result;
    }
}
```