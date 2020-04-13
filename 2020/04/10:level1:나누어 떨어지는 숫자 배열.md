문제 설명
--------

**\+문제 요약**
> array의 각 element 중 divisor로 나누어 떨어지는 값을 오름차순으로 정렬한 배열을 반환하는 함수, solution을 작성해주세요.
> divisor로 나누어 떨어지는 element가 하나도 없다면 배열에 -1을 담아 반환하세요.

**\+재한사항**

> - arr은 자연수를 담은 배열입니다.
> - 정수 i, j에 대해 i ≠ j 이면 arr[i] ≠ arr[j] 입니다.
> - divisor는 자연수입니다.
> - array는 길이 1 이상인 배열입니다.

**\+입출력 예**

 arr | divisor | return
---|---|---
[5, 9, 7, 10] | 5 | [5,10] 
[2, 36, 1, 3] | 1 | [1, 2, 3, 36]
[3, 2, 6 | 10 | [-1]

**\+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/12910



문제 풀이
---------

**\+My Solution**
```swift
func solution(_ arr:[Int], _ divisor:Int) -> [Int] {   
    var resultArry: [Int] = []
    arr.sorted().forEach{ (i) in
        if i % divisor == 0 { resultArry.append(i) }
    }

    switch resultArry.count {
    case 0:
        return [-1]
    default:
        return resultArry
    }
}
```

**\+Best Solution**
```swift
func solution(_ arr:[Int], _ divisor:Int) -> [Int] {
    let array = arr.sorted().filter{ $0 % divisor == 0 } //filter 함수 사용
    return  array == [] ? [-1] : array  //삼항 연산자 사용
}
```

Review
-----------------
**\+ New Know**

> 1. filter 함수를 사용한 배열인자 추출 및 처리  
>  - 함수 정의 
> ```swift
> func filter(_ isIncluded: (Int) throws -> Bool) rethrows -> [Int]
> ```
>  - 함수 예제
> ```swift
> func isEven(number: Int) -> Bool {
>  return number % 2 == 0
> }
> var evens = [Int]()
> evens = Array(1...10).filter(isEven)
> print(evens) // 출력: [2, 4, 6, 8, 10] 
> ```
참조 : [함수형 프로그래밍과 배열의 filter, reduce, map](https://outofbedlam.github.io/swift/2016/02/12/functional-programming/)

**\+ Remember or TO-BE**

> 1. 지금은 간단한 함수여서 큰 차이가 없겠지만 나중에 큰 프로그램을 작성할때는 swich문으로 작성할지 if문으로 효율성 측면에서 고려해봐야겠다.

