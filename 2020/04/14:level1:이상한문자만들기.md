문제 설명
--------

**\+문제 요약**

> 문자열 s는 한 개 이상의 단어로 구성되어 있습니다. 각 단어는 하나 이상의 공백문자로 구분되어 있습니다. 각 단어의 짝수번째 알파벳은 대문자로, 홀수번째 알파벳은 소문자로 바꾼 문자열을 리턴하는 함수, solution을 완성하세요.

**\+재한사항**
> - 문자열 전체의 짝/홀수 인덱스가 아니라, 단어(공백을 기준)별로 짝/홀수 인덱스를 판단해야합니다.
> - 첫 번째 글자는 0번째 인덱스로 보아 짝수번째 알파벳으로 처리해야 합니다.

**\+입출력 예**

s | result 
---|---
"try hello world" | "TrY HeLlO WoRiD" 

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/12930



문제 풀이
---------

**\+My Solution**

```swift
// 실패 1 - split을 사용하여 중복된 공백을 재대로 처리하지 못함
// " "로 분리된 문자열 마다 짝수는 대문자, 홀수는 소문자를 재대로 이해 못함
// " "로 분리된 문자열 마다 짝수는 대문자, 홀수는 소문자를 재대로 이해 못함
func solution3(_ s:String) -> String {
    let resultArray = s.split(separator: " ") // 공백을 기준으로 s 스트링 분할
    var resultString:String = ""
    
    for i in 0..<resultArray.count {
        let tempArray = Array(resultArray[i]) // 분할된 스트링 값 선택
        for k in 0..<resultArray[i].count {		// 분할된 스트링 순차적으로 대문자 적용
            let changeCharactor:Character = tempArray[k]
            if k % 2 == 0 {
                resultString += changeCharactor.uppercased()
            }else {
                resultString += changeCharactor.lowercased()
            }
        }
        if i != resultArray.count-1 {
            resultString += " " // 단어와 단어 사이에 " "값 넣어 공백 채움
        }
    }
    return resultString
}
solution("sp ace")		 // "Sp AcE" 처리 가능
solution("sp     ace") // "Sp AcE" 문제 발생!!! // 정답 "Sp    AcE"
```

```swift
// 성공 - 공백 처리를 위해서 myindex 추가 설정 
func solution(_ s:String) -> String {
    var tempArray = Array(s)
    var resultString:String = ""
  
    var myindex:Int = 0
    for i in 0..<tempArray.count {
        if tempArray[i] == " " {	//단어 사이에 " " 공백을 만날 경우
            myindex = 0						// myindex 초기화
            resultString += " "		// 공백 넣어주기
        }else if myindex % 2 == 0 {	// myindex를 통해서 각 단어별 대.소문자 변경
            resultString += tempArray[i].uppercased()
            myindex += 1						// 다음글자 선택
        }else {
            resultString += tempArray[i].lowercased()
            myindex += 1
        }
    }
    return resultString
}
```

**\+Best Solution**

```swift
// 우와.....
import Foundation
func solution(_ s:String) -> String {
    let a = s.components(separatedBy: " ").map { $0.enumerated().map { 				     $0.offset % 2 == 0 ? $0.element.uppercased() : $0.element.lowercased() } } 
	  // enumerated() 시킨후 map
    // offset으로 map에서 추출한 시퀀스의 몇번째 수인지 확인 가능
    return a.map{ $0.map { $0 }.joined() }.joined(separator: " ")
  	//array -> string joined() 함수
}
```
```swift
// 가독성! map
func solution(_ s:String) -> String {
    return String(s).components(separatedBy: " ").map {
        var result = ""
        for i in 0..<$0.count {  // offset 대신 for문 사용
            let char = String($0[String.Index(encodedOffset: i)])
            result += (i % 2 == 0) ? char.uppercased() : char.lowercased()
        }
        return result
    }.joined(separator: " ")
}
```


Review
-----------------
**\+ New Know**

> 1. map 중복사용 가능
>
> 2. offset 함수  : 시퀀스 상에서 해당 값의 몇번째 수인지
>
> 3. joined 함수 : array 형태의 문자를 String으로 변환
>
>    ```swift
>    var arr = ["A","b","c","d","e"]
>    var temp = arr.joined()
>    type(of: arr)			// String.type
>    type(of: temp)		// "Abcde\n"
>    ```
>
> 4. 대소문자 변경
>
>    ```swift
>    // character형에만 적용 가능
>    var char = "a"
>    char.uppercased() // "A" 
>    char.lowercased() // "a"
>    ```

**\+ Remember or TO-BE**

> 문제를 잘 읽어보자 ㅠㅠ
