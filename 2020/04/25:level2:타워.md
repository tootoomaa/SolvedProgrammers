문제 설명
--------

**\+문제 요약**

> 수평 직선에 탑 N대를 세웠습니다. 모든 탑의 꼭대기에는 신호를 송/수신하는 장치를 설치했습니다. 발사한 신호는 신호를 보낸 탑보다 높은 탑에서만 수신합니다. 또한, 한 번 수신된 신호는 다른 탑으로 송신되지 않습니다.
>
> 예를 들어 높이가 6, 9, 5, 7, 4인 다섯 탑이 왼쪽으로 동시에 레이저 신호를 발사합니다. 그러면, 탑은 다음과 같이 신호를 주고받습니다. 높이가 4인 다섯 번째 탑에서 발사한 신호는 높이가 7인 네 번째 탑이 수신하고, 높이가 7인 네 번째 탑의 신호는 높이가 9인 두 번째 탑이, 높이가 5인 세 번째 탑의 신호도 높이가 9인 두 번째 탑이 수신합니다. 높이가 9인 두 번째 탑과 높이가 6인 첫 번째 탑이 보낸 레이저 신호는 어떤 탑에서도 수신할 수 없습니다.
>
> | 송신 탑(높이) | 수신 탑(높이) |
> | ------------- | ------------- |
> | 5(4)          | 4(7)          |
> | 4(7)          | 2(9)          |
> | 3(5)          | 2(9)          |
> | 2(9)          | -             |
> | 1(6)          | -             |
>
> 맨 왼쪽부터 순서대로 탑의 높이를 담은 배열 heights가 매개변수로 주어질 때 각 탑이 쏜 신호를 어느 탑에서 받았는지 기록한 배열을 return 하도록 solution 함수를 작성해주세요.



**\+제한 조건**

> - heights는 길이 2 이상 100 이하인 정수 배열입니다.
> - 모든 탑의 높이는 1 이상 100 이하입니다.
> - 신호를 수신하는 탑이 없으면 0으로 표시합니다.

**\+입출력 예**

| heights         | return          |
| --------------- | --------------- |
| [6,9,5,7,4]     | [0,0,2,2,4]     |
| [3,9,9,3,5,7,2] | [0,0,0,3,3,3,6] |
| [1,5,3,6,7,6,5] | [0,0,2,0,0,5,6] |



##### 입출력 예 설명

**입출력 예 1**
**sun, bed, car의 1번째 인덱스 값은 각각 u, e, a 입니다. 이를 기준으로 strings를 정렬하면 [car, bed, sun] 입니다.**

**입출력 예 2**
**abce와 abcd, cdx의 2번째 인덱스 값은 c, c, x입니다. 따라서 정렬 후에는 cdx가 가장 뒤에 위치합니다. abce와 abcd는 사전순으로 정렬하면 abcd가 우선하므로, 답은 [abcd, abce, cdx] 입니다.**

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/42588

문제 풀이
---------

**\+My Solution**

```swift
func solution(_ heights:[Int]) -> [Int] {
    var returnArray:[Int] = []
    var tempArray:[Int] = heights.reversed()
    
    for sendTower in heights.reversed() {
        tempArray.removeFirst()
        if tempArray.isEmpty {
            returnArray.append(0)
            return returnArray.reversed()
        }
        var myIndex:Int = 0
        for recivetower in tempArray {
            myIndex += 1
            if recivetower > sendTower {
                if tempArray.filter({ $0 == recivetower }).count > 1 {
                    returnArray.append(heights.lastIndex(of: recivetower)!+1)
                } else {
                    returnArray.append(heights.firstIndex(of: recivetower)!+1)
                }
                break
            } else if myIndex == tempArray.count {
                returnArray.append(0)
            }
        }
    }
    return returnArray.reversed()
}
```

**\+Best Solution**

```swift
func solution(_ heights:[Int]) -> [Int] {
    return heights.enumerated().map({ (index, height ) -> Int in
        if index == 0 { return 0 }
        var targetIndex = 0
        let leftArr = Array(heights[0..<index]).reversed()
        for (leftIndex, leftItem) in leftArr.enumerated() {
            if(leftItem > height) {
                targetIndex = index - leftIndex
                break
            }
        }
        return targetIndex
    })
}
```


Review
-----------------
**\+ New Know**

> 1. for문 작성시 문자열을 기준으로 비교할 것인지, 순차적으로 비교할 것인지를 논리적으로 생각해보고 효율적인 방식을 선택해서 작성하기
>
>
> ```swift
> for i in Array
> 
> for i in 0..<Array.count
> ```
>
