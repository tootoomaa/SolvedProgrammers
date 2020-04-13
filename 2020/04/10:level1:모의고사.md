문제 설명
--------

**\+문제 요약**
> 수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.
> 1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
> 2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
> 3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...
> 1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

**\+재한사항**
> 시험은 최대 10,000 문제로 구성되어있습니다.
> 문제의 정답은 1, 2, 3, 4, 5중 하나입니다.
> 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.
 
**\+입출력 예**
 answer | return 
---|---
[1,2,3,4,5] | [1] 
[1,3,2,4,2] | [1,2,3]

**\+문제 URL**
>https://programmers.co.kr/learn/courses/30/lessons/42840#



문제 풀이
---------

* \+My Solution**
```swift
func solution(_ answers:[Int]) -> [Int] {
    let persion = [[1, 2, 3, 4, 5],                 // persion 1
                   [2, 1, 2, 3, 2, 4, 2, 5],        // pserson 2 
                   [3, 3, 1, 1, 2, 2, 4, 4, 5, 5]]  // persion 3
    var correntCount = [0,0,0]
    var result = [Int]()
    
    for n in 0..<3 {
        for i in 0..<answers.count {
            if answers[i] == persion[n][i%persion[n].count] {
                correntCount[n] += 1
            }
        }
    }
    let maxcount = correntCount.max()!
    for k in 0..<3 {
        if correntCount[k] == maxcount {
            result.append(k+1)
        }
        
    }
    return result
}
```

**\+Best Solution**
```swift
func solution(_ answers:[Int]) -> [Int] {
    let answer = (
        a: [1, 2, 3, 4, 5], // index % 5
        b: [2, 1, 2, 3, 2, 4, 2, 5], // index % 8
        c: [3, 3, 1, 1, 2, 2, 4, 4, 5, 5] // index % 10
    )
    var point = [1:0, 2:0, 3:0]

    for (i, v) in answers.enumerated() {
        if v == answer.a[i % 5] { point[1] = point[1]! + 1 }
        if v == answer.b[i % 8] { point[2] = point[2]! + 1 }
        if v == answer.c[i % 10] { point[3] = point[3]! + 1 }
    }

    return point.sorted{ $0.key < $1.key }.filter{ $0.value == point.values.max() }.map{ $0.key }
}
```

Review
-----------------
**\+ New Know**
> 1. Best Solution의 for, if를 활용하여 가독성 있는 코드 작성 방법  
```swift
for (i, v) in answers.enumerated() {
    if v == answer.a[i % 5] { point[1] = point[1]! + 1 }
    if v == answer.b[i % 8] { point[2] = point[2]! + 1 }
    if v == answer.c[i % 10] { point[3] = point[3]! + 1 }
}
```
> 2.  enumerated() 인스턴스 메소드
>   -  (n, x) 쌍의 순서를 돌려줍니다. n은 제로로부터 시작되는 연속 한 정수를 나타내고, x는 시퀀스의 요소를 나타냅니다.
```swift
 for (n, c) in "Swift".enumerated() {
     print("\(n): '\(c)'")
 }
 // Prints "0: 'S'"
 // Prints "1: 'w'"
 // Prints "2: 'i'"
 // Prints "3: 'f'"
 // Prints "4: 't'"
 ```
 [In apple document](https://developer.apple.com/documentation/realitykit/entity/childcollection/3243998-enumerated)

  
**\+ Remember or TO-BE**
> 1. 단순 if, for 문이 아닌 dictionary같은 구문도 사용을 고려해
> 2. 이중 배열로 작성했을때, 딕셔너리로 작성할때보다 확장성이 떨어지는듯
> 3. 코드 작성 시 다른 사람이 봤을 때 명학하게 알 수 있도록 설계
