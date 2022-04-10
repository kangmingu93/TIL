# 신고 결과 받기   

## 문제 설명   
신입사원 무지는 게시판 불량 이용자를 신고하고 처리 결과를 메일로 발송하는 시스템을 개발하려 합니다. 무지가 개발하려는 시스템은 다음과 같습니다.
* 각 유저는 한 번에 한 명의 유저를 신고할 수 있습니다.
    * 신고 횟수에 제한은 없습니다. 서로 다른 유저를 계속해서 신고할 수 있습니다.
    * 한 유저를 여러 번 신고할 수도 있지만, 동일한 유저에 대한 신고 횟수는 1회로 처리됩니다.
* k번 이상 신고된 유저는 게시판 이용이 정지되며, 해당 유저를 신고한 모든 유저에게 정지 사실을 메일로 발송합니다.
    * 유저가 신고한 모든 내용을 취합하여 마지막에 한꺼번에 게시판 이용 정지를 시키면서 정지 메일을 발송합니다.

<br>

다음은 전체 유저 목록이 ["muzi", "frodo", "apeach", "neo"]이고, k = 2(즉, 2번 이상 신고당하면 이용 정지)인 경우의 예시입니다.   

|유저 ID|유저가 신고한 ID|설명|
|:---|:---|:---|
|"muzi"|"frodo"|"muzi"가 "frodo"를 신고했습니다.|
|"apeach"|"frodo"|"apeach"가 "frodo"를 신고했습니다.|
|"frodo"|"neo"|"frodo"가 "neo"를 신고했습니다.|
|"muzi"|"neo"|"muzi"가 "neo"를 신고했습니다.|
|"apeach"|"muzi"|"apeach"가 "muzi"를 신고했습니다.|

<br>

각 유저별로 신고당한 횟수는 다음과 같습니다.

|유저 ID|신고당한 횟수|
|:---|:---|
|"muzi"|1|
|"frodo"|2|
|"apeach"|0|
|"neo"|2|

<br>

위 예시에서는 2번 이상 신고당한 "frodo"와 "neo"의 게시판 이용이 정지됩니다. 이때, 각 유저별로 신고한 아이디와 정지된 아이디를 정리하면 다음과 같습니다.

|유저 ID|유저가 신고한 ID|정지된 ID|
|:---|:---|:---|
|"muzi"|["frodo", "neo"]|["frodo", "neo"]|
|"frodo"|["neo"]|["neo"]|
|"apeach"|["muzi", "frodo"]|["frodo"]|
|"neo"|없음|없음|

<br>

따라서 "muzi"는 처리 결과 메일을 2회, "frodo"와 "apeach"는 각각 처리 결과 메일을 1회 받게 됩니다.   

이용자의 ID가 담긴 문자열 배열 <code>id_list</code>, 각 이용자가 신고한 이용자의 ID 정보가 담긴 문자열 배열 <code>report</code>, 정지 기준이 되는 신고 횟수 <code>k</code>가 매개변수로 주어질 때, 각 유저별로 처리 결과 메일을 받은 횟수를 배열에 담아 return 하도록 solution 함수를 완성해주세요.   

<br>

---

<br>

## 제한 사항

* 2 <= <code>id_list</code> <= 1,000   
    * 1 <= <code>id_list</code> <= 10
    * <code>id_list</code> 의 원소는 이용자의 id를 나타내는 문자열이며 알파벳 소문자로만 이루어져 있습니다.
    * <code>id_list</code> 에는 같은 아이디가 중복해서 들어있지 않습니다.   
* 1 <= <code>report</code> 의 길이 <= 200,000   
    * 3 <= <code>report</code> 의 원소 길이 <= 21
    * <code>report</code> 의 원소는 "이용자id 신고한id"형태의 문자열입니다.
    * 예를 들어 "muzi frodo"의 경우 "muzi"가 "frodo"를 신고했다는 의미입니다.
    * id는 알파벳 소문자로만 이루어져 있습니다.
    * 이용자id와 신고한id는 공백(스페이스)하나로 구분되어 있습니다.
    * 자기 자신을 신고하는 경우는 없습니다.   
* 1 <= <code>k</code> <= 200, <code>k</code> 는 자연수입니다.
* return 하는 배열은 <code>id_list</code> 에 담긴 id 순서대로 각 유저가 받은 결과 메일 수를 담으면 됩니다.   

<br>

---   

<br>

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

private fun solution(id_list: Array<String>, report: Array<String>, k: Int): IntArray {
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
