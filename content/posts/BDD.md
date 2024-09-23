---
author: "seonuk1218"
title: "BDD Test"
image: ""
draft: false
date: 2024-05-07
description: ""
tags: ["testing", "bdd"]
archives: ["2024/05"]
---


BDD 와 CI script, build Script


BDD(Behavior-Driven Development, 행위 주도 개발)는 소프트웨어 개발 방법론 중 하나로, 시스템이 어떻게 행동해야 하는지(behavior)에 초점을 맞추어 테스트를 작성하고 개발하는 방식입니다. BDD는 주로 **비즈니스 요구사항**을 **테스트 가능한 명세**로 변환하는 데 중점을 둡니다. 이를 통해 개발자, 테스트 담당자, 비즈니스 담당자들이 동일한 언어로 시스템의 동작을 이해하고 소통할 수 있게 해줍니다.

### BDD의 특징:
1. **공통된 언어 사용**: BDD는 비즈니스 요구사항을 기술하는 방식과 테스트 코드를 최대한 일치시키려고 합니다. 주로 Gherkin과 같은 자연어 기반의 문법을 사용해 테스트를 작성합니다. 이로 인해 비기술자들도 요구사항과 테스트 시나리오를 쉽게 이해할 수 있습니다.
   
2. **명확한 시나리오 기반 테스트**: 각 기능의 **행위**를 중심으로, 주로 **Given-When-Then** 형식으로 시나리오를 작성합니다.
   - **Given**: 어떤 상황이 주어졌을 때 (사전 조건)
   - **When**: 사용자가 특정 행동을 취하면 (행동)
   - **Then**: 그 결과로 어떤 일이 발생해야 한다 (결과)

3. **요구사항 중심의 테스트**: BDD에서는 테스트를 "시스템이 주어진 상황에서 어떻게 동작해야 하는가"라는 관점에서 작성합니다. 즉, 비즈니스 요구사항이 실제로 충족되는지를 테스트하는 것이므로, 각 요구사항마다 테스트를 작성하는 것이 맞습니다.

### 예시 (Gherkin 언어 사용):
```gherkin
Feature: User login
  As a registered user
  I want to log in to my account
  So that I can access my personalized dashboard

  Scenario: Successful login with correct credentials
    Given the user is on the login page
    When the user enters valid credentials
    Then the user is redirected to their dashboard

  Scenario: Login with incorrect credentials
    Given the user is on the login page
    When the user enters incorrect credentials
    Then an error message is displayed
```

### BDD의 장점:
- **명확한 요구사항 반영**: 비즈니스 요구사항이 구체적으로 테스트로 전환되므로, 요구사항에 빠짐없이 부합하는지 확인할 수 있습니다.
- **협업 강화**: 개발자, 테스터, 비즈니스 담당자 간의 의사소통을 개선해, 모두가 동일한 목표와 이해를 공유할 수 있습니다.
- **테스트 자동화와 연계**: BDD 시나리오를 기반으로 테스트 자동화 도구와 연동해, 지속적으로 기능 테스트를 실행할 수 있습니다.

따라서 BDD는 각 요구사항에 따른 명확한 테스트 케이스를 제공하며, 테스트를 통해 시스템이 요구사항을 충족하는지 확인하는 접근법이라고 할 수 있습니다.

---


테스트와 CI/CD 파이프라인의 통합은 **CI/CD 파이프라인 스크립트(CI Script)**와 **빌드 도구(Gradle 등의 빌드 스크립트)** 모두에서 정의됩니다. 두 가지는 상호 보완적인 역할을 하며, 각자의 책임이 있습니다.

### 1. **CI Script에서의 정의**
CI/CD 파이프라인 스크립트는 Jenkins, GitLab CI, CircleCI, Travis CI 등에서 사용되는 **파이프라인 관리 도구**로, 자동화된 빌드, 테스트, 배포 프로세스를 정의합니다. 이 스크립트는 프로젝트를 빌드하고 테스트하는 전체적인 흐름을 관리합니다.

주요 책임은 다음과 같습니다:
- **파이프라인 단계 정의**: 파이프라인의 각 단계를 정의합니다. 예를 들어, 코드를 빌드하는 단계, 유닛 테스트를 실행하는 단계, 통합 테스트나 배포 등을 포함할 수 있습니다.
- **빌드 도구와 통합**: Gradle이나 Maven 등의 빌드 도구를 사용하여 프로젝트를 빌드하고 테스트하는 명령을 실행합니다.
- **테스트 실행**: CI 도구에서 테스트를 실행할 수 있도록 Gradle과 같은 빌드 도구의 테스트 명령을 호출합니다.
- **성공/실패 기준 설정**: 각 단계가 성공했는지 또는 실패했는지에 대한 기준을 설정하고, 성공하면 다음 단계로 넘어가고, 실패 시 파이프라인을 종료합니다.

예시 (Jenkinsfile):
```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Gradle 빌드와 테스트 실행
                sh './gradlew clean build'
            }
        }
        stage('Test') {
            steps {
                // 테스트가 포함된 Gradle 빌드 실행
                sh './gradlew test'
            }
        }
        stage('Deploy') {
            steps {
                // 배포 단계 정의
                sh './gradlew deploy'
            }
        }
    }
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Build or test failed.'
        }
    }
}
```

### 2. **Gradle 등의 빌드 스크립트에서의 정의**
Gradle이나 Maven 같은 빌드 도구는 프로젝트를 빌드하고 테스트하는 데 필요한 **구체적인 빌드, 테스트, 의존성 관리**를 담당합니다. CI/CD 파이프라인에서 Gradle 스크립트를 호출해 빌드를 실행하게 됩니다.

주요 책임은 다음과 같습니다:
- **테스트 실행**: 테스트에 대한 구체적인 설정과 실행을 정의합니다. 예를 들어, 테스트 프레임워크(JUnit, TestNG 등)를 설정하고, 테스트 실행 시 필요한 추가 설정을 정의합니다.
- **빌드 및 의존성 관리**: 프로젝트 빌드 과정을 정의하고, 필요한 의존성을 관리합니다.
- **배포 스크립트 정의**: 빌드된 결과물(애플리케이션, 아티팩트 등)을 특정 서버나 아티팩트 저장소에 배포하는 과정을 정의할 수 있습니다.

예시 (Gradle `build.gradle`):
```groovy
plugins {
    id 'java'
}

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.7.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.7.0'
}

test {
    useJUnitPlatform() // JUnit5 사용
}

task integrationTest(type: Test) {
    useJUnitPlatform()
    include '**/integration/**'  // 통합 테스트용 디렉토리 포함
}

```

### 정리하자면:
- **CI Script (예: Jenkinsfile)**는 전체 파이프라인의 흐름을 정의하며, 빌드, 테스트, 배포의 각 단계를 관리합니다. 이 스크립트는 Gradle 같은 빌드 도구를 호출해 빌드 및 테스트를 실행합니다.
- **Gradle 같은 빌드 스크립트**는 프로젝트의 빌드와 테스트 프로세스 자체를 정의합니다. 예를 들어, 어떤 테스트를 실행하고, 어떤 의존성을 설치할지, 빌드 아티팩트를 어떻게 생성할지를 설정합니다.

따라서 **CI/CD 파이프라인에서 테스트와 빌드를 자동화하는 데에는 두 스크립트가 협력**하여 정의되는 것입니다. CI 스크립트는 전체적인 단계와 흐름을 담당하고, Gradle 같은 빌드 도구는 실제로 빌드 및 테스트 작업을 수행하는 역할을 합니다.


------

Gradle에서 **`test` task**는 기능적으로 쪼개서 다양한 테스트 작업을 정의할 수 있습니다. 예를 들어, 유닛 테스트, 통합 테스트, 성능 테스트 등을 각각 별도로 정의할 수 있으며, 이를 통해 특정 목적에 맞는 테스트를 독립적으로 실행할 수 있습니다.

### 1. **기본 `test` task**
Gradle에서는 기본적으로 `test`라는 task가 존재하며, 보통 **유닛 테스트**를 실행하는 데 사용됩니다. 이 task는 프로젝트 내의 `src/test/java`에 있는 테스트 코드를 실행합니다.

### 2. **커스텀 테스트 task 만들기**
Gradle에서는 `Test` 타입의 task를 정의함으로써, 다양한 테스트 작업을 독립적으로 설정할 수 있습니다. 이를 통해 특정 디렉토리에 위치한 테스트 코드만 실행하거나, 다른 테스트 프레임워크를 사용할 수 있습니다.

### 예시: 유닛 테스트와 통합 테스트를 분리하는 경우

```groovy
plugins {
    id 'java'
}

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.7.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.7.0'
    integrationTestImplementation 'org.junit.jupiter:junit-jupiter-api:5.7.0'
}

test {
    useJUnitPlatform() // JUnit 5로 유닛 테스트 실행
    include '**/unit/**'  // 유닛 테스트 파일 위치를 지정
}

task integrationTest(type: Test) {
    useJUnitPlatform() // JUnit 5로 통합 테스트 실행
    testClassesDirs = sourceSets.integrationTest.output.classesDirs
    classpath = sourceSets.integrationTest.runtimeClasspath
    include '**/integration/**'  // 통합 테스트 파일 위치를 지정
}

sourceSets {
    integrationTest {
        java {
            srcDir 'src/integrationTest/java' // 통합 테스트 디렉토리 설정
        }
        resources {
            srcDir 'src/integrationTest/resources'
        }
    }
}

configurations {
    integrationTestImplementation.extendsFrom testImplementation
    integrationTestRuntimeOnly.extendsFrom testRuntimeOnly
}
```

### 설명:
- **`test` task**: 유닛 테스트만을 실행하도록 설정되어 있습니다. 이 경우 `unit`이라는 디렉토리 안의 테스트 파일만 실행되도록 `include` 규칙을 지정했습니다.
- **`integrationTest` task**: 통합 테스트 전용 task를 새로 정의했습니다. 통합 테스트는 `src/integrationTest/java` 디렉토리에 위치한 테스트 코드를 실행하며, 이를 위한 별도의 classpath와 sourceSets가 정의되었습니다.
- **`sourceSets`**: 통합 테스트를 위한 별도의 소스 디렉토리를 정의하였습니다. 이 디렉토리에는 통합 테스트 코드 및 리소스 파일이 저장됩니다.

### 3. **테스트 task의 다른 활용법**
- **필터링**: `test` task 내에서 특정 패턴의 테스트만 실행하거나 제외할 수 있습니다.
  
  ```groovy
  test {
      useJUnitPlatform()
      include '**/unit/**'  // 특정 패턴의 테스트만 포함
      exclude '**/slow/**'  // 특정 패턴의 테스트는 제외
  }
  ```

- **환경별 테스트 분리**: 환경 변수나 프로파일에 따라 테스트 실행을 다르게 할 수 있습니다.

  ```groovy
  task integrationTest(type: Test) {
      useJUnitPlatform()
      systemProperty 'environment', 'integration'
  }

  test {
      useJUnitPlatform()
      systemProperty 'environment', 'unit'
  }
  ```

  여기서 `systemProperty`를 통해 테스트 실행 시 특정 환경 변수를 전달하여, 테스트 로직 내에서 이를 참고할 수 있습니다.

### 4. **멀티 모듈 프로젝트에서의 활용**
멀티 모듈 프로젝트의 경우, 각 모듈별로 독립된 `test` task를 정의하거나, 공통 테스트 task를 상속하는 방식으로 재사용성을 높일 수 있습니다.

---

### 요약:
Gradle의 `test` task는 여러 기능으로 쪼개거나 커스텀하여 다양한 테스트 요구 사항에 대응할 수 있습니다. 이를 통해 유닛 테스트와 통합 테스트, 성능 테스트 등 다양한 종류의 테스트를 별도의 task로 나누고, CI/CD 파이프라인에서 유연하게 활용할 수 있습니다.
