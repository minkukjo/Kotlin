# Kafka VS Rabbit MQ

Kafka와 RabbitMQ 모두 메세지큐이다.

메세지큐란 무엇일까?

메세지큐의 목적은 메세지를 한곳에 던지고 그것을 필요한 주체가 가져가서 처리하기 위함이다.

메세지큐는 다양한 분야에서 사용되며 특히 분산시스템에서 메세지큐는 확장가능한 서비스 아키텍처를 구축하는데 필수적으로 사용되어진다.

지금부터 메세지큐를 사용하는 장점에 대해서 알아보자

### 메세지큐의 장점

- Asyncronous : 큐에 넣기 때문에 비동기 처리가 가능하다.
- Decoupling : 어플리케이션과 분리될 수 있다.
- Resilience : 일부의 실패가 전체에 영향을 주지 않는다.
- Redundancy : 실패시 재실행 가능
- Gurantees : 작업 처리를 확인 가능
- Scalable : 다수의 프로세스들이 큐에 메세지를 보낼 수 있다.

<img width="642" alt="스크린샷 2020-02-03 오후 7 15 33" src="https://user-images.githubusercontent.com/43809168/73644897-97544d80-46b9-11ea-9dc7-3f0cb1e1d07e.png">

메시지큐는 일반적으로 위와같은 과정을 거친다.

메세지를 바로 던지는 것이 더 빠르고 성능이 더 좋을 수도 있지만, 반대로 순차적으로 메세지를 처리해야하는 상황이 있을 수도 있다.

엘라스틱 서치의 경우 초당 처리 가능한 커넥션 풀의 크기가 정해져 있다.

만약 초당 많은 처리를 ES에 요청하게 될 경우 ES의 한계 처리량을 넘어설 경우 ES에 부하를 줄 수 있다.

때문에 메세지큐를 이용함으로써 ES의 처리를 순차적으로 진행하게 할 수 있다.

메시지큐의 사용 이유에 대해서 간략히 알아보았으니 지금부터 본격적으로 RabbitMQ와 Kafka를 비교해보자.

RabbitMQ와 비교하는 이유는 메시지큐에서 가장 유명하기 때문이다.

## Rabbit MQ의 장점

- 구성이 쉽다
- 유연한 라우팅이 가능하며 UI가 간편하다
- 20k/sec를 보장
- 제품의 성숙도가 높다
- 필요에 따라 동기/비동기 가능

## Kafka의 장점

- 구독방식의 비동기 구성
- 고성능 고가용성
- 분산처리에 효과적으로 설계
- 100k/sec 처리를 보장

## 결론

분산/대용량/고성능/장애 대응이 필요하다면 **Kafka**

큐의 다양한 기능이 필요하다면 **Rabbit MQ**

## Reference

https://ellune.tistory.com/29