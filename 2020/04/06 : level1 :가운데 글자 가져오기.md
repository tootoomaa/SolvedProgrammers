#### 문제 URL
https://programmers.co.kr/learn/courses/30/lessons/12903/solution_groups?language=swift

#### 문제 요약
> 단어 s의 가운데 글자를 반환하는 함수, solution을 만들어 보세요. 단어의 길이가 짝수라면 가운데 두글자를 반환하면 됩니다.

재한사항
> 입력값 s는 길이가 1 이상, 100이하인 스트링입니다.
 
입출력 예
s | return 
---|---
abcde | c
qwer | we


문제 풀이
---------

* My Solution
```swift
func solution(_ s:String) -> String {

    let countString = s.count
    var resultValue: String = ""
    let tempArray = Array(s)

    if countString%2 == 0 { // 짝수
        resultValue.append(tempArray[countString/2-1])
        resultValue.append(tempArray[countString/2])
        return resultValue
    } else { // 홀수
        return String(tempArray[(countString-1)/2])
    }
}

```

* Best Solution
```swift
func solution(_ s:String) -> String {
    return String(s[String.Index(encodedOffset: (s.count-1)/2)...String.Index(encodedOffset: s.count/2)])
}
```

Review
-----------------
\+ New Know
> 배열을 Array 형태로 변환 하지 않고 String.Index 를 통해서 string값에서 특정 위치의 character 추출

\+ Remember
>string 값은 배열처럼 \[] 을 사용하여 character값 추출 불가 아래 처럼 변환 하여 사용
> * 배열 -> String 변환 방법 : joined()
> * string -> 배열 변환 방법 : Array(s)
