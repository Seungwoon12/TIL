# Volatile
volatile은 Java변수를 Main Memory에 저장하겠다고 명시하는 것이다. 이렇게 하면 매번 변수의 값을 read할 때 CPU cache에 저장된 값이 아닌 Main Memory에서 읽는다. 또한, 변수의 값을 write할 때마다 Main Memory에 까지 작성한다.  

## - 사용이유
volatile 변수를 사용하고 있지 않은 멀티쓰레드 환경에서 작업을 수행하는 동안 성능 향상을 위해서 Main Memory에서 읽은 변수를 CPU Cache에 저장하게 된다. 만약 멀티쓰레드 환경이라면 쓰레드가 변수 값을 읽어올 때 각각의 CPU Cache에 저장된 값이 다르기 때문에 변수 값 불일치 문제가 발생하게 된다. 즉, 한 쓰레드에서 업데이트가 발생했을 때 다른 쓰레드에 표시되지 않는 것이다. 이것을 방지하고 Main Memory에 값을 저장하고 읽어오기 위해 변수에 volatile 키워드를 추가한다. 

## - 사용이 필요한 상황
멀티쓰레드 환경에서 하나의 쓰레드만 read & write을 하고 나머지 쓰레드가 read하는 상황에서 가장 최신의 값을 보장하기 위해 사용한다. CPU Cache 보다 Main Memory가 비용이 더 크기 때문에 변수 값의 일치를 보장해야 하는 경우에만 volatile을 사용하는 것이 좋다.