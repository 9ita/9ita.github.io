### Spring Boot Application Monitoring: Logging, Metrics, Tracing

Spring Boot 애플리케이션의 **모니터링**은 시스템 상태를 파악하고, 성능을 최적화하며, 오류를 신속하게 파악하기 위해 필수적이다. 이를 위해 **Logging**, **Metrics**, **Tracing**의 세 가지 요소를 통합하여 모니터링 시스템을 구축할 수 있다.

#### 1. **Logging (로그 관리)**

로그는 애플리케이션에서 발생하는 이벤트나 오류, 정보 등을 기록하는 중요한 모니터링 도구다. Spring Boot는 다양한 로깅 프레임워크를 지원하며, 기본적으로 **SLF4J**와 **Logback**이 통합되어 있다.

##### Spring Boot에서 로그 설정

- **`application.properties`** 또는 **`application.yml`** 파일에서 로깅 수준을 설정할 수 있다.

**`application.properties` 예시**:
```properties
# 기본 로깅 레벨 설정
logging.level.root=INFO
logging.level.org.springframework.web=DEBUG
logging.level.com.example=TRACE

# 콘솔 및 파일 로깅 설정
logging.file.name=app.log
logging.file.path=/var/log/myapp
```

- **SLF4J**를 사용하여 로그를 남길 수 있다.

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Service;

@Service
public class MyService {

    private static final Logger logger = LoggerFactory.getLogger(MyService.class);

    public void performTask() {
        logger.info("Task started");
        try {
            // 작업 수행
            logger.debug("Performing task details...");
        } catch (Exception e) {
            logger.error("Error occurred: ", e);
        }
    }
}
```

##### 외부 로깅 시스템과의 통합

- Spring Boot는 **ELK Stack**(Elasticsearch, Logstash, Kibana) 또는 **Graylog** 같은 외부 로깅 시스템과 쉽게 통합 가능하다.
- **Logstash**를 사용해 로그를 수집하고, **Kibana**를 통해 실시간으로 시각화하여 로그를 분석할 수 있다.

---

#### 2. **Metrics (메트릭 관리)**

**메트릭**은 시스템 성능이나 애플리케이션 상태를 수집하는 데이터로, CPU 사용률, 메모리 사용량, 요청 처리 시간 등을 측정하여 시스템을 모니터링하고 최적화하는 데 도움을 준다.

##### Spring Boot Actuator를 통한 메트릭 수집

Spring Boot는 **Actuator**를 사용해 애플리케이션의 메트릭을 쉽게 수집하고 노출할 수 있다. Actuator는 다양한 메트릭을 제공하며, Prometheus와 같은 메트릭 수집 시스템과 통합할 수 있다.

**`pom.xml`**에 Actuator 의존성 추가:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

**`application.properties`**에서 Actuator 설정:
```properties
management.endpoints.web.exposure.include=health,info,metrics
management.endpoint.metrics.enabled=true
management.metrics.export.prometheus.enabled=true
```

##### 메트릭 수집 예시

Spring Boot Actuator는 기본적으로 CPU, 메모리, 스레드 사용량 등의 **시스템 메트릭**을 제공한다. 또한, **커스텀 메트릭**도 정의할 수 있다.

```java
import io.micrometer.core.instrument.MeterRegistry;
import org.springframework.stereotype.Service;

@Service
public class MetricService {

    private final MeterRegistry meterRegistry;

    public MetricService(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
        meterRegistry.counter("custom.metric.count").increment();
    }

    public void processTask() {
        meterRegistry.timer("custom.metric.timer").record(() -> {
            // 작업 처리 코드
        });
    }
}
```

##### 외부 모니터링 시스템과의 통합

- **Prometheus**: Spring Boot Actuator는 Prometheus와 쉽게 통합되어 메트릭 데이터를 수집할 수 있다. Prometheus는 수집된 메트릭을 기반으로 경고를 설정하고, **Grafana**와 통합하여 시각화할 수 있다.
- **Datadog, New Relic**: Spring Boot는 Datadog, New Relic 같은 클라우드 기반 모니터링 플랫폼과도 통합 가능하며, 애플리케이션 메트릭과 인프라 메트릭을 동시에 모니터링할 수 있다.

---

#### 3. **Tracing (분산 추적)**

**Tracing**은 분산 시스템에서 요청이 여러 서비스에 걸쳐 어떻게 전달되는지 추적하고, 각 서비스 간의 호출 시간을 측정하는 기능이다. 이를 통해 **성능 병목**을 찾아내고, **장애 원인**을 신속하게 파악할 수 있다.

##### Spring Boot에서 분산 추적 도구

Spring Boot는 **Spring Cloud Sleuth**를 통해 **분산 추적**을 지원하며, **Zipkin**이나 **Jaeger**와 같은 분산 추적 시스템과 쉽게 통합할 수 있다.

**`pom.xml`**에 Sleuth와 Zipkin 의존성 추가:
```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zipkin</artifactId>
</dependency>
```

**`application.properties`**에서 Zipkin 설정:
```properties
spring.sleuth.sampler.probability=1.0  # 모든 요청 추적
spring.zipkin.base-url=http://localhost:9411  # Zipkin 서버 URL
```

##### Sleuth를 통한 자동 추적

Spring Cloud Sleuth는 모든 HTTP 요청에 대해 **Trace ID**와 **Span ID**를 자동으로 생성하여 요청을 추적한다. 여러 서비스 간의 호출이 발생하면, 동일한 Trace ID를 통해 요청의 흐름을 추적할 수 있다.

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

@RestController
public class TracingController {

    private final RestTemplate restTemplate;

    public TracingController(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    @GetMapping("/trace")
    public String trace() {
        return restTemplate.getForObject("http://external-service/trace", String.class);
    }
}
```

##### Zipkin, Jaeger와의 통합

- **Zipkin**: Zipkin은 Spring Cloud Sleuth와 통합하여 요청 흐름을 시각화할 수 있다. 애플리케이션 간의 **호출 관계**와 **응답 시간**을 한눈에 파악할 수 있으며, 병목 지점을 빠르게 찾을 수 있다.
- **Jaeger**: Jaeger는 또 다른 분산 추적 시스템으로, **트랜잭션 모니터링**과 **성능 분석**을 위한 다양한 기능을 제공한다.

---

### Spring Boot 애플리케이션에서 ELK Stack을 사용한 Logging 구현 및 검증 예제

ELK Stack(Elasticsearch, Logstash, Kibana)은 실시간으로 로그 데이터를 수집, 저장, 분석 및 시각화할 수 있는 강력한 도구 세트입니다. **Spring Boot** 애플리케이션의 로그를 ELK Stack으로 통합하여 로그를 실시간으로 분석하고 시각화하는 방법을 단계별로 설명하겠습니다.

---

### 1. **ELK Stack 개요**

- **Elasticsearch**: 로그 데이터를 저장하고 검색하는 분산형 검색 엔진.
- **Logstash**: 다양한 소스에서 데이터를 수집하고, Elasticsearch에 전달하기 전에 데이터를 변환하는 역할.
- **Kibana**: Elasticsearch에서 수집된 데이터를 시각화하고 분석할 수 있는 대시보드 도구.

### 2. **환경 설정**

#### a. **Docker Compose를 사용한 ELK Stack 설치**

Docker를 이용하면 ELK Stack을 쉽게 구성할 수 있습니다. 아래는 Docker Compose 파일을 사용해 Elasticsearch, Logstash, Kibana를 한 번에 구성하는 예입니다.

**`docker-compose.yml`**:
```yaml
version: '3'
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.10.0
    environment:
      - discovery.type=single-node
    ports:
      - "9200:9200"
    networks:
      - elk

  logstash:
    image: docker.elastic.co/logstash/logstash:7.10.0
    ports:
      - "5044:5044"
    networks:
      - elk
    volumes:
      - ./logstash/logstash.conf:/usr/share/logstash/pipeline/logstash.conf

  kibana:
    image: docker.elastic.co/kibana/kibana:7.10.0
    ports:
      - "5601:5601"
    networks:
      - elk

networks:
  elk:
    driver: bridge
```

#### b. **Logstash 구성 파일 설정**

Logstash는 Spring Boot 애플리케이션의 로그를 수집하여 Elasticsearch에 전달하는 역할을 합니다. 아래는 Logstash 설정 파일 예시입니다.

**`logstash.conf`**:
```bash
input {
  beats {
    port => "5044"
  }
}

filter {
  if [log][level] == "ERROR" {
    mutate { add_field => { "severity" => "high" } }
  }
}

output {
  elasticsearch {
    hosts => ["http://elasticsearch:9200"]
    index => "spring-boot-logs-%{+YYYY.MM.dd}"
  }
  stdout { codec => rubydebug }
}
```

위 설정에서 `input`은 Filebeat가 보낼 로그 데이터를 수집하고, `output`은 이를 Elasticsearch로 전송합니다.

---

### 3. **Spring Boot 애플리케이션 설정**

Spring Boot 애플리케이션에서 ELK Stack으로 로그를 전송하려면, Filebeat와 Logstash를 통해 로그를 수집해야 합니다.

#### a. **Logback 설정을 통해 Filebeat와 Logstash로 로그 전송**

Spring Boot 애플리케이션은 기본적으로 **Logback**을 사용하여 로그를 관리합니다. Filebeat는 로컬에서 파일을 읽어 Logstash로 로그를 전송하는 역할을 합니다.

**`application.properties`** 파일에서 로그를 파일에 저장하도록 설정합니다:
```properties
# 로그 파일 경로 설정
logging.file.name=/var/log/spring-boot-app/app.log
logging.level.root=INFO
```

#### b. **Logback 설정을 사용해 Logstash로 직접 로그 전송**

Logback을 직접 설정하여 Logstash로 로그를 전송할 수도 있습니다. 이를 위해 Logback의 **SocketAppender**를 사용하여 로그를 네트워크로 전송할 수 있습니다.

**`logback-spring.xml`**:
```xml
<configuration>

    <appender name="LOGSTASH" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
        <destination>localhost:5044</destination>
        <encoder class="net.logstash.logback.encoder.LogstashEncoder" />
    </appender>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>/var/log/spring-boot-app/app.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>/var/log/spring-boot-app/app-%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>30</maxHistory>
        </rollingPolicy>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="LOGSTASH" />
        <appender-ref ref="FILE" />
    </root>
</configuration>
```

- **LogstashTcpSocketAppender**: Spring Boot의 로그를 Logstash 서버로 전송합니다.
- **FileAppender**: 로그를 파일로 저장하여 Filebeat가 읽을 수 있게 합니다.

#### c. **Filebeat 설정**

**Filebeat**는 로그 파일을 읽어 Logstash로 전송하는 역할을 합니다. 

Filebeat 설정 파일(**filebeat.yml**)을 구성합니다:

```yaml
filebeat.inputs:
  - type: log
    enabled: true
    paths:
      - /var/log/spring-boot-app/app.log

output.logstash:
  hosts: ["localhost:5044"]
```

이 설정은 `/var/log/spring-boot-app/app.log`에서 로그를 읽고, Logstash에 전달하는 역할을 합니다.

Filebeat는 로컬 로그 파일을 지속적으로 모니터링하며, 새로운 로그가 생성될 때마다 이를 Logstash로 전송합니다.

---

### 4. **로그 데이터 검증 및 Kibana를 통한 시각화**

#### a. **Elasticsearch에 로그 데이터 수집 확인**

Kibana에서 Elasticsearch 인덱스를 확인하여 로그 데이터가 성공적으로 수집되었는지 확인할 수 있습니다.

1. **Kibana 접속**: `http://localhost:5601`에 접속.
2. **Index Patterns 생성**: Kibana에서 `spring-boot-logs-*` 인덱스 패턴을 생성하여 Elasticsearch에 저장된 로그 데이터를 확인할 수 있습니다.

#### b. **Kibana 대시보드를 통한 로그 시각화**

- **Discover 탭**에서 실시간으로 로그 데이터를 검색하고, 다양한 필터를 적용하여 로그를 분석할 수 있습니다.
- **Visualize 탭**을 통해 로그 데이터를 기반으로 차트나 그래프를 생성하여 시각적으로 표현할 수 있습니다.

---

### 5. **로그 검증 예시**

애플리케이션에서 로그를 생성하고, Kibana를 통해 해당 로그가 제대로 수집되었는지 확인합니다.

**Spring Boot 애플리케이션에서 로그 생성**:
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class LoggingController {

    private static final Logger logger = LoggerFactory.getLogger(LoggingController.class);

    @GetMapping("/log")
    public String logTest() {
        logger.info("This is an INFO log message");
        logger.error("This is an ERROR log message");
        return "Logs generated!";
    }
}
```

이제 `/log` 엔드포인트를 호출하면, Logback을 통해 생성된 로그가 Logstash를 거쳐 Elasticsearch에 저장되고, Kibana에서 이를 확인할 수 있습니다.

---
