문제 설명
--------

**\+문제 요약**

> 어떤 문장의 각 알파벳을 일정한 거리만큼 밀어서 다른 알파벳으로 바꾸는 암호화 방식을 시저 암호라고 합니다. 예를 들어 AB는 1만큼 밀면 BC가 되고, 3만큼 밀면 DE가 됩니다. z는 1만큼 밀면 a가 됩니다. 문자열 s와 거리 n을 입력받아 s를 n만큼 민 암호문을 만드는 함수, solution을 완성해 보세요.

**\+제한 조건**

> - 공백은 아무리 밀어도 공백입니다.
> - s는 알파벳 소문자, 대문자, 공백으로만 이루어져 있습니다.
> - s의 길이는 8000이하입니다.
> - n은 1 이상, 25이하인 자연수입니다.

**+ 출력 형식**

>  3번의 기회에서 얻은 점수 합계에 해당하는 정수값을 출력한다.
>  예) 37

**\+입출력 예**

| s     | n    | result |
| ----- | ---- | ------ |
| AB    | 1    | BC     |
| z     | 1    | a      |
| a B z | 4    | e F d  |

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/12926

문제 풀이
---------

**\+My Solution**

```swift
func solution(_ s:String, _ n:Int) -> String {
    var returnString:String = ""
    let matchingStringArray:[String] = ["a","b","c","d","e","f","g","h","i","j","k","l","m","n","o","p","q","r","s","t","u","v","w","x","y","z"]

    for char in s {
      	// 공백 처리
        if char == " " {
            returnString += " "
        } else {
           // char를 받아 n칸씩 이동하는 작업 수행
            for (k, i) in matchingStringArray.enumerated() {
                // 받아온 char의 대소문자 처리
                if i == String(char.lowercased()) {
                  	// n칸 이동을 위한 
                    var tempString = String(matchingStringArray[(k+n)%26])
                    returnString += char.isUppercase ? tempString.uppercased() : tempString
                }
            }
        }
    }
    return returnString
}
```

**\+Best Solution**

```swift
// 1. 고차함수를 이용한 문제풀이
func solution(_ s:String, _ n:Int) -> String {
    let alphabets = "abcdefghijklmnopqrstuvwxyz".map { $0 }
    return String(s.map {
      	//string 값의 글자를 $0 으로 받아서 alphanets상에서 해당 문자의 index값 설정
        guard let index = alphabets.firstIndex(of: Character($0.lowercased())) else { return $0 }
       // 설정한 index값에서 n만큼 뒤로 이동 한뒤 모듈려 연산 수행(n이 25이상일 경우 처리 방법)
        let letter = alphabets[(index + n) % alphabets.count]
      	// s에서 받은 $0이 대문자일 경우 letter도 변환
        return $0.isUppercase ? Character(letter.uppercased()) : letter
    })
}

//----------------------------------------------
//2 . 비트 연산을 이용한 문제풀이
func solution(_ s:String, _ n:Int) -> String {
    var answer:[UInt8] = []
  	// 주어진 문자열 s를 uft8로 인코딩 한뒤 enumerated 수행
    for (index, element) in s.utf8.enumerated() {
      	// 변환한 글자 element에 n을 더함
        let value = element+UInt8(n)
        switch element {
            case 65...90:  // A->Z 65~90
                answer.append(value > 90 ? value-26 : value)
            case 97...122: // a->z 97~122
                answer.append(value > 122 ? value-26 : value)
            default:
                answer.append(element)
        }
    }
    return String(data:Data(answer), encoding:.utf8)!
}

```


Review
-----------------
**\+ New Know**

> 1. utf8 : 대상을 utf8 로 인코딩하여 변환해줌
>
>
> ```swift
> var utf8: UTF8View { get set }
> ```
>   2. map함수를 이용한 배열 생성 방법
> 
>  ```swift
> let alphabets = "abcdefghijklmnopqrstuvwxyz".map { $0 }
> print(alphabets) 
> //["a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z"]
>  ```
> 

