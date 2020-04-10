문제 설명
--------

\+문제 요약
> N x N 크기의 정사각 격자이며 위쪽에는 크레인이 있고 오른쪽에는 바구니가 있습니다. 게임 화면의 격자의 상태가 담긴 2차원 배열 board와 인형을 집기 위해 크레인을 작동시킨 위치가 담긴 배열 moves가 매개변수로 주어질 때, 크레인을 모두 작동시킨 후 터트려져 사라진 인형의 개수를 return 하도록 solution 함수를 완성해주세요.

\+재한사항
>board 배열은 2차원 배열로 크기는 5 x 5 이상 30 x 30 이하입니다.
>board의 각 칸에는 0 이상 100 이하인 정수가 담겨있습니다.
>0은 빈 칸을 나타냅니다.
>1 ~ 100의 각 숫자는 각기 다른 인형의 모양을 의미하며 같은 숫자는 같은 모양의 인형을 나타냅니다.
>moves 배열의 크기는 1 이상 1,000 이하입니다.
>moves 배열 각 원소들의 값은 1 이상이며 board 배열의 가로 크기 이하인 자연수입니다.
 
\+입출력 예
 board | moves | result 
---|---|---
[[0,0,0,0,0],[0,0,1,0,3],[0,2,5,0,1],[4,2,4,4,2],[3,5,1,3,1]] | [1,5,3,5,1,2,1,4] | 4

\+문제 URL
>https://programmers.co.kr/learn/courses/30/lessons/64061



문제 풀이
---------

*** *My Solution**
```swift
import Foundation

func solution(_ board:[[Int]], _ moves:[Int]) -> Int {
    
    let boardSize = board[0].count
    var tempBoard = board
    var pickupValue: Int
    var resultArray = [Int]()
    var resultValue: Int = 0
    
    for i in moves {
        for k in 0..<boardSize {
            if tempBoard[k][i-1] != 0 {
                pickupValue = tempBoard[k][i-1] // 추출할 값 선택
                tempBoard[k][i-1] = 0
                if resultArray.count == 0 {  //마지막 배열에 값이 하나만 있는 경우
                    resultArray.append(pickupValue)
                } else { // 배열에 기존 값이 있는 경우
                    let beforeValue = resultArray.popLast()
                    if beforeValue != pickupValue {
                        resultArray.append(beforeValue!)
                        resultArray.append(pickupValue)
                    } else {
                        resultValue += 2
                    }
                }
                break
            }
        }
    } 
    return resultValue
}


```

** *Best Solution**
```swift
import Foundation

func solution(_ board:[[Int]], _ moves:[Int]) -> Int {
    var count = 0
    var stacks: [[Int]] = Array(repeating: [], count: board.count)
    var bucket: [Int] = []

    board.reversed().forEach {
        $0.enumerated().forEach {
            if $0.1 != 0 {
                stacks[$0.0].append($0.1)
            }
        }
    }

    moves.forEach {
        if let doll = stacks[$0-1].popLast() {
            if let last = bucket.last, last == doll {
                bucket.removeLast(1)
                count += 2
            } else {
                bucket.append(doll)
            }
        }
    }

    return count
}
```

Review
-----------------
**\+ New Know**> 1. 배열에서  popLast()과 last의 차이점
>   - popLast() : 추출 및 배열에서 값 제거
>   - last : 배열의 마지막 값 선택 (배열에는 값 유지)

**\+ Remember or TO-BE**
>string 값은 배열처럼 \[] 을 사용하여 character값 추출 불가 아래 처럼 변환 하여 사용
> * 배열 -> String 변환 방법 : joined()
> * string -> 배열 변환 방법 : Array(s)
