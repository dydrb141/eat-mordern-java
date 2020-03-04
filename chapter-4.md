# Chapter 4

## 4.1 스트림이란 무엇인가?

데이터 처리 연산을 지원하도록 소스에서 추출된 연속된 요소

2가지 특징

* 파이프라이닝 : 게으름, 숏서킷등 최적화 제공 
* 내부 반복: 내부 반복 자

### 스트림과 컬렉션 차이

* 컬렉션의 모든 요소는 컬렉션에 추가 하기전에 계산 되어야 함.
* 스트림은 요청할 때만 요소를 계산
* 데이터를 언제 계산하느냐가 켈렉션과 스트림의 가장 큰 차이이다.
* 컬렉샨은 현재 자료구조가 포함하는 모든 값을 메모리에 저장하는 자료구조이다.
* 반면 스트림은 이론적으로 <u>**요청할 때만**</u> 요소를 계산하는 고정된 자료구조이다.

#### 딱 한번만 탐색할 수 있다.


### java collection 외부 반복


```
List<String> names1 = new ArrayList<>();
        for(Dish dish:menu){
            names1.add(dish.getName());
        }
```


###스트림 내부 반복

```

List<String> names = menu.stream()
                            .map(Dish::getName)
                            .collect(toList());
```

java collection의 외부 반복과 Stream에 내부 반복의 차이는 컬렉션은 컬렉션 항목을 하나씩
가져와서 처리한다.<br>
반면에 Stream은 동시에 내부적으로 투명하게 병렬적으로 처리하거나 최적화된 다양한 순서로 처리할 수 있다.


## 스트림 연산

아래의 예제에서 연산은 두 그룹으로 구분할 수 있다.

### 중간 연산

filter , map , limit는 서로 연결되어 파이프라인을 형성

### 최종 연산

collect로 파이프라인을 실행한 다음에 닫는다.

```java
public class Menu {
    public static void main(String[] args) {
        List<Dish> menu = Arrays.asList(
                new Dish("pork",false,800,Dish.Type.MEAT),
                new Dish("beef",false,700,Dish.Type.MEAT),
                new Dish("chichken",false,400,Dish.Type.MEAT),
                new Dish("french fires",true,530,Dish.Type.OTHER),

                new Dish("rice",true,350,Dish.Type.OTHER),
                new Dish("season fruit",true,120,Dish.Type.OTHER),
                new Dish("pizza",true,550,Dish.Type.OTHER),
                new Dish("prawns",false,300,Dish.Type.FISH),
                new Dish("salmon",false,450,Dish.Type.FISH)
        );


        List<String> threeHighCaloricDishNames =
                menu.stream()
                    .filter(dish -> dish.getCalories() > 300)
                    .map(Dish::getName)
                    .limit(3)
                    .collect(toList());

    }
}
```

