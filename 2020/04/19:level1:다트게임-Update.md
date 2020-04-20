문제 설명
--------

**\+문제 요약**

> 카카오톡 게임별의 하반기 신규 서비스로 다트 게임을 출시하기로 했다. 다트 게임은 다트판에 다트를 세 차례 던져 그 점수의 합계로 실력을 겨루는 게임으로, 모두가 간단히 즐길 수 있다.
> 갓 입사한 무지는 코딩 실력을 인정받아 게임의 핵심 부분인 점수 계산 로직을 맡게 되었다. 다트 게임의 점수 계산 로직은 아래와 같다.
>
> 1. 다트 게임은 총 3번의 기회로 구성된다.
> 2. 각 기회마다 얻을 수 있는 점수는 0점에서 10점까지이다.
> 3. 점수와 함께 Single(`S`), Double(`D`), Triple(`T`) 영역이 존재하고 각 영역 당첨 시 점수에서 1제곱, 2제곱, 3제곱 (점수1 , 점수2 , 점수3 )으로 계산된다.
> 4. 옵션으로 스타상(`*`) , 아차상(`#`)이 존재하며 스타상(`*`) 당첨 시 해당 점수와 바로 전에 얻은 점수를 각 2배로 만든다. 아차상(`#`) 당첨 시 해당 점수는 마이너스된다.
> 5. 스타상(`*`)은 첫 번째 기회에서도 나올 수 있다. 이 경우 첫 번째 스타상(`*`)의 점수만 2배가 된다. (예제 4번 참고)
> 6. 스타상(`*`)의 효과는 다른 스타상(`*`)의 효과와 중첩될 수 있다. 이 경우 중첩된 스타상(`*`) 점수는 4배가 된다. (예제 4번 참고)
> 7. 스타상(`*`)의 효과는 아차상(`#`)의 효과와 중첩될 수 있다. 이 경우 중첩된 아차상(`#`)의 점수는 -2배가 된다. (예제 5번 참고)
> 8. Single(`S`), Double(`D`), Triple(`T`)은 점수마다 하나씩 존재한다.
> 9. 스타상(`*`), 아차상(`#`)은 점수마다 둘 중 하나만 존재할 수 있으며, 존재하지 않을 수도 있다. 
>
> 0~10의 정수와 문자 S, D, T, *, #로 구성된 문자열이 입력될 시 총점수를 반환하는 함수를 작성하라.
>
> **문제가 복잡하여 해당 링크로 직접 접속 권장**

**\+입력 형식**

> - 점수|보너스|[옵션]으로 이루어진 문자열 3세트.
>  예) `1S2D*3T`
>  - 점수는 0에서 10 사이의 정수이다.
>  - 보너스는 S, D, T 중 하나이다.
>  - 옵선은 *이나 # 중 하나이며, 없을 수도 있다.

**+ 출력 형식**

>  3번의 기회에서 얻은 점수 합계에 해당하는 정수값을 출력한다.
>  예) 37

**\+입출력 예**


| 예제 | dartResult | answer | 설명                        |
| ---- | ---------- | ------ | --------------------------- |
| 1    | `1S2D*3T`  | 37     | 11 * 2 + 22 * 2 + 33        |
| 2    | `1D2S#10S` | 9      | 12 + 21 * (-1) + 101        |
| 3    | `1D2S0T`   | 3      | 12 + 21 + 03                |
| 4    | `1S*2T*3S` | 23     | 11 * 2 * 2 + 23 * 2 + 31    |
| 5    | `1D#2S*3S` | 5      | 12 * (-1) * 2 + 21 * 2 + 31 |
| 6    | `1T2D3D#`  | -4     | 13 + 22 + 32 * (-1)         |
| 7    | `1D2S3T*`  | 59     | 12 + 21 * 2 + 33 * 2        |

+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/17682

문제 풀이
---------

**\+My Solution**

```swift
import Foundation

func decodeBonusChar(_ number:Int, _ bounsChar:String) -> Int{
//    print(" number \(number), bonusChar \(bounsChar)")
    switch bounsChar {
    case "S":
        return number
    case "D":
        return number * number
    case "T":
        return number * number * number
    default:
        return 0
    }
}
 
func solution(_ dartResult:String) -> Int {
    var divideResult:[[String]] = [[],[],[]]
    var finalScore:[Int] = []
    var beforeValue:Character = "a"    
    var divideString:[Character] = Array(String(dartResult))

  	//주어진 문자열을 이차배열로 넣기 위한 2중 for문
    for k in 0..<3 {
        for l in 0..<3 {
            // 주어진 문자열이 비었을 경우
            if divideString.isEmpty {
                divideResult[k].append(" ")
                break
            if l == 2 && divideString[0] != "*" && divideString[0] != "#"{
                divideResult[k].append(" ")
            } else if divideString[0] == "0" {
                if beforeValue != "1" { // 획득 점수가 0 인경우 처리 방법
                    divideResult[k].append(String(divideString[0]))
                    beforeValue = divideString.removeFirst()
                } else { // 10을 처리하기 위한 조건
                    divideResult[k].removeLast()
//                    divideResult[k][l]
                    //10추가한 문자 삽입
                    let newString = String(beforeValue) + "0"
//                    add0String.append(contentsOf: "0")
                    divideResult[k].append(newString)
                    // 0 제거
                    divideString.removeFirst()
                    //그다음운 무조건 S D T
                    divideResult[k].append(String(divideString[0]))
                    divideString.removeFirst()
                }
            } else { //
                divideResult[k].append(String(divideString[0]))
                beforeValue = divideString.removeFirst()
            }
        }
    }
    print(divideResult)
    for i in 0..<3 { //획득한 점수의 보너스 옵션 처리 
        finalScore.append(decodeBonusChar(Int(divideResult[i][0])!,divideResult[i][1]))
        if divideResult[i][2] == "*" {
            //1라운드 일 경우 i=0 / 0..<1 0
            //2라운드 일 경우 i=1 / 0..<2 0,1
            //3라운드 일 경우 i=2 / 1..<3 1,2,
            if i == 0 {
                finalScore[0] *= 2
            } else {
                for round in i-1..<i+1 {
                    finalScore[round] *= 2
                }
            }
        } else if divideResult[i][2] == "#" {
            finalScore[i] *= -1
        }
        print(finalScore)
    }
    return finalScore.reduce(0, +)
}

solution("1S2D*3T") // 37
solution("1D2S#10S") // 9
solution("1D2S0T") // 3
solution("0S10T10D*")
solution("1S*2T*3S") // 23
solution("1T2D3D#") // -4
solution("1D2S3T*") // 59
```

**\+Best Solution**

```swift
추후 업데이트
```


Review
-----------------
**\+ 주의사항**

- 1부터 10까지의 점수 획득이 가능하여, 1자리 뿐만 아니라 10인 두자리 정수처리 필요


**\+ Remember or TO-BE**
>1. 총 32개 테스트 중에 2가지 테스트가 지속적으로 실패 하여 추후 재 풀이 예정
>
>| 테스트 6 〉 | 실패 (signal: illegal instruction (core dumped)) |
>| ----------- | ------------------------------------------------ |
>| 테스트 7 〉 | 실패 (signal: illegal instruction (core dumped)) |
>
