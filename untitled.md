# Chapter2

## 동작 파라미터화\(behavior parameterization\)

아직은 어떻게 실행할 것인지 결정하지 않은 코드 블록 자주 바뀌는 요구사항에 효과적으로 대응

선택 조건을 결정하는 인터페이스

```java
 public interface ApplePredicate { 
    boolean test (Apple apple); 
 }
```

```java
public class AppleHeavyWeighPredicate implements ApplePredicate { 
    public boolean test(Apple apple) { 
        return apple.getWeight() > 150; //무거운 사과만 선택 
    } 
 }
```

```java
public class AppleGreenColorPredicate implements ApplePredicate { 
    public boolean test(Apple apple) { 
        return GREEN.equals(apple.getColor()); //녹색 사과만 선택 
    } 
}
```

전략 디자인 패턴 : 각 알고리즘\(전략\)을 캠슐화 하는 알고리즘 패밀리를 정의해둔 다음 런타임에 알고리즘을 선택하는 기법

```java
public static List filter(List inventory, ApplePredicate p) { 
    List result = new ArrayList<>(); 
    
    for (Apple apple : inventory) { 
        if (p.test(apple)) { 
                result.add(apple); 
        } 
    } 
    return result; 
}
```



### 코드/동작 전달하기

```java
static class AppleRedAndHeavyPredicate implements ApplePredicate { 
    @Override 
    public boolean test(Apple apple) { 
        return apple.getColor() == Color.RED && apple.getWeight() > 150; 
    } 
}
List redAndHeavyApples = filter(inventory, new AppleRedAndHeavyPredicate);
```



위 소스는 여러 클래스를 정의한 다음 인스턴스화 해야되기 때문에 상당히 번거롭다

### 익명 클래스

자바의 지역 클래스와 비슷한 개념

```java
List redApples2 = filter(inventory, new ApplePredicate() { 
    @Override
     public boolean test(Apple a) { 
        return a.getColor() == Color.RED; 
     } 
 });
   System.out.println(redApples2); 
}
```

익명 클래스는 많은 공간을 차지해 가독성이 떨어진다.

### 람다 표현식 사용

List result = filterApples\(inventory, \(Apple apple\) -&gt; apple.getColor\(\) == Color.RED\);

### 리스트 형식 추상화

제네릭 T를 사용해 형식을 추가하면 좀더 유연해짐

