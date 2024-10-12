Gradle 멀티 프로젝트에서 IntelliJ IDEA를 사용하여 Tomcat에 artifact를 배포할 때, 하위 프로젝트까지 다시 빌드되면서 시간이 오래 걸리는 문제는 Gradle의 증분 빌드 및 캐싱과 관련된 설정을 최적화하여 해결할 수 있습니다. 아래의 방법을 통해 불필요한 재빌드를 방지하고, Tomcat에 더 빠르게 artifact를 배포할 수 있는 방안을 설명하겠습니다.

1. 증분 빌드(Incremental Build) 활성화

Gradle은 기본적으로 증분 빌드를 지원하지만, 특정 설정이 잘못되면 모든 하위 프로젝트를 매번 다시 빌드하게 됩니다. 이를 방지하려면 Gradle의 증분 빌드 기능이 제대로 동작하는지 확인해야 합니다.

	•	--incremental 플래그를 명시적으로 사용하거나, IntelliJ가 이를 자동으로 처리할 수 있도록 설정되어 있는지 확인하세요.
	•	기본적으로 Gradle은 이전에 빌드된 아티팩트를 캐시하고, 변경된 부분만 빌드하지만, IntelliJ에서 Gradle 캐시가 제대로 관리되고 있는지 확인이 필요합니다.

2. 하위 프로젝트 의존성 관리

하위 프로젝트들이 서로 간에 의존 관계가 있을 경우, 특정 프로젝트의 변경 사항이 다른 하위 프로젝트들의 빌드에 영향을 미칠 수 있습니다. 따라서 불필요한 의존성을 줄여 하위 프로젝트의 불필요한 빌드를 방지해야 합니다.

	•	settings.gradle에서 각 하위 프로젝트 간의 의존성을 다시 확인하세요. 필요하지 않은 의존성을 제거하여, 하나의 프로젝트를 빌드할 때 다른 프로젝트가 다시 빌드되지 않도록 관리할 수 있습니다.

3. Gradle의 빌드 캐시(Build Cache) 사용

Gradle의 빌드 캐시 기능을 활용하면 동일한 태스크를 여러 번 수행할 필요 없이 캐싱된 결과를 재사용할 수 있습니다. 이 기능을 사용하면 반복되는 빌드를 방지할 수 있습니다.

빌드 캐시 활성화 방법:

gradle.properties 파일에 다음 내용을 추가하여 빌드 캐시를 활성화하세요.

org.gradle.caching=true

4. 컴파일된 클래스 파일 사용

IntelliJ에서 Gradle을 사용하는 대신 IntelliJ 자체의 빌드 시스템을 사용할 수도 있습니다. IntelliJ는 Gradle 대신 자신의 빌드 시스템을 사용하여 컴파일된 클래스 파일을 재활용할 수 있기 때문에 빌드 시간을 줄이는 데 도움이 될 수 있습니다.

IntelliJ의 빌드 시스템 사용:

	1.	Preferences -> Build, Execution, Deployment -> Build Tools -> Gradle로 이동합니다.
	2.	Build and run using 옵션을 IntelliJ로 변경합니다.
	3.	Run tests using도 IntelliJ로 설정합니다.

이렇게 하면 IntelliJ의 빌드 시스템을 사용하여 더 빠르게 컴파일하고 배포할 수 있습니다.

5. Tomcat 배포 설정 조정

Tomcat에 artifact를 배포할 때, Gradle을 통해 직접 빌드 및 배포하는 대신, IntelliJ에서 핫스왑(HotSwap) 기능을 활용하면 코드를 수정한 후 서버를 재시작하지 않고도 빠르게 변경된 내용을 반영할 수 있습니다.

Tomcat HotSwap 설정 방법:

	1.	Run/Debug Configurations에서 Tomcat 서버 설정을 열고 On Update Action과 On Frame Deactivation 옵션을 HotSwap classes로 설정합니다.
	2.	코드 수정 후 바로 배포될 수 있도록 하여 전체 빌드를 피할 수 있습니다.

6. Gradle Daemon 사용

Gradle Daemon을 활성화하면 매번 Gradle을 시작할 때 초기화되는 시간을 절약할 수 있습니다. Daemon을 사용하면 빌드가 종료된 후에도 백그라운드에서 Gradle 프로세스가 유지되며, 다음 빌드 시 속도가 빨라집니다.

Gradle Daemon 활성화:

gradle.properties 파일에 다음을 추가하세요.

org.gradle.daemon=true

7. 하위 프로젝트의 빌드 제외

어떤 하위 프로젝트들은 배포할 때 굳이 빌드할 필요가 없을 수도 있습니다. 특정 프로젝트들만 선택적으로 빌드하여 불필요한 빌드를 방지할 수 있습니다.

	•	Tomcat에 배포할 때는 최종적으로 웹 애플리케이션 아티팩트를 생성하는 프로젝트만 빌드하는 것이 이상적입니다.
	•	Gradle에서 특정 프로젝트만 빌드하려면, IntelliJ에서 하위 프로젝트 빌드 구성을 명시적으로 관리할 수 있습니다. 예를 들어, 루트 프로젝트가 아닌 특정 모듈만 빌드되도록 조정할 수 있습니다.

8. 병렬 빌드 활성화

Gradle은 병렬로 태스크를 실행할 수 있는 기능을 제공합니다. 병렬 빌드를 활성화하면 멀티코어 CPU를 활용해 빌드 시간을 크게 단축할 수 있습니다.

병렬 빌드 설정:

gradle.properties 파일에 다음 옵션을 추가하세요.

org.gradle.parallel=true

이 옵션을 활성화하면, Gradle은 여러 프로젝트를 병렬로 빌드하여 빌드 시간을 단축합니다.

9. IDE 캐시 정리 및 성능 최적화

마지막으로, IntelliJ의 캐시를 정리하거나 IDE 자체에서 빌드와 관련된 성능 최적화를 진행할 수 있습니다.

캐시 정리:

	•	File -> Invalidate Caches / Restart를 선택하여 IntelliJ의 캐시를 정리하고 IDE의 성능을 최적화합니다.

결론

하위 프로젝트들이 매번 빌드되는 문제는 Gradle 설정 및 IntelliJ 설정의 조합으로 해결할 수 있습니다. 증분 빌드, Gradle Daemon, IntelliJ 자체 빌드 시스템 사용, 불필요한 의존성 제거 등의 방법을 통해 빌드 시간을 최적화할 수 있습니다.