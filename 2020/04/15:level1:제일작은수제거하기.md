문제 설명
--------

**\+문제 요약**

> 정수를 저장한 배열, arr 에서 가장 작은 수를 제거한 배열을 리턴하는 함수, solution을 완성해주세요. 단, 리턴하려는 배열이 빈 배열인 경우엔 배열에 -1을 채워 리턴하세요. 예를들어 arr이 [4,3,2,1]인 경우는 [4,3,2]를 리턴 하고, [10]면 [-1]을 리턴 합니다.

**\+재한사항**
> - arr은 길이 1 이상인 배열입니다.
> - 인덱스 i, j에 대해 i ≠ j이면 arr[i] ≠ arr[j] 입니다.

**\+입출력 예**

arr | result 
---|---
[4,3,2,1,] | [4,3,2] 
[10] | [-1] 

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/12931



문제 풀이
---------

**\+My Solution**

```swift
func solution(_ arr:[Int]) -> [Int] {
    var smallistValue:Int = 99999		// 임의이 큰 수를 기본 값으로 적용
    var resultArray = arr
    for i in resultArray {
        //배열 내 값들을 비교하여 가장 작은 값을 선택 by 3항 연산자
        smallistValue = i > smallistValue ? smallistValue : i
    }
    // 지우려는 값의 배열 내 위치를 확인
    var deleteValueindex = resultArray.firstIndex(of: smallistValue)! 
    resultArray.remove(at: deleteValueindex)
    // 최소값 제거 후 배열이 비었는지 확인
    if resultArray.isEmpty == true {
        return [-1]
    } else {
        return resultArray
    }
}
```

**\+Best Solution**

```swift
func solution(_ arr:[Int]) -> [Int] {
    var result = arr

    if result.count <= 1 {
        return [-1]
    } else {
      	// min() 함수를 통해 배열내 작은 값 즉시 확인
        if let index = arr.index(of: arr.min()!) {
            result.remove(at: index)
            return result
        }else {
            return [-1]
        }
    }
}
```


Review
-----------------
**\+ New Know**

> 1. min 메소드 : 배열내에서 최소값을 추출
>
>    ```swift
>    var arr:[Int] = [4,3,2,1]
>    arr.min() 	// 1 
>    ```
>    
> 2. 배열내에 값을 삭제하는 방법
> 
>    ```swift
>    var arr:[Int] = [4,3,2,1]
>    arr.remove(at: Int) // 여기서 Int는 해당 값의 배열내 위치  
>    ```

**\+ Remember or TO-BE**

> 1. min() 메소드를 활용했다면 더 간결한 코드 작성 가능
>
> 2. 애플 공식 문서를 이용하여 내가 필요로 하는 메소드가 존제하는지 확인 하는 습관 필요
>    애플 공식 문서 : xcode 실행 후 "**shift + command + 0**"
