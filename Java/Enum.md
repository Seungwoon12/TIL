# Enum(열거형)
## - Enum에 대해..
- 열거형 클래스의 생성자는 외부에서 호출할 수 없다. 즉, 열거형 클래스의 인스턴스를 생성할 수 없다. 열거형 클래스의 생성자의 접근제어자는 암묵적으로 private이다. 

- 열거형 상수에 여러값을 지정하고 싶으면 그 값에 해당하는 인스턴스 변수를 만들어주고 생성자를 만들어주면 된다.

- 열거형은 상수간에 ==로 비교가 가능하다. equals가 아닌 ==로 비교할 수 있기 때문에 성능이 더 좋다. >, <와 같은 비교연산은 불가능.

- ordinal() 메소드는 웬만하면 사용하지말기. 열거형 상수에 매칭되는 정수가 필요하다면 인스턴스 변수를 만들고 생성자를 만들어주면 된다. 

## - Enum 클래스에 있는 메소드

```java
    - Class<E> getDeclaringClass() : 열거형의 Class객체를 반환.
    - String name() : 열거형 상수 이름을 문자열로 반환.
    - int ordinal() : 열거형 상수가 정의된 순서를 반환.(0부터 시작함)
    - T valueOf(Class<T> enumType, String name) : 매개변수로 지정된 열거형에서 name과 일치하는 상수를 반환. 
```