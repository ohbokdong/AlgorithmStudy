```java
import java.util.Arrays;
import java.util.Comparator;

class Solution {
    public static int[] solution(int N, int[] stages) {

        int[] answer = new int[N];
        float[][] fRate = new float[N][2]; // fRate[n][0] = 실패율, fRate[n][1] = 해당 실패율의 index

        for(int i = 1; i < N+1; i++) {

            int notArrive = 0; // 도달X
            int failure = 0; // 도달O, 클리어X
            int arrival = 0; // 도달O, 클리어O

            for(int j = 0; j < stages.length; j++) {
                if (i <= stages[j]) { // 스테이지에 도달한 경우
                    arrival++;
                    if (i == stages[j]) { // 도달했지만 클리어 못함
                        failure++;
                    }
                } else { // 도달 못한 경우
                    notArrive++;
                }
            }

            fRate[i-1][1] = i;

            if (notArrive == stages.length) { // 스테이지에 도달한 유저가 없음
                fRate[i-1][0] = 0;
            } else {
                fRate[i-1][0] = ((float)failure / (float)arrival);    
            }

        }

        // Comparator 구현하여 정렬
        Arrays.sort(fRate, new Comparator<float[]>() {
            @Override
            public int compare(float[] o1, float[] o2) { 
                // 2차원 배열은 1차원 배열로 비교됨 - o1[0] = fRate[0][0], o2[0] = fRate[1][0]
                if(o1[0] < o2[0]) { return 1; }
                if(o1[0] > o2[0]) { return -1; }
                return 0;
            }
        });

        // 정렬된 실패율의 원래 index를 저장
        for (int i = 0; i < N; i++) {
            answer[i] = (int)fRate[i][1];
        }

        return answer;

    }
}
```