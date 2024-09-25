---
author: "seonuk1218"
title: "Kafka researching"
image: ""
draft: false
date: 2024-09-20
description: ""
tags: ["kafka", "message-broker"]
archives: ["2024/09"]
---


### 기존 MQ와 Kafka로의 발전: 개요

#### 기존 MQ(Message Queue) 솔루션

**Message Queue(MQ)**는 분산 시스템에서 **비동기식 메시지 전달**을 위한 대표적인 솔루션이다. MQ는 **생산자(Producer)**와 **소비자(Consumer)** 간의 메시지 중개 역할을 하며, 데이터를 안전하게 전달하는 데 중점을 둔다. 대표적인 MQ 솔루션으로는 **RabbitMQ**, **ActiveMQ** 등이 있다.

기존 MQ의 주요 특징:
1. **포인트-투-포인트(PTP) 모델**: 하나의 생산자가 메시지를 큐에 넣고, 하나의 소비자가 해당 메시지를 처리하는 방식. 메시지는 오직 한 번 소비된다.
2. **퍼블리시-서브스크라이브(pub-sub) 모델**: 생산자가 메시지를 발행하면, 여러 소비자가 이를 동시에 구독할 수 있다.
3. **메시지 보장**: 대부분의 MQ 시스템은 **메시지의 손실 없이 전달**을 보장하며, 이를 위해 메시지를 **디스크에 저장**하여 안전하게 유지한다.
4. **순차성 보장**: 일부 MQ는 메시지의 처리 순서를 보장하며, 소비자가 메시지를 받은 후에 큐에서 삭제한다.

하지만, 기존의 MQ 솔루션은 **대규모 데이터 처리**나 **실시간 데이터 스트리밍**의 요구사항을 충족하는 데 제한적이었다. 예를 들어, **수평적 확장성**, **높은 처리량**, **고가용성**에서 MQ 시스템은 제약이 있었다.

---

### Kafka의 등장과 발전

**Apache Kafka**는 **LinkedIn**에서 2011년에 개발되었으며, 기존 MQ 솔루션의 한계를 극복하고 **실시간 스트리밍 데이터 처리**와 **대규모 메시지 처리**를 위해 설계된 **분산형 스트리밍 플랫폼**이다. Kafka는 단순한 메시지 큐를 넘어 **분산형 로그 시스템** 및 **데이터 스트리밍 처리** 솔루션으로 발전했다.

Kafka의 주요 특징은 다음과 같다:

#### 1. **고성능 및 확장성**
Kafka는 **분산형 아키텍처**를 기반으로 높은 처리량을 지원한다. 기존 MQ 솔루션과 달리 Kafka는 **수평적으로 확장** 가능하며, **클러스터**를 통해 데이터를 여러 **파티션(partition)**으로 나누어 저장하고 처리한다. 이를 통해 **수백만 건의 메시지**를 초당 처리할 수 있다.

- **파티셔닝(partitioning)**: 메시지를 여러 파티션에 분산시켜 병렬로 처리함으로써 높은 처리량을 유지할 수 있다. 각 파티션은 독립적인 메시지 큐처럼 동작하며, 이를 통해 메시지 병렬 처리가 가능하다.
  
#### 2. **내구성 및 고가용성**
Kafka는 메시지를 **디스크에 영구 저장**하는 구조로 설계되어 있으며, 메시지는 기본적으로 **로그 파일**에 기록된다. 이를 통해 데이터 손실 없이 안정적으로 메시지를 저장할 수 있으며, **복제(replication)** 기능을 통해 **고가용성**을 보장한다. 여러 브로커에 메시지를 복제하여 브로커 장애 시에도 데이터 손실이 발생하지 않도록 한다.

#### 3. **실시간 스트리밍**
Kafka는 단순한 메시지 큐가 아니라 **실시간 스트리밍 처리 플랫폼**으로도 사용된다. Kafka의 **컨슈머 그룹** 모델은 여러 소비자가 동시에 메시지를 소비할 수 있게 하며, **스트리밍 데이터를 처리**하는 데 최적화되어 있다. 이를 통해 **실시간 분석**, **실시간 로그 수집** 등의 작업에 적합하다.

#### 4. **높은 신뢰성 및 메시지 보장**
Kafka는 메시지의 **순차적 처리**와 **일관성**을 보장한다. 메시지는 소비자가 처리한 후에도 **일정 기간 동안 로그에 남아** 있어, 필요에 따라 동일한 메시지를 다시 처리할 수 있다. 기존 MQ와 달리 Kafka는 **메시지를 삭제하지 않고 로그로 남겨두는** 특성을 가지므로, 소비자가 언제든지 과거 메시지를 재처리할 수 있다.

#### 5. **디커플링(Decoupling)**
Kafka는 생산자와 소비자가 느슨하게 결합되어 있어, 시스템 간의 의존성을 최소화할 수 있다. 생산자는 메시지를 Kafka 브로커에 전달하고, 소비자는 메시지가 언제든지 준비되었을 때 가져갈 수 있다. 이는 기존 MQ 시스템에서도 중요한 개념이지만, Kafka는 이를 대규모로 확장한 시스템이다.

#### 6. **다양한 사용 사례**
Kafka는 기존 MQ와 달리, 다양한 용도로 사용될 수 있다. 예를 들어, **이벤트 스트리밍**, **로그 수집**, **메시지 큐** 역할뿐만 아니라 **데이터 파이프라인**, **데이터 복제** 등에서도 폭넓게 사용된다. Kafka는 데이터 스트리밍의 표준으로 자리 잡으며, 실시간 데이터 통합 및 분석, 대규모 로그 수집, 이벤트 기반 시스템 구축 등에 활용된다.

---

### MQ에서 Kafka로의 발전: 표로 정리

| **항목**                    | **기존 MQ (Message Queue)**                        | **Apache Kafka**                                         |
|-----------------------------|----------------------------------------------------|----------------------------------------------------------|
| **설계 목적**               | 비동기 메시지 전송 및 처리                          | 분산 로그 저장 및 대규모 데이터 스트리밍 처리              |
| **메시지 전달 모델**         | 포인트-투-포인트(PTP), 퍼블리시-서브스크라이브(Pub/Sub) | 퍼블리시-서브스크라이브(Pub/Sub) 및 스트리밍 처리         |
| **메시지 저장**             | 대부분 메모리 기반 (일부 디스크 옵션)                | 디스크에 로그 저장 (내구성 보장)                          |
| **메시지 처리**             | 메시지를 읽은 후 삭제                               | 메시지를 로그로 보관, 읽어도 삭제되지 않음                |
| **확장성**                  | 수직적 확장 (메시지 처리량 증가 시 성능 저하)        | 수평적 확장 가능 (파티셔닝을 통해 클러스터 확장)          |
| **처리량**                  | 상대적으로 낮음                                      | 초당 수백만 건 이상의 메시지 처리 가능                    |
| **순서 보장**               | 보통 순서 보장 (1개의 소비자가 1개의 메시지를 읽음)  | 파티션 내에서는 순서 보장, 파티션 간에는 병렬 처리         |
| **내구성**                  | 메시지를 메모리에 저장하거나 일부 솔루션은 디스크에 기록 | 메시지를 디스크에 영구 저장 (복제 기능으로 장애 대응)     |
| **고가용성**                | 장애 발생 시 복구 복잡                               | 복제(Replication)를 통해 장애에도 메시지 손실 없이 처리   |
| **메시지 유효 기간**        | 메시지 소비 후 삭제                                 | 메시지를 삭제하지 않고 로그로 저장하여 일정 기간 보관     |
| **복제 및 장애 복구**        | 일부 솔루션에서 지원                                | 복제 기능 기본 제공, 클러스터 내 여러 브로커에 복제        |
| **구성 요소**               | 생산자(Producer), 소비자(Consumer), 메시지 큐(Queue) | 생산자(Producer), 소비자(Consumer), 토픽(Topic), 파티션(Partition), 브로커(Broker) |
| **트랜잭션 지원**           | 일부 솔루션에서 지원                                | 메시지 순서 및 일관성 보장을 위한 트랜잭션 지원            |
| **실시간 스트리밍 처리**     | 제한적                                             | 실시간 스트리밍 데이터 처리에 최적화                      |
| **데이터 재처리**           | 메시지 소비 후 재처리 어려움                         | 로그에 저장된 데이터를 언제든지 재처리 가능               |
| **커뮤니티 및 발전**         | 안정적이나 발전 속도 느림                           | 활발한 오픈소스 커뮤니티, 빠른 발전 및 다양한 도구 지원    |
| **사용 사례**               | 단순한 메시지 큐, 비동기 작업                       | 로그 수집, 실시간 분석, 데이터 스트리밍, 대용량 메시징 시스템 |

---

이 표는 기존 MQ 시스템과 Apache Kafka의 주요 차이점을 비교한 것이다. Kafka는 기존 MQ의 한계를 극복하고, **확장성**, **신뢰성**, **대규모 데이터 처리**에 초점을 맞추어 설계된 솔루션으로, **실시간 데이터 스트리밍** 및 **대용량 메시징**에 특히 강점을 가지고 있다.

### Kafka의 구조

Kafka의 기본 구조는 **브로커(broker)**, **토픽(topic)**, **파티션(partition)**, **프로듀서(producer)**, **컨슈머(consumer)**로 구성된다.

- **브로커(Broker)**: Kafka 서버 역할을 하며, 메시지를 저장하고 클러스터 내의 메시지 전달을 관리한다.
- **토픽(Topic)**: 메시지들이 저장되는 카테고리로, 각 토픽은 여러 파티션으로 나뉘며 병렬 처리가 가능하다.
- **파티션(Partition)**: 토픽 내에서 메시지를 분할하여 저장하는 단위. 각 파티션은 순차적으로 메시지를 기록하며, 여러 파티션이 병렬로 처리되어 높은 처리량을 제공한다.
- **프로듀서(Producer)**: 메시지를 생성하고 Kafka 브로커에 전송하는 역할을 한다.
- **컨슈머(Consumer)**: 메시지를 소비하고 처리하는 역할을 한다. 컨슈머 그룹을 통해 동일한 토픽에서 메시지를 병렬로 처리할 수 있다.

---

### Kafka의 발전

Kafka는 단순한 메시지 큐로 시작되었지만, 현재는 **분산 로그 시스템**과 **스트리밍 데이터 플랫폼**으로 확장되었다. 대규모 실시간 데이터 처리의 필요성이 커지면서, Kafka는 데이터 스트리밍의 표준으로 자리잡았으며, 다양한 산업에서 **데이터 파이프라인**, **실시간 분석**, **이벤트 기반 아키텍처** 구축을 위해 사용된다. Kafka는 기존 MQ 솔루션의 단점을 보완하고, 확장성과 성능, 신뢰성을 극대화한 솔루션으로 진화해 왔다.

---


### Kafka에서 중요하게 사용되는 개념

Kafka는 분산형 메시지 시스템으로, **고성능**과 **확장성**, **신뢰성**을 제공하는 동시에, 여러 핵심 개념들이 중요하게 사용된다. 이를 이해하는 것이 Kafka를 효과적으로 활용하는 데 필수적이다. 아래는 Kafka의 중요한 개념들이다.

#### 1. **토픽(Topic)**

- **토픽**은 메시지를 분류하기 위한 논리적 단위다. Kafka에서 메시지는 특정 토픽에 기록되며, 생산자와 소비자는 특정 토픽을 통해 메시지를 주고받는다.
- 하나의 토픽은 여러 **파티션**으로 나뉘며, 각 파티션은 독립적으로 메시지를 저장하고 처리할 수 있다.

#### 2. **파티션(Partition)**

- **파티션**은 토픽 내에서 메시지를 분산 저장하는 물리적 단위다. Kafka는 파티션을 통해 병렬 처리가 가능하며, 이를 통해 높은 처리량을 보장한다.
- 파티션의 각 메시지는 **오프셋(Offset)**이라는 고유한 ID를 갖고 있으며, Kafka는 이를 통해 메시지의 순서를 보장한다.

#### 3. **브로커(Broker)**

- **브로커**는 Kafka 클러스터의 서버 역할을 하며, 메시지를 저장하고 관리하는 단위다. Kafka 클러스터는 여러 브로커로 구성될 수 있으며, 각 브로커는 파티션을 관리한다.
- 브로커는 클러스터 내에서 메시지를 복제하고, 장애 발생 시에도 데이터의 가용성을 보장한다.

#### 4. **프로듀서(Producer)**

- **프로듀서**는 Kafka로 메시지를 전송하는 역할을 한다. 프로듀서는 메시지를 특정 토픽에 보내며, 각 메시지는 Kafka 브로커에 의해 저장된다.
- 프로듀서는 메시지 전송 시 **파티셔닝 전략**을 지정할 수 있다. 이를 통해 메시지를 특정 파티션에 저장할 수 있다.

#### 5. **컨슈머(Consumer)**

- **컨슈머**는 Kafka에서 메시지를 읽어가는 역할을 한다. 컨슈머는 **컨슈머 그룹(Consumer Group)**으로 메시지를 병렬로 처리할 수 있다.
- 컨슈머는 각 파티션에서 **오프셋**을 기준으로 메시지를 읽으며, 오프셋을 기반으로 메시지 순서 및 처리를 관리할 수 있다.

#### 6. **컨슈머 그룹(Consumer Group)**

- **컨슈머 그룹**은 여러 컨슈머가 함께 메시지를 병렬로 처리하는 구조다. 같은 컨슈머 그룹에 속한 컨슈머들은 토픽 내의 파티션을 나누어 소비하며, 동일한 파티션을 한 번에 하나의 컨슈머만 읽는다.
- 컨슈머 그룹을 사용하면 여러 인스턴스가 동시에 메시지를 처리하면서, 특정 파티션의 메시지를 중복 없이 소비할 수 있다.

#### 7. **리플리케이션(Replication)**

- **리플리케이션**은 Kafka의 고가용성을 보장하는 메커니즘이다. 각 파티션은 여러 개의 **리더(Leader)**와 **팔로워(Follower)**로 복제되며, 리더는 쓰기 및 읽기 요청을 처리하고, 팔로워는 리더를 복제하여 장애 시 데이터를 복구할 수 있게 한다.

#### 8. **오프셋(Offset)**

- **오프셋**은 파티션 내에서 메시지의 고유 ID로, Kafka는 이를 통해 메시지의 순서 및 위치를 관리한다.
- 컨슈머는 오프셋을 기반으로 메시지를 처리하며, 처리된 메시지의 오프셋을 저장하고 다음 오프셋부터 메시지를 소비한다.

---

### Spring Boot와 Kafka를 사용한 예제

Spring Boot는 **Spring Kafka**를 통해 Kafka와 쉽게 통합할 수 있다. 아래는 간단한 예제를 통해 Spring Boot 애플리케이션에서 Kafka를 활용하는 방법을 설명한다.

#### 1. **Spring Boot와 Kafka 의존성 추가**

`pom.xml` 파일에 Kafka 의존성을 추가한다:

```xml
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
```

#### 2. **Kafka 프로듀서 설정**

Spring Boot에서 Kafka 프로듀서를 설정하기 위해 `application.properties` 파일에 Kafka 설정을 추가한다:

```properties
spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.producer.key-serializer=org.apache.kafka.common.serialization.StringSerializer
spring.kafka.producer.value-serializer=org.apache.kafka.common.serialization.StringSerializer
```

이 설정은 Kafka 브로커와의 연결을 구성하며, 키와 값 모두 문자열로 직렬화한다.

#### 3. **Kafka 프로듀서 생성**

Kafka로 메시지를 보내는 프로듀서를 Spring Boot 애플리케이션에 정의한다. `KafkaTemplate`을 사용하여 메시지를 특정 토픽으로 전송할 수 있다.

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.kafka.core.KafkaTemplate;
import org.springframework.stereotype.Service;

@Service
public class KafkaProducer {

    private static final String TOPIC = "my_topic";

    @Autowired
    private KafkaTemplate<String, String> kafkaTemplate;

    public void sendMessage(String message) {
        System.out.println("Sending message: " + message);
        kafkaTemplate.send(TOPIC, message);
    }
}
```

#### 4. **Kafka 컨슈머 설정**

컨슈머를 설정하기 위해 `application.properties`에 추가적인 설정을 한다:

```properties
spring.kafka.consumer.group-id=my-group
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
```

이 설정은 컨슈머가 Kafka의 메시지를 읽기 위해 필요한 정보들을 지정하며, 그룹 ID와 오프셋 초기화 설정을 포함한다.

#### 5. **Kafka 컨슈머 생성**

Kafka에서 메시지를 소비하는 컨슈머를 정의한다. Spring Kafka의 `@KafkaListener` 어노테이션을 사용하여 메시지를 처리할 수 있다.

```java
import org.springframework.kafka.annotation.KafkaListener;
import org.springframework.stereotype.Service;

@Service
public class KafkaConsumer {

    @KafkaListener(topics = "my_topic", groupId = "my-group")
    public void consume(String message) {
        System.out.println("Consumed message: " + message);
    }
}
```

이 컨슈머는 `my_topic`에서 메시지를 소비하고, 메시지를 처리할 때마다 콘솔에 출력한다.

#### 6. **RestController로 메시지 전송 및 소비 테스트**

이제 Spring Boot에서 메시지를 전송하기 위한 간단한 컨트롤러를 정의한다. HTTP 요청을 통해 메시지를 Kafka로 전송할 수 있다.

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class KafkaController {

    @Autowired
    private KafkaProducer producer;

    @GetMapping("/publish")
    public String publishMessage(@RequestParam("message") String message) {
        producer.sendMessage(message);
        return "Message published successfully";
    }
}
```

#### 7. **애플리케이션 실행 및 테스트**

Spring Boot 애플리케이션을 실행하고, 브라우저에서 아래 URL을 호출하여 메시지를 Kafka로 전송한다:

```
http://localhost:8080/publish?message=HelloKafka
```

Kafka 프로듀서는 메시지를 `my_topic`에 전송하며, Kafka 컨슈머는 해당 메시지를 소비하고 콘솔에 출력한다.

---

### 결론

Kafka는 **확장성**, **신뢰성**, **고성능**을 제공하는 분산 메시징 시스템이다. Spring Boot는 **Spring Kafka**를 통해 Kafka와 손쉽게 통합할 수 있으며, 프로듀서와 컨슈머를 쉽게 설정하고 사용할 수 있다. 이 예제는 간단한 Kafka 메시징 시스템의 작동 방식을 보여주었으며, 이를 기반으로 더 복잡한 데이터 스트리밍 애플리케이션을 구축할 수 있다.