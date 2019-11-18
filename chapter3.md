# chapter3

## 3.1 람다란 무엇인가?
람다 표현식은 메서드로 전달할 수 있는 익명 함수를 단순화 한것

## 3.2.2 함수 디스크립터
람다 표현식의 시그니처를 서술하는 메서드를 함수 디스크립터라고 함
함수형 인터페이스의 추상 메서드 시크니처
ex) Runnable 은 인수와 반환값이 없으므로 Runnable인터페이스는 인수와 반환값이 없는 시그니처다.

## 3.3 람다 활용 : 실행 어라운드 패턴
설정과 정리 두 과정이 둘러싸는 형태
ex) 초기화/준비코드 - 작업 A - 정리/마무리 코드
    초기화/준비코드 - 작업 B - 정리/마무리 코드

## 3.4.1 Predicate
* java.util.function.Predicate<T> 인터페이스는 test 추상 메서드 정의
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
* java.util.function.Consumer<T> 
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
* java.util.function.Function<T, R>
* 제네릭 형식 T를 인수로 받음
* R객체를 반환하는 추상 메서드 apply정의





   

