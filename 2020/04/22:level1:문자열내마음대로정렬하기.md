문제 설명
--------

**\+문제 요약**

> 문자열로 구성된 리스트 strings와, 정수 n이 주어졌을 때, 각 문자열의 인덱스 n번째 글자를 기준으로 오름차순 정렬하려 합니다. 예를 들어 strings가 [sun, bed, car]이고 n이 1이면 각 단어의 인덱스 1의 문자 u, e, a로 strings를 정렬합니다.

**\+제한 조건**

> - strings는 길이 1 이상, 50이하인 배열입니다.
> - strings의 원소는 소문자 알파벳으로 이루어져 있습니다.
> - strings의 원소는 길이 1 이상, 100이하인 문자열입니다.
> - 모든 strings의 원소의 길이는 n보다 큽니다.
> - 인덱스 1의 문자가 같은 문자열이 여럿 일 경우, 사전순으로 앞선 문자열이 앞쪽에 위치합니다.

**\+입출력 예**

| strings           | n    | return            |
| ----------------- | ---- | ----------------- |
| [sun, bed, car]   | 1    | [car, bed, sun]   |
| [abce, abcd, cdx] | 2    | [abcd, abce, cdx] |

##### 입출력 예 설명

**입출력 예 1**
sun, bed, car의 1번째 인덱스 값은 각각 u, e, a 입니다. 이를 기준으로 strings를 정렬하면 [car, bed, sun] 입니다.

**입출력 예 2**
abce와 abcd, cdx의 2번째 인덱스 값은 c, c, x입니다. 따라서 정렬 후에는 cdx가 가장 뒤에 위치합니다. abce와 abcd는 사전순으로 정렬하면 abcd가 우선하므로, 답은 [abcd, abce, cdx] 입니다.

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/12915

문제 풀이
---------

**\+My Solution**

```swift
func solution(_ strings:[String], _ n:Int) -> [String] {
    var tempString:[String] = strings.sorted() // 문자열을 사전순서로 정렬
    var indexArray:[String] = []
    
    for i in tempString {
        indexArray.append(String(Array(String(i))[n]))
    }
  	// 인덱스 기준 버블정렬 수행
    for i in 0..<strings.count{
        for j in 0..<strings.count-1 {
            if indexArray[j] > indexArray[j+1] {
                indexArray.swapAt(j, j+1)
                tempString.swapAt(j, j+1)
            }
        }
    }
    return tempString
}
```

**\+Best Solution**

```swift
func solution(_ strings:[String], _ n:Int) -> [String] {
    let answer: [String] = strings.sorted { // sorted 함수를 통한 정렬 
        // 값 2개 추출 후 인덱스(n)값 비교 후 순서 지정 
        let left: Character = $0[$0.index($0.startIndex, offsetBy: n)]
        let right: Character = $1[$1.index($1.startIndex, offsetBy: n)]

        if left == right {
            return $0 < $1
        } else {
            return left < right
        }
    }
    return answer
}
```


Review
-----------------
**\+ New Know**

> 1. index 활용 방법	
> 	- 문자열 값에 인덱스를 생성하고 offsetBy 값을 통해 특정 위치의 index값을 리턴
>
>
> ```swift
> // 함수 정의
> func index(_ i: Index, offsetBy n: IndexDistance) -> Index
> 
> //  사용 예제
> let s = "Swift"
> let i = s.index(s.startIndex, offsetBy: 4)
> print(s[i])  // Prints "t"
> ```
>
> 2. sorted { ~~ }
> 	- { } 를 통해서 사용자가 원하는 조건을 수동으로 설정 가능
>
>  ```swift
> func sorted(by areInIncreasingOrder: (String, String) throws -> Bool) rethrows -> [String]
> 
> // 전달 인자 2개 (String, String) 
> // true 일 경우 리턴값 [String]
>  ```
>

