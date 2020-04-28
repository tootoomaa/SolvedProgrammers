문제 설명
--------

**\+문제 요약**

> 일반적인 프린터는 인쇄 요청이 들어온 순서대로 인쇄합니다. 그렇기 때문에 중요한 문서가 나중에 인쇄될 수 있습니다. 이런 문제를 보완하기 위해 중요도가 높은 문서를 먼저 인쇄하는 프린터를 개발했습니다. 이 새롭게 개발한 프린터는 아래와 같은 방식으로 인쇄 작업을 수행합니다.
>
> ```
> 1. 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.
> 2. 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.
> 3. 그렇지 않으면 J를 인쇄합니다.
> ```
>
> 예를 들어, 4개의 문서(A, B, C, D)가 순서대로 인쇄 대기목록에 있고 중요도가 2 1 3 2 라면 C D A B 순으로 인쇄하게 됩니다.
>
> 내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 알고 싶습니다. 위의 예에서 C는 1번째로, A는 3번째로 인쇄됩니다.
>
> 현재 대기목록에 있는 문서의 중요도가 순서대로 담긴 배열 priorities와 내가 인쇄를 요청한 문서가 현재 대기목록의 어떤 위치에 있는지를 알려주는 location이 매개변수로 주어질 때, 내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 return 하도록 solution 함수를 작성해주세요.



**\+제한 조건**

> - 현재 대기목록에는 1개 이상 100개 이하의 문서가 있습니다.
> - 인쇄 작업의 중요도는 1~9로 표현하며 숫자가 클수록 중요하다는 뜻입니다.
> - location은 0 이상 (현재 대기목록에 있는 작업 수 - 1) 이하의 값을 가지며 대기목록의 가장 앞에 있으면 0, 두 번째에 있으면 1로 표현합니다.

**\+입출력 예**

| priorities         | location | return |
| ------------------ | -------- | ------ |
| [2, 1, 3, 2]       | 2        | 1      |
| [1, 1, 9, 1, 1, 1] | 0        | 5      |


##### 입출력 예 설명

예제 #1
문제에 나온 예와 같습니다.

예제 #2
6개의 문서(A, B, C, D, E, F)가 인쇄 대기목록에 있고 중요도가 1 1 9 1 1 1 이므로 C D E F A B 순으로 인쇄합니다.



**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/42587

문제 풀이
---------

**\+My Solution**

```swift
func solution(_ priorities:[Int], _ location:Int) -> Int {
    var tempPriorities = priorities	// 출력물 리스트
    var tempLocation:Int = location	// 목표 출력물 인덱스
    var returnValue:Int = 0					// 프린트된 출력물 갯수
    // 출력물이 모두 출력될 때 까지 수행
    while !tempPriorities.isEmpty { 
        // 현제 1순위 출력물이 긴급도가 최고가 아닌 경우
        if tempPriorities.max() != tempPriorities[0] {
          	// 해당 값을 배열 맨 뒤로 이동
            tempPriorities.append(tempPriorities[0])
        } else {
          	// 목표 출력물이고, 출력물 리스트에서 긴급도가 가장 높은경우
            if tempLocation == 0 && tempPriorities.max() == tempPriorities[0] {
              	// 출력물 갯수 추가 및 종료
                returnValue += 1
                return returnValue
            }
            returnValue += 1
        }
        // 첫번째 인자 제거, 목표값 인덱스 수정
        tempPriorities.removeFirst()
        tempLocation -= 1
        // 목표값 인덱스가 마지막으로 이동할 경우 인덱스 변경
        if tempLocation == -1 {
            tempLocation = tempPriorities.count - 1
        }
    }
    return returnValue
}
```

**\+Best Solution**

```swift
func solution(_ priorities:[Int], _ location:Int) -> Int {
    var cPriorities = priorities
    var targetIndex = location
    var seq = 0

    while cPriorities.count > 0 {
      	// 출력물 리스트에서 contains함수를 통해 값이 첫번째 값보다 긴급성이 큰 값이 배열에 있는지 확인
        if cPriorities.contains(where: { $0 > cPriorities[0] }) {
            let first = cPriorities.removeFirst()
            cPriorities.append(first)
          	// 타겟인덱스가 0보다 작아지면 남은 배열의 마지막 값 인덱스로 변경
            targetIndex = targetIndex - 1 < 0 ? cPriorities.count - 1 : targetIndex - 1
        } else {
            if(targetIndex == 0) {
              	// 초기 출력물 갯수 - 남아있는 출력물 갯수 + 1 => 나의 긴급한 출력물 출력 인덱스
              	return priorities.count - cPriorities.count + 1
            } 
            cPriorities.removeFirst()
            targetIndex = targetIndex - 1
        }
    }
    return 0
}
```


Review
-----------------
**\+ New Know**

> 1. contains 함수
>    - 배열 내에 where 뒤에 정의 된 조건에 만족하는 값이 있을 경우 true, 없을 경우 false
>
>
> ```swift
> func contains(where predicate: (Int) throws -> Bool) rethrows -> Bool
> func contains(where predicate: (Self.Element) throws -> Bool) rethrows -> Bool
> 
> // 예제
> let expenses = [21.37, 55.21, 9.32, 10.18, 388.77, 11.41]
> let hasBigPurchase = expenses.contains { $0 > 100 }
> // 'hasBigPurchase' == true
> ```
>

