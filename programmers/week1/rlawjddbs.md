# [1차] 비밀지도 문제풀이
- 중간에 `toCharArray()`로 순환문 돌리는 대신 `replaceAll()`을 쓰는게 원래 생각하던 방향과 맞았을 듯
```java
class Solution {
    public String[] solution(int n, int[] arr1, int[] arr2) {
        String[] result = new String[n];

        for(int i = 0; i < n; i++) {

            // 비트 연산 후 변의 길이(n)에 맞춰 문자열 자리수 설정
            String strBin = String.format("%"+ n + "s", Integer.toBinaryString(arr1[i] | arr2[i])).replace(" ", "0");
            String wallOrSpace = ""; // 지도의 1열에 해당되는 문자열

            // 굳이 순환문을? - replaceAll() 두번으로 해결 가능했음
            for(char bin : strBin.toCharArray()) {
                switch (bin) {
                case '0':
                    wallOrSpace += " ";
                    break;
                case '1':
                    wallOrSpace += "#";
                    break;
                default:
                    wallOrSpace += " ";
                    break;
                }
            }

            result[i] = wallOrSpace;
        }

        return result;
    }
}
```

# [보충] - 팀원의 의견을 반영하여 수정해본 풀이
```java
class Solution {
    public String[] solution(int n, int[] arr1, int[] arr2) {
        String[] result = new String[n];

        for(int i = 0; i < n; i++) {
            // 아래 for문을 통해 단일 케이스("1"일 때 "#" 변환)에만 대응하면 되므로 replace 쓰지 않아도 됨
            String strBin = String.format("%"+ n + "s", Integer.toBinaryString(arr1[i] | arr2[i]));
            String wallOrSpace = "";

            for(char bin : strBin.toCharArray()) {
                switch (bin) {
                case '1':
                    wallOrSpace += "#";
                    break;
                default: // 예외 처리를 위해 default 문을 작성하는 것이 정석
                    wallOrSpace += " ";
                    break;
                }
            }

            // 각 열에 보물 위치 저장
            result[i] = wallOrSpace;
        }

        return result;
    }
}
```

## 인상깊은 다른 사람 풀이
- 자바 풀이 중 좋아요 가장 많이 받은 소스... 이긴 한데 GC가 많이 돌아서 출력 속도가 느리다는 리뷰가 있음
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