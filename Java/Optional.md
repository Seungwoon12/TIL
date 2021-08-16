# Optional\<T>
Optional\<T>는 지네릭 클래스로 T타입의 객체를 감싸는 일종의 래퍼 클래스다. 그래서 Optional 타입의 객체에는 모든 타입의 참조변수를 담을 수 있다. Optional객체에는 null값을 체크하는 메소드가 정의되어 있어서 Optional을 사용하면 매번 if문으로 값이 null인지 체크 할 필요가 없다.

## - Optional객체 생성하기
- optional객체 생성 시 of() 또는 ofNullable()을 사용한다. 만약 참조변수 값이 null일 가능성이 있으면 of()대신 ofNullable()을 사용해야한다. 왜냐하면 of()는 매개변수의 값이 null이면 NullPointerException이 발생하기 때문이다.
```java
String str = "abc";
Optional<String> optVal = Optional.of(str);
Optional<String> optVal = Optional.of("abc");
Optional<String> optVal = optional.of(new String("abc"));

Optional<String> optVal = Optional.of(null); // NullPointerException 발생
Optional<String> optVal = Optional.ofNullable(null); // null 이라도 Ok
```
## - Optional객체의 값 가져오기
- Optional객체에 저장된 값을 가져올 때는 get()을 사용한다. 값이 null일때는 NoSuchElementException이 발생한다. 따라서 이에 대비해 orElse()를 사용하여 대체할 값을 지정할 수 있다.
```java
Optional<String> optVal = Optional.of("abc");
String str1 = optVal.get(); // optVal에 저장된 값을 반환. null이면 예외발생.
String str2 = optVal.orElse(""); // optVal에 저장된 값이 null일때는 ""를 반환
```
- orElse()의 변형으로 null을 대체할 값을 반환하는 람다식을 지정할 수 있는 orElseGet()과 null일때 지정된 예외를 발생시키는 orElseThrow()가 있다.
```java
T orElseGet(Supplier<? extends T> other)
T orElseThrow(Supplier<? extends X> exceptionSupplier)

// 사용하는 방법
String str3 = optVal2.orElseGet(String::new); // () -> new String()과 동일
String str4 = optVal2.orElseThrow(NullPointerException::new); // 널이면 NullPointerException 발생
```

- ifPresent() 사용하기
```java
// 참조변수 str이 null이 아닐때만 값을 출력한다. null이면 아무일도 일어나지 않는다.
Optional.ofNullable(str).ifPresent(System.out::println);
```

## - OptionalInt, OptionalLong, OptionalDouble
- IntStream과 같은 기본형 스트림에는 Optional도 기본형을 값으로 하는 OptionalInt, OptionalLong, OptionalDouble을 반환한다. 기본형 Optional에 저장된 값을 꺼내는 메소드 이름이 다르다는 것에 주의!

|Optional클래스|값을 반환하는 메소드|
|--|--|
|Optional\<T>|T get()|
|OptionalInt|int getAsInt()|
|OptionalLong|long getAsLong()|
|OptionalDouble|double getAsDouble()|

```java
// OptionalInt가 정의된 방식
public final class OptionalInt {
    ...
    private final boolean isPresent; // 값이 저장되어 있으면 true
    private final int value;
}
```

int의 기본값은 0이므로 아무런 값도 갖지 않는 OptionalInt에 저장되는 값은 0일 것이다. 그럼 아래의 두 OptionalInt객체는 같을까?

```java
OptionalInt opt = OptionalInt.of(0); // OptionalInt에 0을 저장
OptionalInt opt2 = OptionalInt.empty(); // OptionalInt에 0을 저장
```
답은 같지 않다. 저장된 값이 없는 것과 0이 저장된 것은 isPresent라는 인스턴스 변수로 구분이 가능하다. isPresent()는 이 인스턴스 변수의 값을 반환한다.

```java
System.out.println(opt.isPresent()); // true
System.out.println(opt2.isPresent()); // false

System.out.println(opt.getAsInt()); // 0
System.out.println(opt2.getAsInt()); // NoSuchElementException 발생

System.out.println(opt.equals(opt2)); // false
```

그러나 Optional객체의 경우 null을 저장하면 비어있는 것과 동일하게 취급한다.
```java
Optional<String> opt = Optional.ofNullable(null);
Optional<String> opt2 = Optional.empty();

System.out.println(opt.equals(opt2));  // true
```