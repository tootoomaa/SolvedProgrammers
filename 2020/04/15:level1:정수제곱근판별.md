문제 설명
--------

**\+문제 요약**

> 임의의 양의 정수 n에 대해, n이 어떤 양의 정수 x의 제곱인지 아닌지 판단하려 합니다.
> n이 양의 정수 x의 제곱이라면 x+1의 제곱을 리턴하고, n이 양의 정수 x의 제곱이 아니라면 -1을 리턴하는 함수를 완성하세요.

**\+재한사항**
> - n은 1이상, 50000000000000 이하인 양의 정수입니다.

**\+입출력 예**

n | result 
---|---
121 | 144 
3 | -1 

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/12934



문제 풀이
---------

**\+My Solution**

```swift
func solution(_ n:Int64) -> Int64 {
    // 입력값의 최대 크기가 50000000000000임으로 for문의 효율을 고려할 필요가 있다고 생각함
  	// 제곱근의 특성상 100 * 100 = 10000  2자리를 제곱할 경우 4자리로 나옴
  	// 따라서 결과값의 자리수를 구하고, 최대 자리수를 구한 뒤 for문 수행
    let numberCount = Array(String(n)).count
    var maxInt = 1
    
    if n == 1 {
        return 4
    }else {
        for k in 1..<numberCount {
            maxInt *= 10
        }
        for i in 1..<maxInt {
            if Int(n)/i == i && Int(n)%i == 0 {
                 return Int64((i+1)*(i+1))
            }
        }
        return -1
    }
}
```

**\+Best Solution**

```swift
import Foundation
func solution(_ n:Int64) -> Int64 {
  	//sqrt 메소드를 이용한 루트 적용
    let t = Int64(sqrt(Double(n)))
    return t * t == n ? (t+1)*(t+1) : -1
}
```


Review
-----------------
**\+ New Know**

> 1. sqrt 메소드
>
>    ```swift
>    import Foundation // 필요
>    var temp:Double = 100
>    sqrt(temp) = 10.0
>    ```

**\+ Remember or TO-BE**

> 명심하자. 정답은 같더라도 실행 효율성은 다르다.
>
> 함수를 찾아보자 ㅠㅠ



### Performance Compare

- Best code testcase

TEST | Best | MY | Worst 
---|---|---|---|---
 테스트 1 〉  | 통과 (0.10ms, 24.1MB) | 통과 (64.94ms, 24.3MB) |통과 (1748.20ms, 24.1MB)

- Worst 코드


```swift

func solution(_ n:Int64) -> Int64 {
    if(n == 1) {
        return 4
    }

    for i in 1..<n {
        if(n == i * i) {
            return (i + 1) * (i + 1)
        }
    }
    return -1
}

```

