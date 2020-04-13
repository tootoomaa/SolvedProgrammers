문제 설명
--------

**\+문제 요약**

> 길이가 n이고, 수박수박수박수....와 같은 패턴을 유지하는 문자열을 리턴하는 함수, solution을 완성하세요. 
> 예를들어 n이 4이면 수박수박을 리턴하고 3이라면 수박수를 리턴하면 됩니다.

**\+재한사항**
> - n은 길이 10,000이하인 자연수입니다.

**\+입출력 예**

n | result 
---|---
3 | "수박수" 
4 | "수박수박" 

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/12922



문제 풀이
---------

**\+My Solution**

```swift
func solution(_ n:Int) -> String {
    var result:String = ""
    for i in 0..<n {
        if i%2 == 1 {
            result += "박"
        } else {
            result += "수"
        }
    }
    return result
}
```

**\+Best Solution**

```swift
func solution(_ n:Int) -> String {
    return "\(String(repeating: "수박", count: n / 2))\(n % 2 == 0 ? "" : "수")"
} // 짝수만큼 "수박"을 n/2번 반복하고, n 이 홀수면 "수" 붙혀준다
```

Review
-----------------
**\+ New Know**

> 1. return "    " 안에 String을 통해 바로 문자열을 생성 후 반환 하는 방법
>
> 2. repeating 함수
>
>    ```swift
>    let s = String(repeating: "ab", count: 10)
>    print(s)
>    // Prints "abababababababababab"
>    ```

**\+ Remember or TO-BE**

> 1. 익숙한 방법이 아닌 다른 방법으로 풀려고 노력해보기
