# 스트림(Stream)

## - 스트림이란?
스트림이 나오기전에는 컬렉션이나 배열에 데이터를 담고 원하는 결과를 얻기 위해 for문과 Iterator를 이용해서 코드를 작성해야했다. 그러나 이러한 방식으로 작성된 코드는 너무 길고 가독성이 떨어진다. 그리고 재사용성도 떨어진다. 또 다른 문제는 데이터 소스마다 다른 방식으로 다뤄야 한다는 것이다. Collection이나 Iterator와 같은 인터페이스를 이용해서 컬렉션을 다루는 방식을 표준화하기는 했지만 각 컬렉션 클래스에는 같은 기능의 메소드들이 중복해서 정의되어 있다. 예를 들어 List를 정렬할 때는 Collections.sort()를 사용해야하고, 배열을 정렬할 때는 Arrays.sort()를 사용해야한다. 이러한 문제점들을 해결하기 위해서 만든 것이 스트림(Stream)이다. 스트림은 데이터 소스를 추상화하고, 데이터를 다루는데 자주 사용되는 메소드들을 정의해 놓았다. 데이터 소스를 추상화하였다는 것은 데이터 소스가 무엇이던 간에 같은 방식으로 다룰 수 있게 되었다는 것과 코드의 재사용성이 높아진다는 것을 의미한다. 스트림을 이용하면 배열이나 컬렉션뿐만 아니라 파일에 저장된 데이터도 모두 같은 방식으로 다룰 수 있다.

```java
String[] strArr = {"aaa", "bbb", "ccc"};
List<String> strList = Arrays.asList(strArr);

// 스트림 생성 및 데이터 소스를 읽어서 정렬하고 화면에 출력하기
Stream<String> strStream1 = strList.stream();   // 스트림 생성
Stream<String> strStream2 = Arrays.stream(strArr); // 스트림 생성

strStream1.sorted().forEach(System.out::println);
strStream2.sorted().forEach(System.out::println);

// 예전 방식의 정렬 및 출력
Arrays.sort(strArr);
Collections.sort(strList);

for(String str : strArr) {
    System.out.println(str);
}
for(String str : strList) {
    System.out.println(str);
}
```
--> 스트림을 사용한 코드가 간결하고 이해하기 쉬우며 재사용성도 높다.

<br><br>

## - 스트림은 데이터 소스를 변경하지 않는다.
스트림은 데이터 소스로 부터 데이터를 읽기만하며 데이터 소스를 변경하지 않는다. 정렬된 결과를 컬렉션이나 배열에 담아서 반환할 수도 있다.

```java
//아래의 코드는 정렬된 결과를 새로운 List에 담아서 반환한다.
List<String> sortedList = strStream2.sorted().collect(Collectors.toList());
```
<br>

## - 스트림은 일회용이다.
스트림은 Iterator처럼 일회용이다. Iterator로 컬렉션의 요소를 모두 읽고 나면 다시 사용할 수 없는 것처럼 스트림도 한번 사용하면 닫혀서 다시 사용할 수 없다. 필요하다면 스트림을 다시 생성해야한다.

```java
strStream1.sorted().forEach(System.out::println);
int numOfStr = strStream1.count(); // 에러. 스트림이 이미 닫혔음.

// cf) forEach()는 스트림에 정의된 메서드 중의 하나로 매개변수에 대입된 람다식을 데이터 소스의 모든 요소에 적용한다.
```

<br>

## - 스트림의 연산
- 스트림이 제공하는 다양한 연산을 이용해서 복잡한 작업들을 간단히 처리할 수 있다. 마치 데이터베이스에 SELECT문으로 질의하는 것과 같은 느낌이다. 스트림에 정의된 메서드 중에서 데이터 소스를 다루는 작업을 수행하는 것을 연산(Operation)이라고 한다.

- 스트림이 제공하는 연산은 중간 연산과 최종연산으로 분류할 수 있는데, 중간 연산은 연산결과를 스트림으로 반환하기 때문에 중간 연산을 연속해서 연결할 수 있다. 반면에 최종 연산은 스트림의 요소를 소모하면서 연산을 수행하므로 단 한번만 연산이 가능하다.
    - 중간연산 : 연산 결과가 스트림인 연산. 스트림에 연속해서 중간 연산할 수 있다.
    - 최종연산 : 연산 결과가 스트림이 아닌 연산. 스트림의 요소를 소모하므로 단 한번만 가능하다.
    ```java
    String[] strArr = {"dd", "aaa", "CC", "cc", "b"};
    Stream<String> stream = Stream.of(strArr); // 문자열 배열이 소스인 스트림
    Stream<String> filteredStream = stream.filter(); // 걸러내기 (중간연산)
    Stream<String> distinctedStream = stream.distinct(); // 중복제거 (중간연산)
    Stream<String> sortedStream = stream.sort(); // 정렬 (중간연산)
    Stream<String> limitedStream = stream.limit(5); // 스트림 자르기 (중간연산)
    int total = stream.count(); // 요소 개수 세기(최종연산)
    ```

## - 지연된 연산
스트림 연산에서 최종 연산이 수행되기 전까지는 중간 연산이 수행되지 않는다. 스트림에 대해 distinct()나 sort()같은 중간 연산을 호출해도 즉각적인 연산이 수행되는 것이 아니다. 중간 연산을 호출하는 것은 단지 어떤 작업이 수행되어야하는지를 지정해주는 것일 뿐이다. 최종 연산이 수행되어야 비로소 스트림의 요소들이 중간 연산을 거쳐 최종 연산에서 소모된다. 

<br><br>

## - Stream\<Integer> 와 IntStream
요소의 타입이 T인 스트림은 기본적으로 Stream\<T>이지만, 오토박싱&언박싱으로 인한 비효율을 줄이기 위해 데이터 소스의 요소를 기본형으로 다루는 스트림인 IntStream, LongStream, DoubleStream이 제공된다. 일반적으로 Stream\<Integer> 대신 IntStream을 사용하는 것이 더 효율적이고, IntStream에는 Int타입의 값으로 작업하는데 유용한 메서드들이 포함되어 있다. 

<br><br>

## - 병렬 스트림
스트림이 병렬로 연산을 수행하게 하고싶으면 parallel() 메서드를 호출한다. 반대로 병렬로 처리되지 않게 하려면 sequential()을 호출하면 된다. 모든 스트림은 기본적으로 병렬 스트림이 아니므로 sequential()을 호출할 필요가 없다. sequential()은 parellel()을 호출한 것을 취소할 때만 사용한다. parellel()과 sequential()은 새로운 스트림을 생성하는 것이 아니라, 그저 스트림의 속성을 변경할 뿐이다. 

```java
int sum = strStream.parallel() // strStream을 병렬 스트림으로 전환
                                .mapToInt(s -> s.length())
                                .sum();

// cf) 병렬처리가 항상 더 빠른 결과를 얻게 해주는 것이 아니다.                                
```