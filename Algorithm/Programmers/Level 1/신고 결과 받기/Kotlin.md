## 해결 방안

1. 유저가 신고한 ID(report)를 공백(스페이스) 단위로 나눈다.
2. 신고 당한 ID를 'key'로 신고한 ID들을 'value'로 그룹으로 묶는다. (나의 경우 해시맵을 사용함)
3. 'value'의 크기가 'k' 이상인 것만 필터를 한다.
4. 필터된 'value'를 각각 유저 ID 리스트(id_list)를 통해서 포지션을 구한 뒤 결과 배열에 맞는 포지션에 카운팅을 한다.


<br>

--- 

<br>

## 풀이   

<br>

> ### 내가 한 풀이

```kotlin

class Solution {
    fun solution(id_list: Array<String>, report: Array<String>, k: Int): IntArray {
        val answer = IntArray(id_list.size) { 0 }

        val hash = HashMap<String, ArrayList<String>>()
        report.forEach {
            val str = it.split(" ")
            if (hash[str[1]].isNullOrEmpty()) {
                hash[str[1]] = arrayListOf(str[0])
            } else {
                if (hash[str[1]]?.contains(str[0]) == false) {
                    hash[str[1]]?.add(str[0])
                }
            }
        }

        hash.values.filter { it.size >= k }.forEach {
            it.forEach { id ->
                val pos = id_list.indexOf(id)
                answer[pos] = answer[pos].plus(1)
            }
        }

        return answer
    }
}


```

> ### 다른 사람이 한 풀이

```kotlin

class Solution {
    fun solution(id_list: Array<String>, report: Array<String>, k: Int): IntArray =
    report.map { it.split(" ") }
        .groupBy { it[1] }
        .asSequence()
        .map { it.value.distinct() }
        .filter { it.size >= k }
        .flatten()
        .map { it[0] }
        .groupingBy { it }
        .eachCount()
        .run { id_list.map { getOrDefault(it, 0) }.toIntArray() }
}

```
