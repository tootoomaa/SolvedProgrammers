문제 설명
--------

**\+문제 요약**

> 스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.
>
> 예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.	
>
> | 종류 | 이름                       |
> | ---- | -------------------------- |
> | 얼굴 | 동그란 안경, 검정 선글라스 |
> | 상의 | 파란색 티셔츠              |
> | 하의 | 청바지                     |
> | 겉옷 | 긴 코트                    |
>
> 스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.



**\+제한 조건**

> - clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
> - 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
> - 같은 이름을 가진 의상은 존재하지 않습니다.
> - clothes의 모든 원소는 문자열로 이루어져 있습니다.
> - 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.
> - 스파이는 하루에 최소 한 개의 의상은 입습니다.

**\+입출력 예**

| clothes                                                      | return |
| ------------------------------------------------------------ | ------ |
| [[yellow_hat, headgear], [blue_sunglasses, eyewear], [green_turban, headgear]] | 5      |
| [[crow_mask, face], [blue_sunglasses, face], [smoky_makeup, face]] | 3      |



##### 입출력 예 설명

예제 #1
headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.

```
1. yellow_hat
2. blue_sunglasses
3. green_turban
4. yellow_hat + blue_sunglasses
5. green_turban + blue_sunglasses
```

예제 #2
face에 해당하는 의상이 crow_mask, blue_sunglasses, smoky_makeup이므로 아래와 같이 3개의 조합이 가능합니다.

```
1. crow_mask
2. blue_sunglasses
3. smoky_makeup
```

**+문제 URL**

>https://programmers.co.kr/learn/courses/30/lessons/42578

문제 풀이
---------

**\+My Solution**

```swift
func solution(_ progresses:[Int], _ speeds:[Int]) -> [Int] {
    var resultArray:[Int] = []
    var progressesTemp:[Int] = progresses
    var speedsTemp:[Int] = speeds

    while !progressesTemp.isEmpty {
        for i in 0..<progressesTemp.count { // 모든 작업에 1일 작업량 추가
            progressesTemp[i] += speedsTemp[i]
        }
        var completeCount:Int = 0
        if progressesTemp[0] > 99 { // 1번째 작업이 100이되어서 적용 가능시
            for i in progressesTemp { // 뒤에 100 이상 작업 체크
                if i > 99 {
                    completeCount += 1	// completeCount 변수에 저장
                } else {
                    break;
                }
            }
            resultArray.append(completeCount) // 완료된 작업 갯수 배열 추가
            for _ in 0..<completeCount { // 완료된 작업 삭제
                progressesTemp.remove(at: 0)
                speedsTemp.remove(at: 0) // <-- 이부분을 놓쳐서 시간이 오래걸림
            }
        }
    }
    return resultArray
}
```

**\+Best Solution**

```swift
func solution(_ progresses:[Int], _ speeds:[Int]) -> [Int] {
    return zip(progresses, speeds)
        .map { (100 - $0) / $1 } // 각 작업의 잔여 일 수 계산
        .reduce(([], 0)) { (tuple, day) -> ([Int], Int) in
            let (list, lastMax) = tuple 
            guard let lastCount = list.last else { // 결과값이 없으면 실행
                return ([1], day) // day 값은 작업별 소요 기간이 순차적으로 대입
            }
            if lastMax >= day { // 작업 기간보다 작은 값 갯수 구하여 저징
                return (list.dropLast() + [lastCount + 1], lastMax)
            }
            return (list + [1], day)
        }.0
}

```


Review
-----------------
**\+ New Know**

> 1. zip 메소드
>    - 두개의 서로다른 순차적인 값을 묶어서 새로운 시퀀스 쌍으로 만들어 주는 메소드
>
>
> ```swift
> func zip<Sequence1, Sequence2>(_ sequence1: Sequence1, _ sequence2: Sequence2) -> Zip2Sequence<Sequence1, Sequence2> where Sequence1 : Sequence, Sequence2 : Sequence
> // 예제
> let words = ["one", "two", "three", "four"]
> let numbers = 1...4
> 
> for (word, number) in zip(words, numbers) {
>     print("\(word): \(number)")
> }
> // Prints "one: 1"
> // Prints "two: 2
> // Prints "three: 3"
> // Prints "four: 4"
> ```
>

