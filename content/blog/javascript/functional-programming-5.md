---
title: '[번역] 함수형 프로그래밍 전문가 되기 (Part 5)'
date: 2020-07-19 23:04:12
category: Javascript
thumbnail: ./images/functional_programming/functional_programming_5_1.png
draft: false
---


이 글은 Charles Scalfani의 [So You Want to be a Functional Programmer (Part 5)](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-5-c70adc9cf56a)를 번역한 게시물입니다.  
Thank you Charles Scalfani! Thanks to your writing, I can grow further as a developer.

![functional_programming_5_1](./images/functional_programming/functional_programming_5_1.png)

함수형 프로그래밍의 개념을 이해하기 위해 내딛는 첫걸음은 매우 중요하다. 매우 힘든 첫걸음이지만 올바른 관점으로 접근한다면 힘들어할 필요가 없다.

이전 게시물 : [Part 1](https://front-end.me/javascript/functional-programming-1/), [Part 2](https://front-end.me/javascript/functional-programming-2/), [Part 3](https://front-end.me/javascript/functional-programming-3/), [Part 4](https://front-end.me/javascript/functional-programming-4/)

## 참조 투명성

![functional_programming_5_2](./images/functional_programming/functional_programming_5_2.png)

**참조 투명성**은 순수 함수가 표현식으로 안전하게 대체될 수 있다는 멋진 용어이다. 아래 예제가 이해하는데 도움이 될 것이다.

대 수학에서 다음 공식을 봤을 때,

```
y = x + 10
```

그리고 x 값이 다음과 같을 때

```
x = 3
```

x값을 방정식에 대입하여 다음과 같은 식을 얻을 수 있다.

```
y = 3 + 10
```

방정식이 아직 유효하다는 것에 주목하자. 우리는 순수함수로도 위와 같이 대체를 할 수 있다.

여기 Elm로 작성한 문자열 주위에 작은 따옴표를 넣는 함수가 있다.

```elm
quote str =
    "'" ++ str ++ "'"
```

그리고 위의 함수를 사용하는 코드를 살펴보자.

```elm
findError key =
    "Unable to find " ++ (quote key)
```

`findError`는 `key` 검색을 실패할때 오류 메시지를 생성한다.   
`quote` 함수가 순수하기 때문에, 우리는 `quote`를 (')으로 대체 할 수 있다.

```elm
findError key =
   "Unable to find " ++ ("'" ++ str ++ "'")
```

나는 이것을 **Reverse Refactoring**이라고 부른다(그 편이 이해가 잘된다).    
이 프로세스는 프로그래머 또는 프로그램이 코드를 추론하기 위해 사용할 수 있다. (예: 컴파일러 또는 테스트 프로그램)

**Reverse Refactoring**는 특히 재귀함수에 대한 추론을 할때 도움이 된다.

![functional_programming_5_3](./images/functional_programming/functional_programming_5_3.png)

대부분의 프로그램들은 싱글 스레드다. 즉 한 번에 하나의 코드만 실행 된다.    
프로그램이 멀티스레드 일지라도 대부분의 스레드는 I/O 작업(파일, 네트워크 기타 등등)이 완료될 때 까지 차단된다.

이것이 우리가 코드를 작성할 때 자연스럽게 순차적인 순서(ordered steps) 관점으로 생각하는 이유 중 하나이다.
```
1. 빵을 꺼내라  
2. 빵 2조각을 토스트 기계에 넣어라  
3. 굽기를 선택해라  
4. 레버를 밑으로 내려라  
5. 토스트가 올라올때까지 기다려라  
6. 토스트를 꺼내라  
7. 버터를 꺼내라  
8. 버터 칼을 꺼내라  
9. 토스트에 버터를 발라라
```


이 예제에서 2가지 독립적인 동작이 있다. 버터를 꺼내는 것과, 빵을 굽는 것이다.   
이들은 9단계가 되서야 상호의존적이 된다.

우리는 7, 8 단계와 1 ~ 6 단계를 동시에 진행 할 수 있다. 왜냐하면 서로에게 독립적인 행동들이기 때문이다.

하지만 동시에 진행하게 되면, 우리는 혼란에 빠진다. 아래의 예시를 살펴보자.

```
Thread 1  
--------  
1. 빵을 꺼내라  
2. 빵 2조각을 토스트 기계에 넣어라  
3. 굽기를 선택해라  
4. 레버를 밑으로 내려라  
5. 토스트가 올라올때까지 기다려라  
6. 토스트를 꺼내라
```
``` 
Thread 2  
--------  
1. 버터를 꺼내라  
2. 버터 칼을 꺼내라  
3. (중요) Thread 1이 끝날 때까지 기다려라
4. 토스트에 버터를 발라라
```
만약 스레드 1이 실패 하면 스레드 2는 어떤 일이 발생 할까? 두 스레드를 조율하는 메커니즘은 무엇인가? 누가 토스트의 주인인가? 스레드 1? 스레드 2? 아니면 둘다?   
이러한 복잡성에 대해 생각하지 않고, 차라리 우리의 프로그램이 단 하나의 스레드만 사용하도록 하는 것이 더 쉽다.   
그러나 프로그램의 효율을 가능한 최대로 짜내야 한다면, 우리는 멀티스레딩 소프트웨어를 만드는 것에 엄청난 노력을 기울여야 한다.
그러나 멀티 스레딩에는 2가지 주요한 문제점이 있다.    

첫번 째, 멀티 스레드 프로그램은 쓰기, 읽기, 추론, 테스트, 디버그가 어렵다.  
두번 째, 자바스크립트 같은 몇몇 언어들은 멀티스레딩을 지원하지 않는다. 지원 하더라도 성능이 좋지 않다.   

순서 상관 없이 모든 것을 병렬로 처리되면 어떨까?
이상한 소리로 들릴지도 모르지만, 그렇게 나쁜 방법은 아니다. 이를 이해하기 위해 Elm 코드를 살펴보자.

```elm
buildMessage message value =
    let
        upperMessage =
            String.toUpper message
        quotedValue =
            "'" ++ value ++ "'"
    in
        upperMessage ++ ": " ++ quotedValue
```

`buildMessage`는 `message`와 `value`를 받은 뒤 대문자 `message`와 콜론(:), 그리고 작은 따옴표(')안의 `value`를 생산한다.

병렬로 처리하기 위해서는 `upperMessage`와 `quotedValue`가 독립적이여야 한다. 그리고 독립적이기 위해서는 2가지 조건을 충족시켜야 한다.
  
첫 번째, 순수 함수여야 한다. 순수함수는 실행될 때 서로의 실행에 영향을 끼치지 않기 때문에 독립적이기 위해서 중요하다.  
두 번째, 한 함수의 출력 값이 다른 함수의 입력 값이 되면 안된다. 그렇지 않으면, 두번째 스레드가 실행되지 못하고 첫번째 스레드가 끝날 때까지 계속 기다릴 것이다.

위의 코드에서 `upperMessage`와 `quotedValue`는 둘다 순수 함수고, 다른 하나의 출력 값을 필요로 하지 않는다.
그러므로 이 두 함수는 순서에 상관없이 실행이 잘 된다.   

컴파일러는 프로그래머의 도움없이 알아서 결정 할 수 있다. 하지만 이것은 부작용의 영향을 파악하는 것이 어렵기 때문에 순수 함수 언어에서만 가능하다.

> 순수 함수 언어의 실행 순서는 컴파일러에 의해 결정된다.

이것은 CPU 속도가 빨라지지 않고 있는 현재 시점에선 굉장한 이점이 있다.    
CPU 속도가 빨라지지 않는 대신에 코어수가 점점 더 증가하고 있는데, 이는 하드웨어 레벨에서 코드를 병렬로 실행시킬 수 있다는 것을 뜻한다.

불행하게도, 명령형 언어에서는 매우 raw한 수준을 제외하고 이러한 코어 수 증가의 장점을 완전히 활용할 수 없다. 완전히 활용하려면 프로그램의 구조를 획기적으로 바꿔야 한다.
순수 함수형 언어를 사용하면 코드를 한줄도 변경하지 않고도 미세한 레벨의 CPU 코어를 자동으로 활용할 수 있다.

## 타입 명시

![functional_programming_5_4](./images/functional_programming/functional_programming_5_4.png)

정적 타입 언어에서, 타입은 인라인(inline)으로 정의된다. 이 뜻을 이해 하기 위해 아래의 Java 코드를 살펴보자.

```java
public static String quote(String str) {
    return "'" + str + "'";
}
```

함수 정의와 타입이 어떻게 함께 인라인이 되는지 주목해라. 제네릭을 사용한다면 더 구별하기 힘들어 질 것이다.

```java
private final Map<Integer, String> getPerson(Map<String, String> people, Integer personId) {  
    // ...  
}
```

변수의 이름을 찾기 위해서는 주의 깊게 읽어야 한다.

동적 타입 언어에서는 전혀 문제가 되지 않는다. 자바스크립트에서 코드를 다음과 같이 작성 할 수 있다.

```js
var getPerson = function(people, personId) {
    // ...
};
```

타입의 정보가 없는 것이 문제가 되지 않는다면 더 읽기 쉬워진다.  
단 하나의 문제는 타입 안전성을 포기한 것이다. 우리는 쉽게 이 파라미터들을 거꾸로 전달할 수도 있다.    

즉, `people`을 숫자타입으로, `personId`를 객체 타입으로 전달할 수 있다. 이렇게 변경을 하더라도 에러가 발생하지 않는다.    
즉 프로그램상 문제가 있지만, 어디에서 에러가 발생했는지 발견하기 힘들다는 것을 뜻한다.
  
자바에서는 이런 현상이 일어나지 않는다. 컴파일 자체가 되지 않기 때문이다.  
만약 우리가 자바스크립트의 유연함과 자바의 안정성을 모두 가질 수 있다면 어떨까? 

그렇게 할 수 있다. 아래 Elm에서의 함수 타입 명시를 살펴보자.

```elm
add : Int -> Int -> Int
add x y =
    x + y
```

각 라인에서 타입 정보가 어떻게 작성되었는지 주목해라. 띄어쓰기는 분리를 하기 위함이다.  
당신은 타입 명시에 오타가 있다고 생각할 수도 있다. 저 코드를 처음봤을때는  `->`이 `,`의 오타가 아닐까 생각했다.   
 
하지만 오타가 아니었다.

괄호 함축(implied parentheses)을 사용하면 좀더 이해하기 쉬울 것이다.

```elm
add : Int -> (Int -> Int)
```

`add`함수는 1개의 `Int`파라미터를 사용한다. 그리고 1개의 `Int`파라미터를 받고 `Int`를 리턴하는 함수를 리턴한다.

여기 괄호 함축(implied parentheses)을 사용한 또 다른 타입 명시 예제를 살펴보자.

```elm
doSomething : String -> (Int -> (String -> String))
doSomething prefix value suffix =
    prefix ++ (toString value) ++ suffix
```

`doSomething`함수는 1개의 `String`파라미터를 받아서 1개의 `Int`파라미터를 받는 함수를 리턴하고, 1개의 `String`파라미터를 받고 `String`을 리턴한다.

어떻게 모든 함수들은 1개의 파라미터만 받을까? 그 이유는 Elm에서의 모든 함수는 커링 함수이기 때문이다.

항상 코드는 오른쪽으로 진행되기 때문에 괄호가 꼭 필요한 것은 아니다. 그래서 아래와 같이 간단하게 작성할 수 있다.

```elm
doSomething : String -> Int -> String -> String
```

괄호는 함수를 파라미터로서 전달할때 필수적이다. 괄호가 없다면 타입 명시가 애매모호하다.    
아래 예제를 보자.

```elm
takes2Params : Int -> Int -> String
takes2Params num1 num2 =
    -- do something
```

위의 코드는 아래의 코드와 매우 다르다.

```elm
takes1Param : (Int -> Int) -> String
takes1Param f =
    -- do something
```

`takes2Param`함수는 2개의 파라미터 `Int`와 또 다른 `Int`를 받는다. 반면에 `takes1Param`함수는 한개의 `(Int -> Int)` 파라미터를 받는 함수이다.
여기 `map`을 위한 타입 명시가 있다.

```elm
map : (a -> b) -> List a -> List b
map f list =
    // ...
```

위 코드의 경우 괄호가 필요하다. 왜냐하면 함수 f의 타입이 `(a -> b)`이기 때문이다.    
즉, 이 함수는 타입이 `a`인 파라미터를 받고, 타입이 `b`인 결과값을 리턴한다.

여기 `a`라는 어떤 타입이 있다. 타입이 대문자라면 그것은 명시적 타입(예 : String)이다.     
타입이 소문자라면 그것은 어떤 타입이든 될 수 있다. 여기서 `a`의 타입이 `String`일수도 있지만, `Int`일수도 있다.   
만약 `(a -> a)`라면, 그것은 input 타입과 output 타입이 **반드시** 같아야 한다는 것을 뜻한다.   

위 코드에 있는 `map`의 경우 `(a -> b)`로 되어있는데, 이는 `a`(input)와 `b`(output)가 같은 타입 일수도, 다른 타입 일수도 있다는 것을 뜻한다.   
일단 `a`의 타입이 결정되면, `a`는 전체 구문에서 반드시 동일해야 한다.

```elm
(Int -> String) -> List Int -> List String
```

위에서 모든 `a`가 `Int`로 대체되었다. 그리고 `b`또한 `String`으로 대체되었다.   
`List Int`리스트에 `Int`타입 들이 포함되었다는 뜻이고, `List String`은 리스트에 `String`타입들이 포함되어 있다는 것을 뜻한다.    
만약 당신이 자바 또는 다른 언어에서 제네릭 타입을 사용한다면 이 개념은 친숙할 것이다.

## 머리 아파! 이제 한계야!

![functional_programming_5_5](./images/functional_programming/functional_programming_5_5.png)

오늘은 여기까지.

다음 마지막 게시물에서는 당신이 지금까지 배운 것들을 실무에서 어떻게 사용할지를 이야기 하려고 한다.

다음 게시물 : [Part 6](https://front-end.me/javascript/functional-programming-6/)

---

글에 번역 오류가 있으면 알려주세요. 감사합니다.
