문제 설명
--------

**\+문제 요약**

> 슈퍼 게임 개발자 오렐리는 큰 고민에 빠졌다. 그녀가 만든 프랜즈 오천성이 대성공을 거뒀지만, 요즘 신규 사용자의 수가 급감한 것이다. 원인은 신규 사용자와 기존 사용자 사이에 스테이지 차이가 너무 큰 것이 문제였다.
> 
> 이 문제를 어떻게 할까 고민 한 그녀는 동적으로 게임 시간을 늘려서 난이도를 조절하기로 했다. 역시 슈퍼 개발자라 대부분의 로직은 쉽게 구현했지만, 실패율을 구하는 부분에서 위기에 빠지고 말았다. 오렐리를 위해 실패율을 구하는 코드를 완성하라.
> 
> 실패율은 다음과 같이 정의한다.
스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수
전체 스테이지의 개수 N, 게임을 이용하는 사용자가 현재 멈춰있는 스테이지의 번호가 담긴 배열 stages가 매개변수로 주어질 때, 실패율이 높은 스테이지부터 내림차순으로 스테이지의 번호가 담겨있는 배열을 return 하도록 solution 함수를 완성하라.
>
> **문제가 복잡하여 해당 링크로 직접 접속 권장**

**\+재한사항**

> - 스테이지의 개수 N은 `1` 이상 `500` 이하의 자연수이다.
>
> - stages의 길이는 `1` 이상 `200,000` 이하이다.
>
> - stages에는 `1`이상 `N+1` 이하의 자연수가 담겨있다.
>
> - 각 자연수는 사용자가 현재 도전 중인 스테이지의 번호를 나타낸다.
> - 단, `N + 1` 은 마지막 스테이지(N 번째 스테이지) 까지 클리어 한 사용자를 나타낸다.
> - 만약 실패율이 같은 스테이지가 있다면 작은 번호의 스테이지가 먼저 오도록 하면 된다.
> - 스테이지에 도달한 유저가 없는 경우 해당 스테이지의 실패율은 `0` 으로 정의한다.

**+ 출력 형식**

>  원래의 비밀지도를 해독하여 `'#'`, `공백`으로 구성된 문자열 배열로 출력하라.

**\+입출력 예**

N | stages | result
---|---|---
 5 | [2,1,2,6,2,4,3,3] |[3,4,2,1,5]
4 | [4,4,4,4,4] |[4,1,2,3]
**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/42889

문제 풀이
---------

**\+My Solution**

```swift
func solution(_ N:Int, _ stages:[Int]) -> [Int] {
    var tempArray:[Double] = []
    var tempArray1:[Double] = []
    var solvedPeopleCount:Double = 0
    var resultArray:[Int] = []
    //배열 0으로 초기화
    for _ in 0..<N {
        tempArray.append(0)
    }
    //각 스테이지별 통과하지 못한 사용자 수 카운트 하여 tempArray에 저장
    for k in stages {
        if k != N + 1 {
            tempArray[k-1] += 1
        }
    }
    // 문제별 통화하지 못한 사용자가 담긴 배열을 순차적으로 돌면서
  	// 각 문제별 실패율을 계산하여 tempArray1에 저장
    solvedPeopleCount = Double(stages.count)
    for k in tempArray {
        if k == 0 || solvedPeopleCount == 0 {
            tempArray1.append(0.0)
        } else {
            tempArray1.append(Double(k)/solvedPeopleCount)
            solvedPeopleCount -= k
        }
    }
    //실패율(확률)이 담긴 tempArray1을 순차적으로 돌면서 
  	//확률이 큰 순서대로 resultArray에 입력
    for _ in 0..<N {
        let maxValueIndex:Int = tempArray1.firstIndex(of: tempArray1.max()!)!
        resultArray.append(maxValueIndex+1)
        tempArray1[maxValueIndex] = -1
    }
    return resultArray
}
```

**\+Best Solution**

```swift
func solution(_ N:Int, _ stages:[Int]) -> [Int] {
    var stageNonClears = [0]
    var failRate: [Int: Double] = [:] // 딕셔너리
  	// lazy와 filter를 사용하여 스테이지별(1,2,3,4,...) 오답자 총합 후
  	// stageNonClears 에 저장
    (1...N+1).forEach { i in
        stageNonClears.append(stages.lazy.filter{$0==i}.count)
    }
		// 각 스테이지 넘버당 실패율을 설정
    for stageNum in 1...N {
        // 해당스테이지를 통과한 모든 사용자 합계 n
        let n = stageNonClears[stageNum..<stageNonClears.endIndex].reduce(0, +)
        if n == 0 { // 아무도 없는 스테이지 번호는 -1 설정
            failRate[stageNum] = -1
        } else {	// 나머지 스테이지는 확률 구하여 저장
            failRate[stageNum] = Double(stageNonClears[stageNum])/Double(stageNonClears[stageNum..<stageNonClears.endIndex].reduce(0, +))
        }
    }
    // 클로져를 통한 결과 값 정렬
    return failRate.keys.lazy.sorted { (a, b) -> Bool in
        if failRate[a]! > failRate[b]! {
            return true
        } else if failRate[a]! == failRate[b]! {
            return a < b
        } else {
            return false
        }
    }
}
```


Review
-----------------
**\+ New Know**

> 1. lazy 
>    - 아주 큰 배열을 map, filter 등과 함께 사용시, 해당 배열에 값이 미처 생성되지 않았음에도 map, filter를 처리하게 될때  nil값을 처리하는게 아니라 실제 값이 들어오면 처리하도록 지연시켜 줌	
>   2. 클로저 사용 익히기
>
>  ```swift
> return failRate.keys.lazy.sorted { (a, b) -> Bool in
>         if failRate[a]! > failRate[b]! {
>             return true
>         } else if failRate[a]! == failRate[b]! {
>             return a < b
>         } else {
>             return false
>         }
>     }
>  ```
>
>
>     3. 가독성 (함수이름 등)
>        - 이름만 보고도 어떤 함수인지, 변수인지 짐작할 수 있도록 작성! (tempArray등 사용 지양)
>
> ```swift
> stageNonClears// 스테이지별 도달한 플레이어 수 (해결 x) 
> failRate 			// 각 스테이지별 실패율
> ```


   **\+ Remember or TO-BE**
>1. 문제를 제대로 이해하지 않아 문제 풀이가 많이 지연됨
>
>2. 문제를 이해하고, 코드를 구현하기 전에 설계를 꼭!!! 시간을 많이 투자하자.
>   - 중간에 설계라고 하기보다는 문제를 완전히 이해하고 재 작성했을때, 코드의양이 절반정도 줄었음...
>3. 클로저와 map, filter, reduce에 대한 확인 필요
>
>