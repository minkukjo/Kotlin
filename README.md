![apache-kafka](https://user-images.githubusercontent.com/43809168/66971663-0b4bf500-f0cd-11e9-9077-fcaae4ecf658.png)


# 시작하기에 앞서

이 글은 Apache Kafka 공식 문서인 **Apache Kafka Document**를 토대로 작성되었습니다.

[Kafka Security Docuemnt](https://kafka.apache.org/documentation/#security_jaas_broker)

기본적인 Kafka에 대한 개념을 알고 있다는 전제하에 글을 작성할 것이므로, Kafka에 대해서 궁금하시다면 ```andrew```님께서 작성하신

[Kafka 쉽게 이해하기](https://medium.com/@umanking/%EC%B9%B4%ED%94%84%EC%B9%B4%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-%EC%9D%B4%EC%95%BC%EA%B8%B0-%ED%95%98%EA%B8%B0%EC%A0%84%EC%97%90-%EB%A8%BC%EC%A0%80-data%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-%EC%9D%B4%EC%95%BC%EA%B8%B0%ED%95%B4%EB%B3%B4%EC%9E%90-d2e3ca2f3c2)

글을 참고해주시기 바랍니다.

# Apache Kafka Authentication?

오늘은 Apache 재단의 Kafka Authentication에 대해서 알아보도록 하겠습니다.

Kafka는 **SASL** 방식을 이용한 인증방법을 사용하고 있습니다.

SASL은 인증과 보안을 위한 프레임워크 입니다. 

그러면 이러한 SASL 인증은 왜 필요한 것일까요?

Kafka는 Broker의 IP 주소와 Topic만 알고 있다면,

Consumer로 중간에 데이터를  가져갈 수 있고, Producer로 더미 데이터를 Broker에게 던져줄 수 있습니다.

이러한 보안을 위해 Kafka는 SASL 방식으로 인증을 사용하고 있습니다.

Kafka에서는 JAAS(Java Authentication and Authorization Service) 파일을 이용해 SASL에 필요한 설정들을 정의하고 있습니다.

JAAS 파일에는 ID/PASSWORD가 적혀있고, 우리는 이 ID/PASSWORD를 이용해 인증을 하는 것 입니다.

Kafka Broker의 경우 JAAS 파일에 ID/PASSWORD를 명시할 수 있고, Listener Name을 정의할 수 있습니다.

Client의 경우 

1. JAAS를 설정 값으로 넣어주기
2. JAAS 파일 설정 파일을 만드는 방법 

2가지 방법이 있습니다.

첫번째 방법의 경우 JVM 내부에서 Producer/Consumer가 서로 다른 인증 설정값을 가질 수 있다는 장점이 있습니다.
두번째 방법의 경우 설정 값을 JAAS 실제 파일로 만들어서, JVM에 파라미터로 파일을 넘겨주는 방법 입니다.
명령어는 다음과 같습니다. 

```-Djava.security.auth.login.config={JAASFILE}```

# SASL

SASL은 보안 프로토콜인 SASL_PLAIN_TEXT나 SASL_SSL을 사용합니다. SASL_SSL을 사용하는 경우 별도로 SSL를 구성해주어야 합니다.

SASL은 현재 2019년 10월 17일을 기준으로 5가지의 SASL 방식이 있습니다.

- GSSAPI (Kerberos)
- PLAIN
- SCRAM-SHA-256
- SCRAM-SHA-512
- OAUTHBEARER

우리는 여기서 가장 기본이라 할 수 있는 SASL/PLAIN 방식에 대해 알아보도록 하겠습니다.

SASL/PLAIN 방식을 한마디로 정의하면 **ID/PASSWORD를 이용한 인증방식** 입니다.

대신, ID/PASSWORD가 노출되면 안되므로 위에서 언급한 보안 프로토콜을 사용해야 하고, APACHE에서는 SSL을 추천하고 있습니다.


# SASL/PLAIN Broker Configure

그러면 어떻게 SASL/PLAIN 인증방식을 사용하지에 대해 알아보도록 하겠습니다.

위에서 언급한 JAAS 파일을 Broker에서 설정하기 위해서는 Kafka Broker의 Configure 폴더에 아래와 유사한 형태의 JAAS 파일을 추가해야 합니다.

```
KafkaServer {
    org.apache.kafka.common.security.plain.PlainLoginModule required
    username="admin"
    password="admin-secret"
    user_admin="admin-secret"
    user_alice="alice-secret";
};
```


SASL Port와 Mechanism의 경우 아래와 같이 server.properties에 작성하면 됩니다.

```
    listeners=SASL_SSL://host.name:port
        security.inter.broker.protocol=SASL_SSL
        sasl.mechanism.inter.broker.protocol=PLAIN
        sasl.enabled.mechanisms=PLAIN
```

# SASL/PLAIN Client Configure

