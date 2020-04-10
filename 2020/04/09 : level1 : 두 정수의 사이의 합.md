문제 설명
--------

\+문제 요약
> 두 정수 a, b가 주어졌을 때 a와 b 사이에 속한 모든 정수의 합을 리턴하는 함수, solution을 완성하세요. 예를 들어 a = 3, b = 5인 경우, 3 + 4 + 5 = 12이므로 12를 리턴합니다.

\+재한사항
> a와 b가 같은 경우는 둘 중 아무 수나 리턴하세요.
>a와 b는 -10,000,000 이상 10,000,000 이하인 정수입니다.
>a와 b의 대소관계는 정해져있지 않습니다.
 
\+입출력 예
 a | b | result 
---|---|---
3 | 5 | 12
3 | 3 | 3
5 | 3 | 12

\+문제 URL
>https://programmers.co.kr/learn/courses/30/lessons/12912



문제 풀이
---------

*** *My Solution**
```swift
func solution(_ a:Int, _ b:Int) -> Int64 {

    var sumValue:Int = 0

    if a == b {
        sumValue = a
    } else if b > a {
        for a in a...b {
            sumValue += a
        }
    } else if a > b {
        for b in b...a {
            sumValue += b
        }
    }
    return Int64(sumValue)
}
```

** *Best Solution**
```swift
func solution(_ a:Int, _ b:Int) -> Int64 {
  return Int64(Array(a > b ? b...a : a...b).reduce(0, +))
}
```

Review
-----------------
**\+ New Know**
>_ reduce 함수 사용 방법 

Summary
   Returns the result of combining the elements of the sequence using the given closure.
   
Declaration
```swift
func reduce<Result>(_ initialResult: Result, _ nextPartialResult: (Result, Int) throws -> Result)rethrows -> Result
```

For example
```swift
let numbers = [1, 2, 3, 4]
let numberSum = numbers.reduce(0, { x, y in
    x + y
})
  // numberSum == 10
``` 
  
**\+ Remember or TO-BE**
> 최대한 축약할 수 있는 방안은 찾아보기
> 3항 연산자를 통한 최대,최소값 구한 뒤 for문 변수 대입
```swift
func solution(_ a:Int, _ b:Int) -> Int64 {
    
    var sumValue:Int = 0
    var minValue:Int = a < b ? a : b
    var maxValue:Int = a > b ? a : b

    if a == b {
        sumValue = a
    } else {
        for i in minValue...maxValue {
            sumValue += i
        }
    }
    return Int64(sumValue)
}
```
