# [가장 큰 수](https://programmers.co.kr/learn/courses/30/lessons/42746)
- Comparator 인터페이스 사용
- "330"과 "303"의 비교를 통해 더 큰수 비교 후 정렬
- "00..." 인 경우 그냥 0 이므로 "0" 반환
```java
public String solution(int[] numbers) {
    String answer = "";
            
    // 대입
    String[] strArr = new String[numbers.length];
    for (int i = 0, n = numbers.length; i < n; ++i) {
        strArr[i] = String.valueOf(numbers[i]);
    }

    // 정렬
    Arrays.sort(strArr, new Comparator<String>() {
        @Override
        public int compare(String str1, String str2) {
            return (str2 + str1).compareTo(str1 + str2);
        }
    });

    // 가장 큰 수가 0인 경우 - "0"
    if ("0".equals(strArr[0])) {
        return "0";
    }

    // return
    for (String str : strArr) {
        answer += str;
    }

    return answer;
}
```