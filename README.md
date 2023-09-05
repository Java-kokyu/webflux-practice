# Spring Webflux

### 서론

웹툰 회사에 관심이 많아서, 웹툰 회사의 백엔드 채용 공고를 보던 중 Spring을 사용하는 회사의 대부분에는 `Spring Webflux` 가 필수 스택으로 들어가 있었다. 현재 다니고 있는 회사에서는 MVC 패턴만 사용하여 한 번도 사용하지 않았던 기술이라 궁금하기도 했고, 공부해보면 좋을 것 같아 Spring Webflux에 관한 글을 작성하기로 했다.

### Spring Webflux란?

Spring Webflux는 Spring Framework 5부터 추가된 비동기 Reactive 웹 프레임워크이다. 이전에는 Spring MVC가 웹 어플리케이션을 개발하기 위해 사용되었지만 Spring Webflux는 MVC와는 다르게 Reactive 프로그래밍 모델을 사용하며, 이를 통해 비동기 처리와 이벤트 기반의 Non-blocking I/O 처리를 지원한다.

Non-blocking I/O란, 요청을 보낸 클라이언트 측에서 응답을 받을 때까지 기다리지 않고 다른 작업을 처리할 수 있는 I/O 방식을 말한다. Spring Webflux에서는 Reactive Stream 구현체를 사용하여 Non-blocking I/O를 지원한다. 이를 통해 요청이 많은 대규모 어플리케이션에서도 빠른 응답 처리와 서버의 확장성을 제공할 수 있다. 이와 달리 Spring MVC는 Blocking I/O 방식을 사용하며, 요청에 따라 새로운 스레드를 생성하고, 스레드 풀에서 작업을 처리하는 방식을 사용하기 때문에 요청이 많을 경우 서버의 성능 저하가 발생할 수 있다.

Spring MVC는 **동기식 처리 모델을 사용하며, Blocking I/O 방식으로 동작**하는데 반해 Spring Webflux는 **Reactive Stream 구현체를 통해 비동기, Non-blocking I/O 방식으로 동작**한다. 그렇기 때문에 Spring Webflux는 **대용량 트래픽 처리에 뛰어난 성능과 확장성을 제공**한다.

### Reactive란?

Reactive는 프로그래밍 패러다임 중 하나로, 비동기 데이터 스트림을 처리하기 위한 모델이다. 기존의 명령형 프로그래밍 패러다임과는 달리, Reactive 프로그래밍 패러다임은 데이터가 발생할 때마다 이를 처리하는 함수를 호출하고, 이벤트가 발생하지 않으면 대기하고 있는 모델이다. 이러한 방식으로 Reactive 프로그래밍 패러다임은 Non-blocking I/O 처리와 비동기 처리를 지원하며, 대용량 트래픽 처리와 서버의 확장성을 제공한다.

Reactive 프로그래밍 패러다임은 Reactive Stream 표준 인터페이스 규격을 따르며, Reactor 라이브러리를 사용하여 구현할 수 있다. Reactor 라이브러리에서는 Mono와 Flux 두 가지 타입을 제공하며, Mono는 0 또는 1개의 결과 값을 방출할 수 있는 단일 스트림이며, Flux는 0개 이상의 결과 값을 방출할 수 있는 다중 스트림이다. Reactor 라이브러리에서는 flatMap, map, filter 등 다양한 연산자를 제공하여 비동기 데이터 스트림 처리를 쉽게 구현할 수 있게 도와준다.

따라서 Reactive 프로그래밍 패러다임은 명령형 프로그래밍 패러다임과는 달리 데이터를 처리하는 방식에서 차이가 있다. Reactive 프로그래밍 패러다임은 데이터가 발생할 때마다 이를 처리하는 함수를 호출하고, 이벤트가 발생하지 않으면 대기하고 있는 모델이다. 이러한 방식으로 Reactive 프로그래밍 패러다임은 Non-blocking I/O 처리와 비동기 처리를 지원하며, 대용량 트래픽 처리와 서버의 확장성을 제공한다.

### Reactive Stream 구현체, Mono와 Flux

`Reactive Stream`은 비동기 데이터 스트림을 처리하기 위한 표준화 된 인터페이스 규격이다. 이를 구현하기 위해 다양한 라이브러리가 있으며, 그 중 `Reactor` 라이브러리가 대표적이다.

`Reactor`에서는 `Mono`와 `Flux`라는 두 가지 타입을 제공하는데, `Mono`는 0 또는 1개의 결과 값을 방출할 수 있는 단일 스트림이며, `Flux`는 0개 이상의 결과 값을 방출할 수 있는 다중 스트림이다.

예를 들어, 단일 스트림인 `Mono`를 사용하여 비동기적으로 데이터를 조회하고 이를 처리하는 코드는 아래와 같다.

```java
Mono<User> userMono = userRepository.findById(userId);
userMono.subscribe(user -> {
    // user 정보를 처리하는 로직
});

```

위 코드에서 `findById` 메서드는 `Mono<User>` 타입의 결과 값을 반환한다. 이를 `subscribe` 메서드로 구독하면 결과 값을 비동기적으로 처리할 수 있다.

`Flux` 타입을 사용하면 여러 값을 단일 스트림으로 처리할 수 있다. 예를 들어, `findAll` 메서드로 조회한 여러 건의 데이터를 `Flux`로 처리하는 코드는 아래와 같다.

```
Flux<User> userFlux = userRepository.findAll();
userFlux.subscribe(user -> {
    // user 정보를 처리하는 로직
});

```

`Reactor` 라이브러리에서는 `flatMap`, `map`, `filter` 등 다양한 연산자를 제공하여 비동기 데이터 스트림 처리를 쉽게 구현할 수 있게 도와준다.
