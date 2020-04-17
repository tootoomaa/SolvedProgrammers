문제 설명
--------

**\+문제 요약**

> 네오는 평소 프로도가 비상금을 숨겨놓는 장소를 알려줄 비밀지도를 손에 넣었다. 그런데 이 비밀지도는 숫자로 암호화되어 있어 위치를 확인하기 위해서는 암호를 해독해야 한다. 다행히 지도 암호를 해독할 방법을 적어놓은 메모도 함께 발견했다.
>
> ~~ 중략 ~~~
>
> 문제가 복잡하여 해당 링크로 직접 접속 권장

**\+재한사항**

> - 두 수는 1이상 1000000이하의 자연수입니다.
> 

**+ 출력 형식**

>  원래의 비밀지도를 해독하여 `'#'`, `공백`으로 구성된 문자열 배열로 출력하라.

**\+입출력 예**

매개변수 | m 
---|---
n | 5 
arr1 | [9,20,28,18,11] 
 arr2 | [30,1,21,17,28] 
 출력 | ["#####","# # #", "### #", "# ##", "#####"] 

 매개변수 | m 
---|---
n | 12 
arr1 | [46, 33, 33 ,22, 31, 50] 
 arr2 | [27 ,56, 19, 14, 14, 10] 
 출력 | ["######", "### #", "## ##", " #### ", " #####", "### # "] 

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/17681

**문제 해설 ( kakao )**

> https://tech.kakao.com/2017/09/27/kakao-blind-recruitment-round-1/

문제 풀이
---------

**\+My Solution**

```swift
// 두 배열을 받아 값이 같으면 #, 다르면 " " 입력
func newArrToString(_ n:Int,_ arr1:[Int], _ arr2:[Int]) -> [String] {
    var arrtempArray:[String] = []
    var arrtempString:String
    var myIndex = 0
    var arr1temp:Int = 0
    var arr2temp:Int = 0
    
    for i in 0..<arr1.count {
        arr1temp = arr1[i]
        arr2temp = arr2[i]
        arrtempString = ""
        for _ in 0..<n {
            if (arr1temp%2 == 0 ? " " : "#") == " " && (arr2temp%2 == 0 ? " " : "#") == " " {
                arrtempString.append(" ")
            } else {
                arrtempString.append("#")
            }
            arr1temp = arr1temp/2
            arr2temp = arr2temp/2
        }
        myIndex += 1
        arrtempArray.append(arrtempString)
    }
    print(arrtempArray)
    return arrtempArray
}
// 값을 잘 못 넣어서 급하게 수정, 각 배열에 저장된 "####" 값을 거꾸로 변경
func reverseString(_ n:Int,_ arr:[String]) -> [String] {
    var tempString:String = ""
    var resultArray:[String] = []
    for i in 0..<n {
        tempString = ""
        for k in arr[i].reversed() {
            tempString += String(k)
//            print(tempString)
        }
        resultArray.append(tempString)
    }
    return resultArray
}

func solution(_ n:Int, _ arr1:[Int], _ arr2:[Int]) -> [String] {
    var tempArr1Tostring: [String] = []
    
    tempArr1Tostring = newArrToString(n, arr1, arr2)
    tempArr1Tostring = reverseString(n, tempArr1Tostring)
    return tempArr1Tostring
}
```

**\+Best Solution**

```swift
func solution(_ n:Int, _ arr1:[Int], _ arr2:[Int]) -> [String] {
    var answer: [String] = []
    for i in 0..<n{
      	// "|" 를 통한 두 배열의 OR 연산
        var data = UInt16(arr1[i]) | UInt16(arr2[i])
        answer.append("") // 배열 초기화
        for _ in 0..<n{
            if data & 0b00000001 == 1{ // 나머지 1
                answer[i] = "#" + answer[i]
            }else{
                answer[i] = " " + answer[i]
            }
            data = data >> 1 // 나머지 제거, data/2 의 몫 부분
        }
    }
    return answer
}
```


Review
-----------------
**\+ New Know**

> 1. 비트 연산
>
>    1. " | " , UInt16 두값을 받아 각 값의 OR 비트 연산 수행
>
>    - 	```swift
>      static func | (lhs: UInt16, rhs: UInt16) -> UInt16
>      
>      let x: UInt8 = 5          // 0b00000101
>      let y: UInt8 = 14         // 0b00001110
>      let z = x | y             // 0b00001111
>      // z == 15
>      ```
>      
>
>    2. "&", UInt16 두값을 받아 각 값의 and 비트 연산 수행
>
>    - ```swift
>      static func & (lhs: Int, rhs: Int) -> Int
>      
>      et x: UInt8 = 5          // 0b00000101
>      let y: UInt8 = 14         // 0b00001110
>      let z = x & y             // 0b00000100
>      // z == 4
>      ```
>
>    3. ">>", 오른쪽으로 비트 이동
>
>    - ```swift
>      var temp = 3
>      temp = temp >> 1
>      ```
>
> **\+ Remember or TO-BE**

> - 비트 연산은 알았지만 &, |  을 통해 직접 숫자끼리, 숫자와 비트를 이용하여 계산하는 방법은 처음 알게됨
> - 

