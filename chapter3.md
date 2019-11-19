# chapter3

## 3.1 람다란 무엇인가?

람다 표현식은 메서드로 전달할 수 있는 익명 함수를 단순화 한것

## 3.2.2 함수 디스크립터

람다 표현식의 시그니처를 서술하는 메서드를 함수 디스크립터라고 함 함수형 인터페이스의 추상 메서드 시크니처 ex\) Runnable 은 인수와 반환값이 없으므로 Runnable인터페이스는 인수와 반환값이 없는 시그니처다.

## 3.3 람다 활용 : 실행 어라운드 패턴

설정과 정리 두 과정이 둘러싸는 형태 ex\) 초기화/준비코드 - 작업 A - 정리/마무리 코드 초기화/준비코드 - 작업 B - 정리/마무리 코드

## 3.4.1 Predicate

* java.util.function.Predicate 인터페이스는 test 추상 메서드 정의
* test는 제네릭 형식 T객체를 받아 불리언 반환

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}

public <T> List<T> filter(List<T> list, Predicate<T> p) {
    List<T> results = new ArrayList<>();

    for(T t : list) {
        if (p.test(t)) {
            results.add(t);
        }
    }

    return results;
}

Predicate<String> nonEmptyStringPredicate = (String s) -> !s.isEmpty();
List<String> nonEmpty = filter(listOfStrings, nonEmptyStringPredicate);
```

## 3.4.2 Consumer

* java.util.function.Consumer 
* 제네릭 형식 T 객체를 받아서 void를 반환
* accepct 추상 메서드 정의
* T형식의 객체를 인수로 받아서 어떤 동작을 수행하고 싶을때

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}

public <T> void forEach(List<T> list, Consumer<T> t) {
    for(T t : list) {
        c.appect(t);
    }
}

forEach(Arrays.asList(1, 2, 3, 4), (Integer i) -> System.out.println(i));
```

## 3.4.3 Function

* java.util.function.Function
* 제네릭 형식 T를 인수로 받음
* R객체를 반환하는 추상 메서드 apply정의

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}

public <T, R> List<R> map(List<T> list, Function<T, R> f) {
    List<R> result = new ArrayList<>();

    for(T t : list) {
        result.add(f.apply(t));
    }

    return result;
}

List<Integer> l = map(Arrays,asList("lamda", "in"), (String s) -> s.length());
```

| 사용 사례 | 람다 예제 | 대응하는 함수 인터페이스 |
| :--- | :---: | ---: |
| 불리언 표현 | \(List list.isEmpty\(\) | Predicate&gt; |
| 객체 생성 | \(\) -&gt; new Apple\(10\) | Supplier |
| 객체에서 소비 | \(Apple a\) -&gt; System.out.println\(a.getWeight\(\) | Consumer |
| 객체에서 선택 추출 | \(String s\) -&gt; s.length\(\) | Function or ToIntFunction |
| 두 값 조합 | \(int a, int b\) -&gt; a \* b | IntBinaryOperator |
| 두 객체 비교 | \(Apple a1, Apple a2\) -&gt; a1.getWeght\(\).compareTo\(a2.getWeight\(\)\) | BiFunction or ToIntBiFunction |

## 3.5 형식 검사, 형식 추론, 제약

### 3.5.1 형식 검사

* 람다가 사용되는 콘텍스트를 이용해서 람다의 형식을 추론 가능
* 람다의 표현식의 형식을 대상 형식

람다 표현식을 사용할때 형식 확인 과정

```java
List<Apple> heavierThan150g = filter(inventory, (Apple apple) -> apple.getWeight() > 150);
```

1. filter 메서드 선언 확인
2. filter 메서드는 두 번째 파라미터로 Predicate 형식\(대상 형식\)을 기대 
3. Predicate은 test라는 한 개의 추상 메서드를 정의한 함수형 인터페이스
4. test 메서드는 Apple을 받아 boolean을 반환하는 함수 디스크립터를 묘사
5. filter 메서드로 전달된 인수는 이와 같은 요구사항 만

### 3.5.2 형식 추론

대상 형식을 이용해 함수 디스크립터를 알 수 있으므로 컴파일러는 람다의 시그니처도 추론할수 있다.

```java
ex) Comparator<Apple> c = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()); //형식 추론 안함
ex) Comparator<Apple> c = (a1,  a2) -> a1.getWeight().compareTo(a2.getWeight()); //형식 추론
```

### 3.5.4 지역 변수 사용

람다 캡쳐링

```java
int portNumber = 1337; //자동 final
Runnable r = () -> System.out.println(portNumber);
```

#### 지역 변수의 제약

왜 지역 변수에 이런 제약이 필요하냐면 말이다?

* 인스턴스 변수는 어디에 저장되냐 힙에 저장된다 이거야
* 지역 변수는 어디에 저장되냐 하면 스택에 저장 된다 이거야
* 람다에서 지역 변수에 바로 접근 할수 있다 치자 이거야 람다가 스레드에서 실행된다면  지역 변수를 할당한 스레드가 사라져버리면 우짤거냐 이그야
* 그래서 자바에서는 지역 변수의 복사본을 제공한다 이그야 

#### 클로저

* 클로저란 함수의 비지역 변수를 자유롭게 참조할 수 있는 함수의 인스턴스를 가리킨다.

## 3.6 메소드 참조

### 3.6.1 요약

* 특정 메소드만 호출하는 람다의 축약형
* 가독성을 높임

#### 메소드 참조를 만드는 법

1. 정적 메소드 참조
   * ex\) Integer의 parseInt -&gt; Integer::parseInt 
2. 다양한 형식의 인스턴스 메서드 참조
   * ex\) String 의 length -&gt; String::length
3. 기존 객체의 인스턴스 메서드 참조
   * ex\) Transaction expensiveTransaction = new Transaction\(\) -&gt; expensiveTransaction::getValue

## 3.8 람다 표현식을 조합할 수 있는 유용한 메서드

### Comparator조합

Comparator.comparing을 이용해서 비교에 사용할 키를 추출 Comparator 의 comparing은 디폴트 메서

```java
Comparator<Apple> c = Comparator.comparing(Apple::getWeight);
```

#### 역정렬

Comparator 에서 reverse 디폴트 메소드를 제공

```java
inventory.sort(comparing(Apple::getWeight).reversed());
```

#### thenComparing

* 소팅할때 비교할 값이 같을때 두 번째 비교자를 사용할 수 있다.

  ```java
  inventory.sort(comparing(Apple::getWeight).reversed().tehComparing(Apple::getCountry)); //두 사과의 무게가 같으면 국가별로 정렬
  ```

  **Predicate조합**

  * negate, and, or 메서드 제공 
  * negate : 기존 프리디케이트 객체를 반전
  * and : 두 프리디케이트를 연결 새로운 프리디케이트 객체 만듬
  * or : 프리디 케이트를 연결해서 더 복잡한 프리디케이트 객체를 만듬

### Function 조합

* andThen과 compose 두 가지 디폴트 메서드 제공

#### andThen 조합

* 숫자를 증가\(x -&gt; x + 1\) 시키는 f 함수
* 숫자에 2를 곱하는 g함수
* f와 g를 조립해서 숫자를 증가 시킨뒤 결과에 2를 곱한

```java
Function<Integer, Integer> f = x -> x + 1;
Function<Integer, Integer> g = x -> x * 2;
Function<Integer, Integer> h = f.andThen(g);
int result = h.apply(1);  //4
```

#### compose

* 인수로 주어진 함수를 먼저 실행한 다음 결과를 외부 함수 인수로 제공
* 즉 andThen\(g\) = g\(f\(x\)\) 지만 compose는 f\(g\(x\)\)된다

```java
Function<Integer, Integer> f = x -> x + 1;
Function<Integer, Integer> g = x -> x * 2;
Function<Integer, Integer> h = f.compose(g);
int result = h.apply(1);  //3을 반환
```

