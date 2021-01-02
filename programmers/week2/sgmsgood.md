# [2차] 실패율

```kotlin
class Solution {
    fun solution(N: Int, stages: IntArray): MutableList<Double> {
        var answer = intArrayOf()
        var result = mutableListOf<Double>()
        
        getChallenger(stages)?.forEach { (key, value) ->
         
                var people = 0
                var challenger = stages.size - people
                result.add(value.toDouble()/challenger.toDouble())
                people =+ value
            
        }
        
        return result
    }
    
    //Key는 단계, value는 list 형식의 Map 반환
    fun getChallenger(stages: IntArray): Map<Int, Int> {
        var stageChallengerMap = mapOf<Int, List<Int>>()
        var a = mutableMapOf<Int, Int>()
        stageChallengerMap = stages.groupBy {it}
         
        stageChallengerMap.forEach { (key, value) ->
            a.put(key, value.size) 
        }
        
        return a
    }
} 
```