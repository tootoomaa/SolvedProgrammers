문제 설명
--------

**\+문제 요약**

> 두 수를 입력받아 두 수의 최대공약수와 최소공배수를 반환하는 함수, solution을 완성해 보세요. 배열의 맨 앞에 최대공약수, 그다음 최소공배수를 넣어 반환하면 됩니다. 예를 들어 두 수 3, 12의 최대공약수는 3, 최소공배수는 12이므로 solution(3, 12)는 [3, 12]를 반환해야 합니다.

**\+재한사항**
> - 두 수는 1이상 1000000이하의 자연수입니다.

**\+입출력 예**

n | m | return
---|---|---
3 | 12 |[3,12]
2 | 5 |[1,10]

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/12940



문제 풀이
---------

**\+My Solution**

```swift
/* algorithm
 - 최대공약수의 최소값은 1, 최대값 
 - 최소공배수의 최소값은 m값이고, 최대값은 n*m
*/
//최대공약수, 최대공배수
func solution(_ n:Int, _ m:Int) -> [Int] {
    let maxNumber = n > m ? n : m
	  let minNumber = n > m ? m : n
    var returnArray:[Int] = [0,0]

    //최대공약수
    for i in 1..<maxNumber { // maxNumber 주어진 숫자중 큰 값 선택 
        if n.isMultiple(of: i) && m.isMultiple(of: i) {
            returnArray[0] = i // 최대값을 구해야 함으로 계속 진행
        }
    }
    //최소공배수
    for k in m..<n*m+1 {
        if k%m == 0 && k%n == 0 { // 주어진 두 수로 각각 나머지가 0일 경우 공배수
            returnArray[1] = k    // 만족하는 수를 배열에 저장 
            break 								//최소값을 구해야함으로 조건 만족시 중지
        }
    }
    return returnArray
}
```

**\+Best Solution**

```swift
/* algorithm
  - (a * b / a,b의 최대공약수) 를 이용
*/
//최대공약수
func gcd(_ a: Int, _ b: Int) -> Int {
    let mod: Int = a % b 			// a가 b의 최대공약수인지 확인
    return 0 == mod ? min(a, b) : gcd(b, mod)
  	//gcd의 재귀함수
}
//최소공배수
func lcm(_ a: Int, _ b: Int) -> Int {
    return a * b / gcd(a, b)
}

func solution(_ n:Int, _ m:Int) -> [Int] {
    return [gcd(n, m), lcm(n, m)]
}
```


Review
-----------------
**\+ New Know**

> - 재귀함수를 통한 동일한 작업 반복
>   - 값을 리턴하여 루프를 종료하는 방법이 중요한듯

**\+ Remember or TO-BE**

> - 다양한 알고리즘 공부 필요

