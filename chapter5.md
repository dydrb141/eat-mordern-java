# Chapter5

## 스트림 활용

### 5.1 필터링

#### 프리디케이트로 필터링

```java
List<Dish> vegetarianMenu = menu.stream()
                                .filter(Dish::isVegetarian)
                                .collect(toList());
```

#### 고유 요소 필터링

distinct메서드를 이용하면 고유한 스트림을 반환할 수 있다.

다음 코드는 모든 짝수를 선택하고 중복을 필터링

```java
List<Integer> numbers = Arrays.asList(1, 2, 1, 3, 3, 2, 4);
numbers.stream()
        .filter(i -> i % 2 == 0)
        .distinct()
        .forEach(System.out::println);
```

### 5.2 스트림 슬라이싱

#### 프리디케이트를 이용한 슬라이싱

자바 9은 스트림의 요소를 효과적으로 선택할 수 있도록 takeWhile, dropWhile 지원

#### TAKEWHILE 활용

```java
List<Dish> specialMenu = Arrays.asList(
    new Dish("seasonal fruit", true, 120, Dish.Type.OTHER),
    new Dish("prawns", false, 300, Dish.Type.FISH),
    new Dish("rice", true, 350, Dish.Type.OTHER),
    new Dish("chicken", false, 400, Dish.Type.MEAT),
    new Dish("french freis", true, 530, Dish.Type.OTHER),
    
```

```java
List<Dish> filteredMenu = specialMenu.stream()
                .filter(dish -> dish.getCalories() < 320)
                .collect(toList()); //seasonal fruit, prawns
```

위와 같이 filter를 사용하면 모든 스트림을 반복하면서 각 요소에 프리디케이트를 적용

```java
List<Dish> sliceMenu = specialMenu.stream()
        .takeWhile(dish -> dish.getCalories() < 320)
        .collect(toList()); //seasonal fruit, prawns
```

위와 같이 takeWhile을 사용한다면 스트림 조건 중 false를 만날때 까지만 적용된다.

#### DROPWHILE 활용

takeWhile과 정 반대의 작업을 수

```java
List<Dish> sliceMenu = specialMenu.stream()
        .dropWhile(dish -> dish.getCalories() < 320)
        .collect(toList()); //rice, chicken, french fries
```

takeWhile은 false를 만날때 까지만 스트림을 반환하고 dorpWhile은 false가 나올때 부터 스트림을 반환

#### 스트림 축소

최대 n개의 요소를 반환 할 수 있다.

```java
List<Dish> filteredMenu = specialMenu.stream()
                .filter(dish -> dish.getCalories() < 300)
                .limit(3)
                .collect(toList()); //rice, chicken, french fries
```

#### 요소 건너뛰기

결과 스트림에 n개를 스킵하고 나머지 결과를 보여줌

스킵의 n값이 결과 갯수보다 커도 스킵 함. 스킵해서 결과가 없으면 빈 스트림을 반환

```java
List<Dish> filteredMenu = specialMenu.stream()
                .filter(dish -> dish.getCalories() < 300)
                .skip(3)
                .collect(toList());
```

### 5.3 매핑

#### 스트림의 각 요소에 함수 적용

* map은 인수로 제공된 함수는 각 요소에 적용되며 함수를 적용한 결과가 새로운 요소에 매핑
* 고친다라는 개념보단 새로운 버전을 만든다는 개념

#### 스트림 평면화

\["Hello", "World"\] 리스트가 있다고 가정 했을때 Stream&lt;String&gt;을 리턴 받고 싶을 때 아래 코드처럼 ㅏㄴ다면 Stream&lt;String\[\]&gt; 형식이 된다.왜냐하면 split가 반환하는 형식이 String\[\]이기 때문이다.

```java
word.stream()
    .map(word -> word.split(""))
    .distinct()
    .collect(toList());
```

#### flatMap사용 

flatMap은 평면화된 스트림을 반환



```java
List<String> uniqueCharacters = words.
                                     .map(word -> word.split(""))
                                     .flatMap(Arrays::stream)
                                     .distinct()
                                     .collect(toList());
```

