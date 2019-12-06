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

#### TAKEWHILE활용

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
                .collect(toList()); //seasonal fruit, prawns목
```

위와 같이 filter를 사용하면 모든 스트림을 반복하면서 각 요소에 프리디케이트를 적
