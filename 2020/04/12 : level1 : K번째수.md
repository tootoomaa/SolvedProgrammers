문제 설명
--------

**\+문제 요약**

> 배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.
>
> 예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면
>
> 1. array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.
> 2. 1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.
> 3. 2에서 나온 배열의 3번째 숫자는 5입니다.
>
> 배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

**\+재한사항**
> - array의 길이는 1 이상 100 이하입니다.
> - array의 각 원소는 1 이상 100 이하입니다.
> - commands의 길이는 1 이상 50 이하입니다.
> - commands의 각 원소는 길이가 3입니다.

**\+입출력 예**

array | commands | return
---|---|---
[1, 5, 2, 6, 3, 7, 4] | [[2, 5, 3], [4, 4, 1], [1, 7, 3]] | [5, 6, 3] 

>  [1, 5, 2, 6, 3, 7, 4]를 2번째부터 5번째까지 자른 후 정렬합니다. [2, 3, 5, 6]의 세 번째 숫자는 5입니다.
> [1, 5, 2, 6, 3, 7, 4]를 4번째부터 4번째까지 자른 후 정렬합니다. [6]의 첫 번째 숫자는 6입니다.
> [1, 5, 2, 6, 3, 7, 4]를 1번째부터 7번째까지 자릅니다. [1, 2, 3, 4, 5, 6, 7]의 세 번째 숫자는 3입니다.

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/42748



문제 풀이
---------

**\+My Solution**

```swift
func solution(_ array:[Int], _ commands:[[Int]]) -> [Int] {

    var resultArray = [Int]()			// 결과를 리턴할 배열
    var tempArray = [Int]()				// 임시 저장을 위한 배열
    
    for i in 0..<commands.count { // commands의 2차 배열의 갯수
        tempArray = []						// 임시 배열 tempArray 초기화
        for k in commands[i][0]-1..<commands[i][1] {		//commands를 이용한 array 배열 자르기
            tempArray.append(array[k])									//초기화된 임시 배열에 값 추가 
        }						//commands[i][0] 부터 commands[i][1] 까지 별도 배열을 tempArray에 담기
        tempArray.sort()																// 배열 정렬
        resultArray.append(tempArray[commands[i][2]-1])	// commands[i][2] 번째 크기 값 추출
    }
    return resultArray
}

solution([1,5,2,6,3,7,4],[[2,5,3],[4,4,1],[1,7,3]])
```

**\+Best Solution**

```swift
func solution(_ array:[Int], _ commands:[[Int]]) -> [Int] {
    return commands.map{	// commands를 map함수의 인자로 투입
        let i = $0[0]-1		// $0을 commands로 생각 -> commands[0]
        let j = $0[1]-1
        let k = $0[2]-1
        return array[i...j].sorted()[k] // k번째 값을 리턴 int형
    }
}
```

Review
-----------------
**\+ New Know**

> 1. array(n...k)
>    - array 배열의 n번째 부터 k번째 까지 선택
> 2. map 
>    - 고차함수중에 하나로 매개변수로 함수를 갖는 함수를 말함
>   - 순차적으로 정의된 배열,딕셔너리 등에서 값을 하나씩 추출하여 map뒤에 정의된 함수를 앞서 추출한 값을 **$0** 으로 받아 적용하는 방식
> 
> 
>```swift
> let array = [0, 1, 2, 3]
> let newArray = array.map { $0 * 5 }
> print(newArray) //[0, 5, 10, 15]
> ```
> 
>**출처** : [링크](https://zetal.tistory.com/entry/swift-기초문법-15-맵Map-필터Filter-리듀스Reduce)

**\+ Remember or TO-BE**

> 1. for-in 구문이 필요한 경우 고차함수를 사용을 고려
> 2. 코드를 최소화 하도록 구성
> ```swift
>//AS-IS
>  tempArray.sort()
>  resultArray.append(tempArray[commands[i][2]-1])
> //TO-BR
>  resultArray.append(tempArray.sorted()[commands[i][2]-1])
> ```

