---
slug: kotlin 기본사항 더보기
title: kotlin 기본사항 더보기
authors: atlas
tags: [ compose, android ,kotlin 기본사항 더보기 , kotlin]
---

## 제네릭 ,객체,확장 
**generics allow a data type, such as a class, to specify an unknown placeholder data type that can be used with its properties and methods**

![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/67367d9308c171da_1920.png?hl%253Dko)


![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/81170899b2ca0dc9_1920.png?hl%253Dko)


![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/9b8fce54cac8d1ea_1920.png?hl%253Dko)


```
class FillInTheBlankQuestion(
    val questionText: String,
    val answer: String,
    val difficulty: String
)

class TrueOrFalseQuestion(
    val questionText: String,
    val answer: Boolean,
    val difficulty: String
)
class NumericQuestion(
    val questionText: String,
    val answer: Int,
    val difficulty: String
)
```

제네릭 적용 

```
class Question<T>(
    val questionText: String,
    val answer: T,
    val difficulty: String
)
```
```
fun main() {
    val question1 = Question<String>("Quoth the raven ___", "nevermore", "medium")
    val question2 = Question<Boolean>("The sky is green. True or false", false, "easy")
    val question3 = Question<Int>("How many days are there between full moons?", 28, "hard")
}
```


## enum

![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/f4bddb215eb52392_1920.png?hl%253Dko)


![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/f3cfa84c3f34392b_1920.png?hl%253Dko)

```
enum class Difficulty {
    EASY, MEDIUM, HARD
}
```
```
class Question<T>(
    val questionText: String,
    val answer: T,
    val difficulty: Difficulty
)
```
```
val question1 = Question<String>("Quoth the raven ___", "nevermore", Difficulty.MEDIUM)
val question2 = Question<Boolean>("The sky is green. True or false", false, Difficulty.EASY)
val question3 = Question<Int>("How many days are there between full moons?", 28, Difficulty.HARD)
```


## data class 

- 데이터만 포함
- 작업하는 메서드 없는 경우 
  데이터 클래스로 정의할 수 있다. 


![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/e7cd946b4ad216f4_1920.png?hl%253Dko)



**데이터 클래스를 사용하면 toString() 및 기타 메서드가 클래스의 속성에 따라 자동으로 구현**


```
println(question1.toString())
Question@37f8bb67
```

data 키워드를 사용해서 데이터 클래스로 지정
```
println(question1.toString())
Question(questionText=Quoth the raven ___, answer=nevermore, difficulty=MEDIUM)
```


데이터 클래스에는 생성자에 매개변수가 하나 이상 있어야 하며 모든 생성자 매개변수는 val 또는 var로 표시되어야 함
데이터 클래스도 abstract 또는 open, sealed, inner일 수 없음


## 싱글톤 

예상 시나리오 
- 현재 사용자를 대상으로 한 모바일 게임의 플레이어 통계
- 단일 하드웨어 기기와 상호작용(예: 스피커를 통해 오디오 전송)
- 원격 데이터 소스(예: Firebase 데이터베이스)에 액세스하는 객체
- 한 번에 한 사용자만 로그인해야 하는 인증


![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/645e8e8bbffbb5f9_1920.png?hl%253Dko)

 
 class 키워드 대신 object 키워드를 사용
 싱글톤 객체에는 생성자를 포함할 수 없음 -> 개발자가 인스턴스를 직접 만들 수 없기 때문 -> 대신 모든 속성이 중괄호 안에 정의되며 초깃값이 부여됨

```
object StudentProgress {
    var total: Int = 10
    var answered: Int = 3
}
```

### 싱글톤 객체에 액세스
![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/1b610fd87e99fe25_1440.png?hl%253Dko)


### companion 객체로 선언 

컴패니언 객체를 사용하여 다른 클래스 내에서 싱글톤 객체를 정의할 수 있다.
컴패니언 객체를 사용하면 클래스 내에서 속성과 메서드에 액세스할 수 있다.
 -> (객체의 속성과 메서드가 해당 클래스에 속한 경우) 문법이 더 간결
```
class Quiz {
    val question1 = Question<String>("Quoth the raven ___", "nevermore", Difficulty.MEDIUM)
    val question2 = Question<Boolean>("The sky is green. True or false", false, Difficulty.EASY)
    val question3 = Question<Int>("How many days are there between full moons?", 28, Difficulty.HARD)

    companion object StudentProgress {
        var total: Int = 10
        var answered: Int = 3
    }
}


  println("${Quiz.answered} of ${Quiz.total} answered.")

```
### 확장 속성 추가 

![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/1e8a52e327fe3f45_1440.png?hl%253Dko)
```
val Quiz.StudentProgress.progressText: String
    get() = "${answered} of ${total} answered"

fun main() {
    println(Quiz.progressText)
}
```

### 확장 함수 추가 


![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/879ff2761e04edd9_1440.png?hl%253Dko)

```
fun Quiz.StudentProgress.printProgressBar() {
    repeat(Quiz.answered) { print("▓") }
    repeat(Quiz.total - Quiz.answered) { print("▒") }
    println()
    println(Quiz.progressText)
}

fun main() {
    Quiz.printProgressBar()
}
```


### 인터페이스를 사용해서 확장 함수 다시 작성 
![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/eeed58ed687897be_1440.png?hl%253Dko)

![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/6b04a8f50b11f2eb_1440.png?hl%253Dko)

![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-generics/img/78af59840c74fa08_1440.png?hl%253Dko)

```
interface ProgressPrintable {
    val progressText: String
    fun printProgressBar()
}
```

### 범위 함수를 사용하여 클래스 속성 및 메서드에 엑세스 
범위 함수로 반복 객체 참조 제거 
let()을 사용하여 긴 객체 이름 바꾸기 

```
as-is
    println(question1.questionText)
    println(question1.answer)
    println(question1.difficulty)
    println()

to-be    
    question1.let {
        println(it.questionText)
        println(it.answer)
        println(it.difficulty)
    }

```

### apply()를 사용하여 변수 없이 객체의 메서드 호출

수신객체 내부 프로퍼티를 변경한 다음 수신객체 자체를 반환하기 위해 사용되는 함수
apply()를 사용하여 변수 없이 객체의 메서드 호출


```
Quiz().apply {
    printQuiz()
}

//동일한코드 
// apply 사용
val peter = Person().apply {
    name = "Peter"
    age = 18
}

//apply를 사용하지 안흔ㄴ 코드 
val clark = Person()
clark.name = "Clark"
clark.age = 18
```

## 컬렉션 
### 배열 
![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-collections/img/bb77ec7506ac1a26_1440.png?hl%253Dko)
![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-collections/img/69e283b32d35f799_1440.png?hl%253Dko)
![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-collections/img/9469e321ed79c074_1440.png?hl%253Dko)

### List 

List는 순서가 지정된 읽기 전용 항목 컬렉션과 관련된 속성과 메서드를 정의하는 인터페이스
MutableList는 요소 추가 및 삭제와 같이 목록을 수정하는 메서드를 정의하여 List 인터페이스를 확장
```
array
println(solarSystem[2])

list
println(solarSystem.get(3))
```

컬렉션의 요소를 추가, 삭제, 업데이트하는 기능은 MutableList 인터페이스를 구현하는 클래스에서만 사용가능


### Set 
![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-collections/img/9de9d777e6b1d265_1440.png?hl%253Dko)

이와 같은 컬렉션이 가능한 이유는? 비결은 해시코드
해시 코드는 Kotlin 클래스의 hashCode() 메서드에서 생성된 Int
Kotlin 객체의 반고유 식별자로 생각할 수 있음
객체를 약간만 변경해도(예: String에 문자 하나를 추가) 해시 값은 크게 달라짐
두 객체가 동일한 해시 코드를 가질 수 있지만(해시 충돌이라고 함) 
hashCode() 함수를 사용하면 다른 두 값이 대부분 각각 고유한 해시 코드를 가지는 일정 수준의 고유성이 보장
![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-collections/img/84842b78e78f2f58_1440.png?hl%253Dko)



### Map
Map은 키와 값으로 구성된 컬렉션

![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-collections/img/8571494fb4a106b6_1440.png?hl%253Dko)

맵은 mapOf() 또는 mutableMapOf() 함수를 사용하여 선언
![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-collections/img/affc23a0e1f2b223_1440.png?hl%253Dko)

![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-collections/img/2ed99c3391c74ec4_1440.png?hl%253Dko)

## 고차함수 

종류 : forEach(), map(), filter(), groupBy(), fold(), sortedBy()

### forEach()

{List object}.forEach {
    println("Menu item: ${it.name}")
}

### Map

![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-higher-order-functions/img/e0605b7b09f91717_1440.png?hl%253Dko)



### filter 
![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-higher-order-functions/img/d4fd6be7bef37ab3_1440.png?hl%253Dko)

### groupBy
![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-higher-order-functions/img/54e190b34d9921c0_1440.png?hl%253Dko)


![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-higher-order-functions/img/4c3333da9e5ee352_1440.png?hl%253Dko)

![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-higher-order-functions/img/4219eacdaca33f1d_1440.png?hl%253Dko)



### fold 
fold() 함수는 컬렉션에서 단일 값을 생성하는 데 사용
초깃값. 데이터 유형은 함수를 호출할 때 추론됩니다. 즉, 초깃값 0은 Int로 추론
(reduce 함수와 동일)
숫자 유형의 sum() , sumOf() 함수도 있음 
![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-higher-order-functions/img/a9e11a1aad05cb2f_1440.png?hl%253Dko)



### sortedBy
![Alt text](https://developer.android.com/static/codelabs/basic-android-kotlin-compose-higher-order-functions/img/5fce4a067d372880_1440.png?hl%253Dko)





