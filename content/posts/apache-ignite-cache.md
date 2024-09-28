### Apache Ignite Cache 개념

**Apache Ignite**는 **인메모리 데이터 그리드(In-Memory Data Grid, IMDG)** 솔루션으로, **데이터 캐싱**, **데이터 분산 저장**, **컴퓨팅 기능**, **트랜잭션 처리** 등을 제공하는 **분산형 플랫폼**입니다. **Apache Ignite Cache**는 데이터를 **메모리**에 저장하여 매우 빠른 읽기 및 쓰기 성능을 제공하며, 필요에 따라 **디스크 기반 저장소**와 연계하여 **영구성**도 보장할 수 있습니다.

Ignite Cache는 **분산 캐시** 또는 **로컬 캐시**로 구성할 수 있으며, 각 노드에 데이터를 분산하여 저장하는 방식으로 **수평적 확장성**을 제공합니다. 이를 통해 **대규모 데이터**를 처리하면서도 **높은 성능**을 유지할 수 있습니다.

#### Apache Ignite Cache의 주요 기능
1. **인메모리 데이터 저장**: 데이터는 메모리에 저장되므로 **저지연(Low-latency)** 데이터 접근이 가능합니다.
2. **분산 캐시**: 데이터를 여러 노드에 **분산**하여 저장하므로, **확장성**과 **고가용성**을 보장합니다.
3. **트랜잭션 지원**: Ignite Cache는 **ACID 트랜잭션**을 지원하여 분산 환경에서도 데이터의 일관성을 유지합니다.
4. **복제 및 파티셔닝**: 데이터를 **파티셔닝(partitioning)**하여 노드 간에 분산하거나, 데이터를 **복제(replication)**하여 고가용성을 보장합니다.
5. **영구성(Optional Durability)**: 메모리 외에도 디스크 기반 저장을 지원하여 데이터를 영구적으로 보존할 수 있습니다.
6. **SQL 및 NoSQL 지원**: Ignite는 **SQL**과 **NoSQL** 질의를 모두 지원하여 유연한 데이터 접근이 가능합니다.

---

### Apache Ignite Cache의 주요 설정 포인트

**Apache Ignite**는 다양한 기능을 지원하는 만큼, 캐시 설정에서 고려해야 할 중요한 포인트들이 있습니다. 주요 설정 포인트는 **캐시 모드**, **데이터 파티셔닝**, **영구성 설정**, **트랜잭션 설정** 등입니다.

#### 1. **캐시 모드(Cache Mode)**

캐시 모드는 데이터가 **어떻게 저장되고 관리**되는지를 결정하는 설정입니다. Apache Ignite는 기본적으로 3가지 캐시 모드를 제공합니다.

- **PARTITIONED** (기본값): 데이터를 **파티션**으로 나누어 클러스터의 각 노드에 분산하여 저장합니다. 각 노드는 전체 데이터의 일부만 저장하며, 확장성과 성능이 뛰어납니다.
  
- **REPLICATED**: 모든 노드가 **전체 데이터**를 저장하는 방식으로, **읽기 성능**이 중요할 때 유리하지만, 데이터 업데이트 시 모든 노드에 데이터 변경이 전파되므로 **쓰기 성능**은 떨어질 수 있습니다.

- **LOCAL**: 데이터가 로컬 노드에만 저장되며, 다른 노드로 데이터가 전파되지 않습니다. 작은 규모의 독립된 작업에 적합합니다.

**설정 예시**:
```java
CacheConfiguration<String, String> cacheConfig = new CacheConfiguration<>("myCache");
cacheConfig.setCacheMode(CacheMode.PARTITIONED);  // 기본적으로 파티셔닝된 모드
```

#### 2. **데이터 복제 및 백업 설정 (Backups)**

**백업(Backups)**은 데이터의 **고가용성**을 보장하기 위한 설정입니다. 데이터를 복제하여 다른 노드에 저장함으로써, 한 노드가 실패하더라도 다른 노드에서 데이터를 복구할 수 있습니다.

- **Backups**: 데이터를 몇 개의 노드에 복제할지 설정합니다. 기본값은 `0`으로, 복제가 없고, 노드 실패 시 데이터가 유실될 수 있습니다.

**설정 예시**:
```java
cacheConfig.setBackups(1);  // 데이터를 한 개의 노드에 복제
```

#### 3. **데이터 파티셔닝 (Data Partitioning)**

**PARTITIONED 모드**에서는 데이터를 **파티션(partition)**으로 나누어 클러스터의 여러 노드에 저장합니다. 파티셔닝은 데이터를 **분산**하여 처리 성능을 높이는 핵심 요소입니다.

- **Affinity Function**: 데이터가 특정 노드에 어떻게 분산될지를 결정하는 함수입니다. 기본적으로 **Rendezvous Affinity Function**을 사용하지만, 커스텀 함수를 설정할 수 있습니다.
  
**설정 예시**:
```java
cacheConfig.setAffinity(new RendezvousAffinityFunction());
```

#### 4. **영구성 설정 (Durability)**

기본적으로 Apache Ignite는 데이터를 메모리에 저장하지만, **지속성 설정**을 통해 데이터를 **디스크**에 저장하여 **데이터 손실**을 방지할 수 있습니다.

- **Native Persistence**: Ignite는 데이터를 디스크에 저장하여 노드가 재시작되더라도 데이터를 복구할 수 있도록 지원합니다. 데이터는 메모리와 디스크에 모두 저장되며, 필요할 때 메모리에서 삭제됩니다.
  
- **WAL (Write-Ahead Logging)**: 데이터를 디스크에 기록하기 전에 로그에 먼저 기록하여 장애 시 데이터 복구를 지원합니다.

**설정 예시**:
```java
DataStorageConfiguration storageCfg = new DataStorageConfiguration();
storageCfg.getDefaultDataRegionConfiguration().setPersistenceEnabled(true);  // 영구성 설정

ignite.configuration().setDataStorageConfiguration(storageCfg);
ignite.cluster().active(true);  // 영구성 모드 활성화
```

#### 5. **트랜잭션 설정 (Transactions)**

Apache Ignite는 분산 환경에서 **ACID 트랜잭션**을 지원합니다. 트랜잭션 설정에 따라 트랜잭션 격리 수준과 커밋 전략을 선택할 수 있습니다.

- **트랜잭션 격리 수준**: 트랜잭션 중 다른 트랜잭션과의 상호작용을 제어하는 수준으로, **READ_COMMITTED**, **REPEATABLE_READ**, **SERIALIZABLE**을 지원합니다.
  
- **트랜잭션 동작 모드**: **OPTIMISTIC** 또는 **PESSIMISTIC** 트랜잭션 모드를 설정할 수 있습니다. `OPTIMISTIC`은 충돌이 발생할 가능성을 줄이고, `PESSIMISTIC`은 더 안전하지만 성능에 영향을 미칠 수 있습니다.

**설정 예시**:
```java
TransactionConfiguration txCfg = new TransactionConfiguration();
txCfg.setDefaultTxConcurrency(TransactionConcurrency.PESSIMISTIC);  // 비관적 트랜잭션
txCfg.setDefaultTxIsolation(TransactionIsolation.REPEATABLE_READ);  // 반복 읽기 격리 수준
ignite.configuration().setTransactionConfiguration(txCfg);
```

#### 6. **SQL 및 NoSQL 질의 지원**

Apache Ignite는 **SQL** 및 **NoSQL** 기반의 질의를 지원하여 데이터를 검색하고 조작할 수 있습니다. **SQL 인덱스**를 통해 효율적인 검색 성능을 제공하며, 복잡한 질의를 최적화할 수 있습니다.

**설정 예시**:
```java
CacheConfiguration<Long, Person> cacheCfg = new CacheConfiguration<>("personCache");

// SQL 인덱스 설정
cacheCfg.setIndexedTypes(Long.class, Person.class);
ignite.getOrCreateCache(cacheCfg);

// SQL 질의 실행
List<List<?>> result = ignite.cache("personCache").query(
    new SqlFieldsQuery("SELECT * FROM Person WHERE age > 30")).getAll();
```

---

### 7. **이벤트 처리 및 리스너 (Event Handling)**

Ignite는 데이터 변경, 트랜잭션 완료 등의 이벤트를 **리스너**로 처리할 수 있습니다. 특정 이벤트에 대한 트리거를 설정하여 동작을 제어할 수 있습니다.

**설정 예시**:
```java
ignite.events().localListen((event) -> {
    System.out.println("이벤트 발생: " + event.name());
    return true;
}, EventType.EVT_CACHE_OBJECT_PUT, EventType.EVT_CACHE_OBJECT_REMOVED);
```

---

### 8. **노드 간 통신 (Cluster Communication)**

Ignite는 클러스터 노드 간의 데이터 전송 및 통신을 지원하며, 다양한 통신 설정을 제공합니다.

- **Discovery SPI**: 클러스터에 노드가 어떻게 연결되고 탐색되는지를 제어하는 인터페이스로, 여러 프로토콜을 지원합니다. 예를 들어, **TCP Discovery**를 통해 클러스터 노드 간의 연결을 설정할 수 있습니다.

**설정 예시**:
```java
TcpDiscoverySpi spi = new TcpDiscoverySpi();
TcpDiscoveryMulticastIpFinder ipFinder = new TcpDiscoveryMulticastIpFinder();
ipFinder.setAddresses(Collections.singletonList("127.0.0.1:47500..47509"));
spi.setIpFinder(ipFinder);

ignite.configuration().setDiscoverySpi(spi);
```

---

### Apache Ignite Cache에서 Container Orchestration 환경에서의 캐시 노드 간 통신

**Apache Ignite**는 **분산형 인메모리 데이터 그리드** 솔루션으로, 여러 노드가 함께 데이터를 저장하고 처리할 수 있도록 분산 환경에서 동작합니다. 특히 **Kubernetes**나 **Docker Swarm** 같은 **Container Orchestration** 환경에서는 **노드 간의 통신**을 통해 데이터를 효율적으로 교환하고, 클러스터 전체에서 **고가용성**과 **확장성**을 유지해야 합니다.

### 1. **Container Orchestration 환경에서의 Apache Ignite 클러스터 통신**

**컨테이너 기반 환경**에서는 각 컨테이너가 개별 노드로 동작하므로, 컨테이너 사이에서 **통신**이 이루어져야 합니다. Apache Ignite는 **노드 간 통신**을 위한 **Discovery SPI**를 통해 노드 탐색 및 통신을 설정합니다. 이 과정에서 중요한 점은 **동적인 네트워크 환경**에서도 노드가 클러스터에 **자동으로 연결**되고 **데이터를 교환**할 수 있도록 하는 것입니다.

#### 주요 고려 사항:
1. **동적 IP 주소**: 컨테이너 환경에서는 노드가 **동적으로 할당된 IP**를 가지므로, 고정 IP를 사용할 수 없습니다.
2. **서비스 디스커버리**: 컨테이너 기반 환경에서는 **서비스 디스커버리**가 필수적입니다. 이는 각 노드가 동적으로 할당된 IP로도 서로를 탐지하고, 클러스터에 합류할 수 있도록 해줍니다.
3. **파드(Pod)의 생성/종료**: **Kubernetes** 같은 환경에서는 노드(파드)가 동적으로 **생성되고 종료**될 수 있습니다. 따라서 Ignite 노드 간 통신이 이러한 **유동적인 환경**에서도 안정적으로 이루어져야 합니다.

---

### 2. **Ignite에서 Kubernetes 환경의 설정**

**Kubernetes**와 같은 컨테이너 오케스트레이션 환경에서 Apache Ignite는 기본적으로 **TCP 기반의 통신**을 사용하여 노드 간의 데이터를 교환합니다. 이때, **TCP Discovery SPI**와 **Kubernetes IP Finder**를 사용하여 **클러스터 내 노드들이 서로 통신**할 수 있도록 설정할 수 있습니다.

#### a. **TcpDiscoveryKubernetesIpFinder 설정**

**TcpDiscoveryKubernetesIpFinder**는 Ignite 클러스터가 **Kubernetes** 환경에서 실행될 때, 노드 간 통신을 자동으로 구성하기 위한 IP 탐색 도구입니다. 이는 **Kubernetes API**를 이용해 Ignite 노드가 실행 중인 파드를 탐지하고, 그들의 IP 주소를 사용해 노드 간 통신을 설정합니다.

**주요 설정 항목**:
1. **Kubernetes Namespace**: Ignite 노드가 속한 **네임스페이스**를 지정하여, 해당 네임스페이스 내의 Ignite 인스턴스들끼리만 클러스터를 형성하게 할 수 있습니다.
2. **Service Name**: Ignite 노드 간 통신을 위한 **서비스 이름**을 지정하여, 해당 서비스에 연결된 파드들끼리만 통신하게 설정할 수 있습니다.
3. **Master URL**: Kubernetes API 서버의 URL을 지정합니다.
4. **Account Token**: Kubernetes API에 접근할 수 있는 **토큰**을 설정합니다.

---

### 3. **Ignite와 Kubernetes 통합을 위한 설정 예시**

아래는 **Kubernetes** 환경에서 **Ignite 노드 간 통신**을 설정하는 예시입니다.

#### Ignite Configuration (Java 설정 코드)

```java
import org.apache.ignite.configuration.IgniteConfiguration;
import org.apache.ignite.spi.discovery.tcp.TcpDiscoverySpi;
import org.apache.ignite.spi.discovery.tcp.ipfinder.kubernetes.TcpDiscoveryKubernetesIpFinder;

public class IgniteKubernetesConfig {
    public static IgniteConfiguration createIgniteConfiguration() {
        // Ignite Configuration 생성
        IgniteConfiguration cfg = new IgniteConfiguration();

        // Discovery SPI 설정 (Kubernetes 환경에서 노드 간 통신 설정)
        TcpDiscoverySpi spi = new TcpDiscoverySpi();
        
        // Kubernetes IP Finder 사용하여 노드 탐색
        TcpDiscoveryKubernetesIpFinder ipFinder = new TcpDiscoveryKubernetesIpFinder();

        // Kubernetes 네임스페이스 설정
        ipFinder.setNamespace("ignite-namespace");

        // Kubernetes 서비스 이름 설정
        ipFinder.setServiceName("ignite-service");

        // Kubernetes API URL 설정
        ipFinder.setMasterUrl("https://kubernetes.default.svc.cluster.local");

        // Kubernetes API 접근을 위한 토큰 설정
        ipFinder.setAccountTokenPath("/var/run/secrets/kubernetes.io/serviceaccount/token");

        // IP Finder를 Discovery SPI에 연결
        spi.setIpFinder(ipFinder);

        // Ignite Configuration에 Discovery SPI 설정
        cfg.setDiscoverySpi(spi);

        return cfg;
    }
}
```

#### 주요 설정 요소 설명

1. **Namespace 설정**: `ipFinder.setNamespace("ignite-namespace")`
   - Kubernetes 네임스페이스를 설정하여, Ignite 인스턴스들이 특정 네임스페이스 안에서만 노드를 탐색하고 클러스터링되도록 합니다. 이는 클러스터 간 충돌을 방지하고, 네트워크 범위를 제한할 수 있습니다.

2. **Service Name 설정**: `ipFinder.setServiceName("ignite-service")`
   - Ignite 클러스터는 **Kubernetes 서비스**를 통해 서로 통신하게 됩니다. 여기서 서비스 이름을 설정하여 특정 서비스에 연결된 노드들끼리만 통신을 하도록 설정합니다.

3. **Master URL 설정**: `ipFinder.setMasterUrl("https://kubernetes.default.svc.cluster.local")`
   - Kubernetes API 서버에 접근할 수 있는 URL을 설정합니다. 기본적으로 `https://kubernetes.default.svc.cluster.local`이 Kubernetes API 서버의 기본 주소입니다.

4. **Account Token 설정**: `ipFinder.setAccountTokenPath("/var/run/secrets/kubernetes.io/serviceaccount/token")`
   - Kubernetes API에 접근하기 위한 **서비스 어카운트 토큰**을 설정합니다. 이는 Ignite 노드가 Kubernetes API를 통해 파드 정보를 얻어오기 위해 필요합니다.

---

### 4. **Kubernetes 환경에서의 클러스터 형성 원리**

**TcpDiscoveryKubernetesIpFinder**는 **Kubernetes API**를 사용하여 해당 네임스페이스에서 실행 중인 모든 Ignite 노드(파드)를 탐색합니다. Kubernetes 서비스와 통신하여 각 노드의 **IP 주소**와 **포트**를 확인하고, 이를 사용해 Ignite 노드 간의 **P2P 통신**을 설정합니다.

- **노드 탐색**: Ignite 노드가 시작되면, Kubernetes API를 통해 동일한 서비스에 속한 Ignite 파드를 탐지합니다.
- **통신 설정**: 탐색된 Ignite 파드들의 IP 주소를 기반으로 노드 간 **TCP 통신**이 설정됩니다.
- **동적 확장**: Kubernetes 파드가 동적으로 생성되거나 삭제될 때, **TcpDiscoveryKubernetesIpFinder**는 이를 감지하여 새로운 노드를 클러스터에 추가하거나 제거된 노드를 처리합니다.

---

### 5. **중요한 설정 포인트와 고려 사항**

1. **Service Type과 Port 설정**
   - Kubernetes에서 Ignite 클러스터는 기본적으로 **Headless Service**를 사용하여 각 Ignite 노드(파드)가 서로를 **직접 탐색**할 수 있도록 해야 합니다.
   - 각 파드가 **고유한 IP 주소**를 가지므로, Ignite 노드는 이를 이용해 서로 통신합니다.

   **Headless Service 예시** (`ignite-service.yaml`):
   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: ignite-service
     namespace: ignite-namespace
   spec:
     clusterIP: None  # Headless Service
     selector:
       app: ignite
     ports:
       - protocol: TCP
         port: 47500
         targetPort: 47500
   ```

2. **포트 설정**: Ignite는 기본적으로 **TCP 포트 47500**을 사용하여 노드 간 통신을 합니다. 각 Ignite 노드가 이 포트로 통신할 수 있도록, **Kubernetes 서비스** 및 **파드**에 적절히 포트를 설정해야 합니다.

3. **동적 스케일링 지원**: Kubernetes 환경에서는 Ignite 파드가 동적으로 생성되고 제거될 수 있으므로, 클러스터가 이를 잘 처리할 수 있도록 **IpFinder**를 적절히 설정해야 합니다. `TcpDiscoveryKubernetesIpFinder`는 이러한 동적 환경을 위해 설계되어 있으며, 클러스터에 새로운 노드를 추가하거나 기존 노드를 제거할 때 자동으로 노드 정보를 갱신합니다.

---

### 6. **장점**

1. **자동화된 노드 디스커버리**: Kubernetes와 Ignite의 통합을 통해 노드 간 **자동 디스커버리**가 가능하므로, 클러스터 확장 및 축소가 간편해집니다.
2. **유연한 확장성**: Kubernetes의 **스케일링 기능**을 사용해 Ignite 클러스터를 자동으로 확장하거나 축소할 수 있으며, `TcpDiscoveryKubernetesIpFinder`를 통해 이를 효율적으로 처리할 수 있습니다.
