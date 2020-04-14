문제 설명
--------

**\+문제 요약**

> 자연수 N이 주어지면, N의 각 자릿수의 합을 구해서 return 하는 solution 함수를 만들어 주세요.
> 예를들어 N = 123이면 1 + 2 + 3 = 6을 return 하면 됩니다.

**\+재한사항**
> - N의 범위 : 100,000,000 이하의 자연수

**\+입출력 예**

n | result 
---|---
123 | 6 
987 | 24 

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/12931



문제 풀이
---------

**\+My Solution**

```swift
// 형 변환을 사용하여 문제 풀이
func solution(_ n:Int) -> Int
{
    var answer:Int = 0

    for i in String(n) {					// n 값을 String형으로 바꿔 값 추출
        answer += Int(String(i))! // 추출한 값을 String, Int 변환 후 더하기
    }															// 옵셔널 설정 필수
    return answer
}
```

**\+Best Solution**

```swift
//1. reduce 를 이용한 문제풀이
func solution(_ n:Int) -> Int
{
    return String(n).reduce(0, {$0+Int(String($1))!});
}
```
```swift
//2. 수학적 원리를 이용한 문제풀이
func solution(_ n:Int) -> Int 
{
    var answer:Int = 0
    var val = n
    while val > 0 {						// while문을 통한 val를 처리
        answer += val % 10		// 정수를 10으로 나눈 나머지 = 1의 자리수	
        val /= 10							// 10으로 나눈 몫 부분을 다음 입력 값으로 저장
    }
    return answer
}
```
```swift
//3. compactMap을 이용한 문제풀이
func solution(_ n:Int) -> Int
{
    var answer:Int = 0
  
    let arr = String(n).compactMap { Int(String($0)) }	//[1,2,3,4,5]
    answer = arr.reduce(0, +)					// 0은 초기값, +는 클로져
																			// 0 을 100으로 변경시 최종 값 115
    return answer
}
solution(12345)		// 15
```

Review
-----------------
**\+ New Know**

> 1. reduce
>
>    ```swift
>    // Summary
>    // - 주어진 클로저를 통해 시퀀스의 요소와 결합(처리)한 결과를 리턴
>    
>    // Declaration
>    func reduce<Result>(_ initialResult: Result, _ nextPartialResult: (Result, Int) throws -> Result) rethrows -> Result
>    
>    // reduce 예제
>    let numbers = [1, 2, 3, 4]
>    let numberSum = numbers.reduce(0, { x, y in
>        x + y
>    })
>    // numberSum == 10
>    ```
>
>    
>
> 2. compacMap
>
>    ```swift
>    // Summary
>    // 시퀀스의 요소마다 주어진 변환식을 적용하고, 해당 결과가 non-nil인 경우 리턴 
>    
>    // compacMap function
>    func compactMap<ElementOfResult>(_ transform: (Character) throws -> ElementOfResult?) rethrows -> [ElementOfResult]
>    
>    // compacMap 예제
>    let possibleNumbers = ["1", "2", "three", "///4///", "5"]
>    let mapped: [Int?] = possibleNumbers.map { str in Int(str) } 
>    // [1, 2, nil, nil, 5] // Int(str) 실패 시 nil 반환
>    let compactMapped: [Int] = possibleNumbers.compactMap { str in Int(str) } 
>    // [1, 2, 5]
>    ```

**\+ Remember or TO-BE**

> 문제 풀이도 중요하지만 다양한 알고리즘 생각해보기
