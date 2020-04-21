문제 설명
--------

**\+문제 요약**

> 1부터 입력받은 숫자 n 사이에 있는 소수의 개수를 반환하는 함수, solution을 만들어 보세요. 
>
> 소수는 1과 자기 자신으로만 나누어지는 수를 의미합니다.
> (1은 소수가 아닙니다.)

**\+제한 조건**

> - n은 2이상 1000000이하의 자연수입니다.

**\+입출력 예**

| n    | result |
| ---- | ------ |
| 10   | 4      |
| 5    | 3      |

입출력 예 #1
1부터 10 사이의 소수는 [2,3,5,7] 4개가 존재하므로 4를 반환

입출력 예 #2
1부터 5 사이의 소수는 [2,3,5] 3개가 존재하므로 3를 반환

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/12921

문제 풀이
---------

**\+My Solution**

```swift
func solution(_ n:Int) -> Int {
    var sum:Int = 0
    let sosuArray:[Int] = [2,3,5,7]
    var resultArray = [Bool](repeating: true, count:n)
    
    if n < 10 { // n이 10 이하인 경우 처리
        return sosuArray.filter{ $0 < n+1 }.count
    }
    resultArray[0] = false   //1
   
    for sosuindex in 0..<resultArray.count {
        if resultArray[sosuindex] { // 소수 선택 
          	// 최고 1000000 단위임으로, 최대 1000의 제곱에서 끝남
            if sosuindex+1 > 1000 {
                break
            }
          	// 추출한 값으로부터, 최대 값까지 ture인 경우만 배수 체크
            for k in sosuindex+2..<n+1 {
                if resultArray[k-1] {
                  	// 배수인 경우 false 처리
                    if k.isMultiple(of: sosuindex+1) { resultArray[k-1] = false }
                }
            }// 배열에는 소수만 ture 형태로 남음
        }
    }
  	// 합계
    for i in resultArray {
        if i { sum += 1 }
    }
    return sum
}
```

**\+Best Solution**

```swift
func solution(_ n:Int) -> Int {
    var answer: [Int] = []
    var current: [Int] = Array(2...n)
    while !current.isEmpty {
        let index = current[0] //2로 초기값 시작
        answer.append(index)	
        current = current.filter { $0%index != 0 } // current에 2의 배수 제거 후 넣음
        if index > 1000 { 												 // 소수 검사 최대값 
            answer.append(contentsOf: current)
            break;
        }
    }
    return answer.count
}
```


Review
-----------------
**\+ New Know**

> 1. filter 활용 방법	
> 	- 조건 값에 해당되는 배열 값들의 배열로 리턴 
>
>
> ```swift
> func filter(_ isIncluded: (Int) throws -> Bool) rethrows -> [Int]
> ```
> 
> 2. 큰 배열의 효율적인 초기화 방법
> 	- for문과 같은 반복문보다 훨씬 빠르게 설정 가능
>
>  ```swift
> var bigArray = [Bool](repeating: true, count:n)
> var bigArray: [Int] = Array(2...n)
>  ```
>

