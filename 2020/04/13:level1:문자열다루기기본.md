문제 설명
--------

**\+문제 요약**

> 문자열 s의 길이가 4 혹은 6이고, 숫자로만 구성돼있는지 확인해주는 함수, solution을 완성하세요. 예를 들어 s가 "a234"이면 False를 리턴하고 "1234"라면 True를 리턴하면 됩니다.

**\+재한사항**
> - `s`는 길이 1 이상, 길이 8 이하인 문자열입니다.

**\+입출력 예**

s |  return
---|---
"a234" | false
"1234" | true

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/12918



문제 풀이
---------

**\+My Solution**

```swift
func solution(_ s:String) -> Bool {
    if s.count != 4 && s.count != 6 {
        return false
    }
    if Int(s) == nil {
        return false
    }
    return true
}
```

**\+Best Solution**

```swift
func solution(_ s:String) -> Bool {
    return (Int(s) != nil && (s.count == 4 || s.count == 6)) ? true : false
}
```

Review
-----------------
**\+ New Know**

> 1. Int()로 형변환은 string값만 가능, character 값은 Int(String(  )) 을 사용하여 진행
>    - array 배열의 n번째 부터 k번째 까지 선택
> 2. 형변환 실패시 **nil** 값 반환

**\+ Remember or TO-BE**

> 1. 3항 연산자 사용 고려
> 2. 코드를 최소화 하도록 구성
