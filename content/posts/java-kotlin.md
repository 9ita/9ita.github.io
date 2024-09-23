---
author: "seonuk1218"
title: "Kotlin (learn with java)"
image: ""
draft: false
date: 2024-09-20
description: ""
tags: ["kotlin", "language"]
archives: ["2024/09"]
---

## 개요

### Kotlin이 만들어진 원리

Kotlin은 **JetBrains**에서 2011년에 발표한 JVM 기반의 프로그래밍 언어로, **간결성**, **안전성**, **상호 운용성**을 목표로 설계되었다. Kotlin은 Java를 기반으로 발전한 언어로, **JVM** 위에서 실행되며 **Java와 100% 호환**될 수 있도록 설계되었다. Kotlin을 만든 주요 이유는 Java의 **오래된 문법과 복잡함**을 개선하여 더 간결하고 현대적인 언어를 제공하는 것이다.

#### Kotlin이 만들어진 배경 및 원리
1. **간결성**:
   Kotlin은 반복적인 코드 작성을 줄이기 위해 만들어졌다. 예를 들어, `getter/setter`를 자동으로 생성하는 등, 보일러플레이트 코드를 최소화하는 데 초점을 맞췄다.
   
2. **안전성**:
   Java의 대표적인 문제 중 하나인 **NullPointerException(NPE)**을 줄이기 위해 Kotlin은 **nullable과 non-nullable 타입 시스템**을 도입했다. 이를 통해 컴파일 타임에 null에 대한 오류를 감지할 수 있도록 설계되었다.

3. **상호 운용성**:
   Kotlin은 Java와의 **완벽한 상호 운용성**을 지원한다. 같은 JVM에서 실행되므로, Kotlin과 Java 코드는 동일한 프로젝트에서 혼용될 수 있으며, Kotlin으로 작성한 코드가 Java에서 호출될 수 있고 그 반대도 가능하다.

4. **함수형 프로그래밍 지원**:
   Kotlin은 Java 8에서 추가된 람다 표현식, 고차 함수 등의 **함수형 프로그래밍 패러다임**을 더욱 강력하게 지원한다. Java보다 더 강력한 함수형 프로그래밍 기능을 제공하며, 이를 통해 더 간결하고 효율적인 코드를 작성할 수 있다.

---

### Kotlin 컴파일 원리: 같은 `.class` 파일이 생성되는가?

#### Kotlin 컴파일러의 동작 원리
Kotlin은 Java와 동일하게 **JVM 위에서 실행**되기 때문에, Kotlin 코드는 **`.kt`** 파일로 작성된 후 **바이트코드로 컴파일**된다. 이 바이트코드는 Java 바이트코드와 동일한 형태로, JVM에서 실행될 수 있는 `.class` 파일을 생성한다.

따라서 **Kotlin 코드가 컴파일되면** Java 코드와 동일하게 `.class` 파일이 생성되며, 이 `.class` 파일은 JVM에서 Java로 작성된 것과 마찬가지로 실행된다. 또한, 이 `.class` 파일은 Java 클래스처럼 동작하기 때문에 **Java 코드와 상호 운용**이 가능하다.

#### Kotlin 컴파일러 동작 흐름
1. **Kotlin 소스 파일(.kt)** → **Kotlin 컴파일러** → **바이트코드(.class)**.
   - Kotlin 컴파일러는 `.kt` 파일을 JVM에서 실행 가능한 **Java 바이트코드**로 컴파일한다.
   - 이 바이트코드는 Java로 작성된 `.java` 파일을 컴파일할 때 생성되는 `.class` 파일과 동일하다.

2. **JVM에서 실행**:
   - 생성된 `.class` 파일은 Java로 컴파일된 클래스 파일과 동일하게 JVM에서 로드되고 실행된다.
   
#### Kotlin 컴파일 후 생성되는 `.class` 파일

```kotlin
// Kotlin 코드
class Person(val name: String, val age: Int) {
    fun greet() {
        println("Hello, my name is $name.")
    }
}
```

Kotlin 컴파일러로 위의 코드를 컴파일하면 다음과 같은 **바이트코드 파일(`.class`)**이 생성된다:

- `Person.class`: Kotlin 코드를 JVM이 이해할 수 있는 바이트코드로 변환한 파일. Java로 컴파일한 파일과 동일한 `.class` 파일 형식을 갖는다.

Kotlin 컴파일러는 내부적으로 Java 컴파일러와 유사하게 작동하며, Kotlin 코드를 **AST(Abstract Syntax Tree)**로 변환한 후, 이 정보를 사용해 바이트코드를 생성한다. 이 바이트코드는 Java의 바이트코드와 동일한 JVM 명령어로 변환된다.

---

### Kotlin 컴파일의 특징

1. **Kotlin과 Java의 동일한 바이트코드 생성**:
   Kotlin은 Java와 동일한 `.class` 파일을 생성하기 때문에, JVM에서 Kotlin과 Java가 **동일하게 동작**한다. 따라서 Kotlin으로 컴파일된 `.class` 파일은 Java 클래스 파일처럼 동작하며, Java에서 Kotlin으로 컴파일된 클래스를 호출하는 데에도 문제가 없다.

2. **JVM 외에도 Kotlin의 다중 플랫폼 지원**:
   Kotlin은 JVM 외에도 **JavaScript** 및 **네이티브 코드**로도 컴파일할 수 있다. 이를 통해 JVM 외의 환경에서도 Kotlin을 사용할 수 있으며, 다양한 플랫폼에서 동일한 코드를 실행할 수 있는 **다중 플랫폼 지원**을 제공한다. 이를 위해 Kotlin/Native 및 Kotlin/JS와 같은 별도의 컴파일러 백엔드도 존재한다.

---


## Java 문법과 비교하며 학습


### 1. Creating Classes & Objects

#### 설명:
Java와 Kotlin에서 클래스를 생성하고 객체를 인스턴스화하는 방식은 다소 차이가 있다. Java에서는 클래스를 정의하고 `new` 키워드를 사용하여 객체를 생성한다. 반면 Kotlin에서는 `new` 키워드가 없으며, 함수 호출처럼 객체를 생성할 수 있다.

#### Java 코드:
```java
// Java에서 클래스 생성 및 객체 인스턴스화
class Person {
    String name;
    int age;
    
    public void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

public class Main {
    public static void main(String[] args) {
        Person person = new Person();  // new 키워드로 객체 생성
        person.name = "John";
        person.age = 30;
        person.displayInfo();
    }
}
```

#### Kotlin 코드:
```kotlin
// Kotlin에서 클래스 생성 및 객체 인스턴스화
class Person {
    var name: String = ""
    var age: Int = 0
    
    fun displayInfo() {
        println("Name: $name, Age: $age")
    }
}

fun main() {
    val person = Person()  // new 키워드 없이 객체 생성
    person.name = "John"
    person.age = 30
    person.displayInfo()
}
```

---

### 2. Class Attributes

#### 설명:
클래스 속성은 클래스 내에서 선언된 변수 또는 상수이다. Java에서는 필드를 사용하고, Kotlin에서는 `var`(가변 속성)와 `val`(불변 속성)을 사용한다. 기본적으로 Kotlin은 getter와 setter를 자동으로 제공한다.

#### Java 코드:
```java
class Car {
    String model;
    int year;
}

public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        car.model = "Tesla";
        car.year = 2023;
        System.out.println("Model: " + car.model + ", Year: " + car.year);
    }
}
```

#### Kotlin 코드:
```kotlin
class Car {
    var model: String = ""
    var year: Int = 0
}

fun main() {
    val car = Car()
    car.model = "Tesla"
    car.year = 2023
    println("Model: ${car.model}, Year: ${car.year}")
}
```

---

### 3. Access Modifiers

#### 설명:
Java와 Kotlin 모두 `public`, `private`, `protected`와 같은 접근 제한자를 사용한다. 다만, Kotlin은 기본적으로 모든 클래스와 멤버가 `public`이다. Java에서는 명시적으로 접근 제한자를 지정하지 않으면 **패키지-프라이빗**(default)이 기본값이다.

#### Java 코드:
```java
class Animal {
    private String species;
    protected int legs;

    public Animal(String species, int legs) {
        this.species = species;
        this.legs = legs;
    }

    public String getSpecies() {
        return species;
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Animal("Dog", 4);
        System.out.println("Species: " + animal.getSpecies() + ", Legs: " + animal.legs);
    }
}
```

#### Kotlin 코드:
```kotlin
class Animal(private var species: String, protected var legs: Int) {

    fun getSpecies(): String {
        return species
    }
}

fun main() {
    val animal = Animal("Dog", 4)
    println("Species: ${animal.getSpecies()}, Legs: ${animal.legs}")
}
```

---

### 4. Getters and Setters

#### 설명:
Java에서는 getter와 setter를 수동으로 작성해야 하지만, Kotlin에서는 `var`나 `val`을 사용하면 자동으로 제공된다. Kotlin은 필요할 경우 사용자 정의 getter/setter도 쉽게 정의할 수 있다.

#### Java 코드:
```java
class Person {
    private String name;
    private int age;
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        this.age = age;
    }
}

public class Main {
    public static void main(String[] args) {
        Person person = new Person();
        person.setName("Alice");
        person.setAge(25);
        System.out.println("Name: " + person.getName() + ", Age: " + person.getAge());
    }
}
```

#### Kotlin 코드:
```kotlin
class Person {
    var name: String = ""
    var age: Int = 0
}

fun main() {
    val person = Person()
    person.name = "Alice"
    person.age = 25
    println("Name: ${person.name}, Age: ${person.age}")
}
```

---

### 5. Constructors

#### 설명:
Java에서는 클래스의 생성자를 명시적으로 정의하고, 여러 개의 생성자 오버로딩이 가능하다. Kotlin은 기본 생성자(primary constructor)를 클래스 헤더에 정의할 수 있으며, 보조 생성자(secondary constructor)도 사용할 수 있다.

#### Java 코드:
```java
class Book {
    String title;
    int pages;
    
    public Book(String title, int pages) {
        this.title = title;
        this.pages = pages;
    }
}

public class Main {
    public static void main(String[] args) {
        Book book = new Book("Effective Java", 416);
        System.out.println("Title: " + book.title + ", Pages: " + book.pages);
    }
}
```

#### Kotlin 코드:
```kotlin
class Book(val title: String, val pages: Int)

fun main() {
    val book = Book("Effective Java", 416)
    println("Title: ${book.title}, Pages: ${book.pages}")
}
```

---

### 6. Value & Reference Types

#### 설명:
Java와 Kotlin 모두 **값 타입**(primitive types)과 **참조 타입**(reference types)을 사용한다. 값 타입은 `int`, `double`과 같은 기본 타입을 의미하며, 참조 타입은 클래스 객체를 가리킨다. Kotlin에서는 값 타입도 객체처럼 처리되며, Java의 **박싱/언박싱** 개념을 내포하고 있다.

#### Java 코드:
```java
public class Main {
    public static void main(String[] args) {
        int x = 10;  // Primitive value type
        Integer y = x;  // Reference type (boxing)
        
        String str1 = "Hello";  // Reference type
        String str2 = new String("Hello");  // Different reference, but same value
        
        System.out.println(x);  // 출력: 10
        System.out.println(str1 == str2);  // 출력: false (참조 비교)
        System.out.println(str1.equals(str2));  // 출력: true (값 비교)
    }
}
```

#### Kotlin 코드:
```kotlin
fun main() {
    val x: Int = 10  // Kotlin에서 Int는 객체처럼 취급됨 (박싱/언박싱 필요 없음)
    
    val str1 = "Hello"  // 참조 타입
    val str2 = String("Hello".toCharArray())  // 다른 참조, 같은 값
    
    println(x)  // 출력: 10
    println(str1 === str2)  // 출력: false (참조 비교)
    println(str1 == str2)  // 출력: true (값 비교)
}
```

### 7. Static

#### 설명:
Java와 Kotlin에서 `static` 키워드는 클래스 레벨에서 변수를 정의하거나 메서드를 사용할 때 유용하다. Java에서는 `static` 키워드를 사용하여 정적 멤버를 선언하지만, Kotlin에서는 `companion object`를 사용하여 동일한 기능을 제공한다.

#### Java 코드:
```java
class Calculator {
    static int add(int a, int b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        int result = Calculator.add(5, 10);  // static 메서드는 클래스명으로 호출
        System.out.println(result);  // 출력: 15
    }
}
```

#### Kotlin 코드:
```kotlin
class Calculator {
    companion object {
        fun add(a: Int, b: Int): Int {
            return a + b
        }
    }
}

fun main() {
    val result = Calculator.add(5, 10)  // companion object 메서드는 클래스명으로 호출
    println(result)  // 출력: 15
}
```

---

### 8. Final

#### 설명:
`final` 키워드는 Java에서 클래스, 메서드 또는 변수를 수정할 수 없도록 만들 때 사용된다. Kotlin에서는 `val` 키워드로 불변 변수를 선언하고, 클래스를 상속 불가로 만들기 위해 `final` 키워드를 사용한다(기본적으로 Kotlin의 클래스는 `final`이다).

#### Java 코드:
```java
class Vehicle {
    final int maxSpeed = 120;  // 변수에 final 적용
    
    final void drive() {  // 메서드에 final 적용
        System.out.println("Driving at max speed of " + maxSpeed);
    }
}

class Car extends Vehicle {
    // 오버라이드 불가, final 메서드
}

public class Main {
    public static void main(String[] args) {
        Vehicle car = new Vehicle();
        car.drive();
    }
}
```

#### Kotlin 코드:
```kotlin
open class Vehicle {
    final val maxSpeed: Int = 120  // 변수에 final 적용 (val은 불변)
    
    final fun drive() {  // 메서드에 final 적용
        println("Driving at max speed of $maxSpeed")
    }
}

class Car : Vehicle() {
    // 오버라이드 불가, final 메서드
}

fun main() {
    val car = Vehicle()
    car.drive()
}
```

---

### 9. Packages

#### 설명:
Java와 Kotlin 모두 **패키지**를 사용하여 클래스를 그룹화하고 네임스페이스를 관리한다. Java에서는 `package` 선언을 파일의 맨 위에 추가하고, Kotlin에서도 동일한 방식으로 패키지를 선언한다.

#### Java 코드:
```java
package com.example;

class Person {
    String name;
    int age;
    
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

public class Main {
    public static void main(String[] args) {
        Person person = new Person("John", 30);
        System.out.println(person.name + " is " + person.age + " years old.");
    }
}
```

#### Kotlin 코드:
```kotlin
package com.example

class Person(val name: String, val age: Int)

fun main() {
    val person = Person("John", 30)
    println("${person.name} is ${person.age} years old.")
}
```

---

### 10. Encapsulation

#### 설명:
캡슐화는 객체의 상태(필드)를 보호하고, 외부에서의 직접적인 접근을 제한하며, getter와 setter를 통해서만 상태를 조작할 수 있도록 하는 개념이다. Java와 Kotlin 모두 `private` 접근 제한자를 사용하여 필드를 감추고, `public` 메서드를 통해 접근하는 방식으로 캡슐화를 구현한다.

#### Java 코드:
```java
class Account {
    private double balance;  // 외부에서 접근 불가, private
    
    public Account(double balance) {
        this.balance = balance;
    }
    
    public double getBalance() {  // balance 필드의 값을 반환하는 getter
        return balance;
    }
    
    public void setBalance(double balance) {  // balance 필드를 설정하는 setter
        if (balance >= 0) {
            this.balance = balance;
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Account account = new Account(100.0);
        System.out.println("Balance: " + account.getBalance());
        account.setBalance(200.0);
        System.out.println("Updated Balance: " + account.getBalance());
    }
}
```

#### Kotlin 코드:
```kotlin
class Account(private var balance: Double) {  // 외부에서 접근 불가, private
    
    fun getBalance(): Double {  // balance 필드의 값을 반환하는 getter
        return balance
    }
    
    fun setBalance(balance: Double) {  // balance 필드를 설정하는 setter
        if (balance >= 0) {
            this.balance = balance
        }
    }
}

fun main() {
    val account = Account(100.0)
    println("Balance: ${account.getBalance()}")
    account.setBalance(200.0)
    println("Updated Balance: ${account.getBalance()}")
}
```

---

### 11. Inheritance

#### 설명:
**상속**은 기존 클래스(부모 클래스)의 속성과 메서드를 새로운 클래스(자식 클래스)가 물려받는 개념이다. Java와 Kotlin 모두 `extends`와 `:`를 사용하여 상속을 구현한다. Kotlin에서 클래스는 기본적으로 `final`이므로 상속하려면 `open` 키워드를 사용해야 한다.

#### Java 코드:
```java
class Animal {
    String species;

    public void sound() {
        System.out.println("Some generic sound");
    }
}

class Dog extends Animal {
    public Dog() {
        this.species = "Dog";
    }

    @Override
    public void sound() {
        System.out.println("Bark");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        System.out.println("Species: " + dog.species);
        dog.sound();
    }
}
```

#### Kotlin 코드:
```kotlin
open class Animal {
    open var species: String = "Unknown"

    open fun sound() {
        println("Some generic sound")
    }
}

class Dog : Animal() {
    override var species: String = "Dog"

    override fun sound() {
        println("Bark")
    }
}

fun main() {
    val dog = Dog()
    println("Species: ${dog.species}")
    dog.sound()
}
```

---

### 12. Polymorphism

#### 설명:
**다형성**은 동일한 이름의 메서드나 클래스가 여러 형태로 동작할 수 있는 개념이다. 상속을 통해 부모 클래스를 참조 타입으로 사용하면서 자식 클래스의 메서드를 호출하는 방식으로 구현된다. Java와 Kotlin에서 모두 동일한 방식으로 다형성이 적용된다.

#### Java 코드:
```java
class Animal {
    public void sound() {
        System.out.println("Some generic sound");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {
    @Override
    public void sound() {
        System.out.println("Meow");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal myDog = new Dog();  // 다형성 적용
        Animal myCat = new Cat();
        
        myDog.sound();  // Bark
        myCat.sound();  // Meow
    }
}
```

#### Kotlin 코드:
```kotlin
open class Animal {
    open fun sound() {
        println("Some generic sound")
    }
}

class Dog : Animal() {
    override fun sound() {
        println("Bark")
    }
}

class Cat : Animal() {
    override fun sound() {
        println("Meow")
    }
}

fun main() {
    val myDog: Animal = Dog()  // 다형성 적용
    val myCat: Animal = Cat()
    
    myDog.sound()  // Bark
    myCat.sound()  // Meow
}
```

---

### 13. Overriding & Overloading

#### 설명:
- **오버라이딩**은 자식 클래스가 부모 클래스의 메서드를 재정의하는 개념이다.
- **오버로딩**은 동일한 이름의 메서드를 매개변수의 타입이나 개수에 따라 다르게 정의하는 것이다.

#### Java 코드 (Overriding & Overloading):
```java
class Calculator {
    public int add(int a, int b) {  // Overloading
        return a + b;
    }
    
    public int add(int a, int b, int c) {  // Overloading
        return a + b + c;
    }
}

class AdvancedCalculator extends Calculator {
    @Override
    public int add(int a, int b) {  // Overriding
        System.out.println("Advanced addition");
        return super.add(a, b);
    }
}

public class Main {
    public static void main(String[] args) {
        AdvancedCalculator calc = new AdvancedCalculator();
        System.out.println(calc.add(5, 10));  // Overriding
        System.out.println(calc.add(1, 2, 3));  // Overloading
    }
}
```

#### Kotlin 코드 (Overriding & Overloading):
```kotlin
open class Calculator {
    open fun add(a: Int, b: Int): Int {  // Overloading
        return a + b
    }
    
    fun add(a: Int, b: Int, c: Int): Int {  // Overloading
        return a + b + c
    }
}

class AdvancedCalculator : Calculator() {
    override fun add(a: Int, b: Int): Int {  // Overriding
        println("Advanced addition")
        return super.add(a, b)
    }
}

fun main() {
    val calc = AdvancedCalculator()
    println(calc.add(5, 10))  // Overriding
    println(calc.add(1, 2, 3))  // Overloading
}
```

---

### 14. Abstract Classes

#### 설명:
**추상 클래스**는 인스턴스화될 수 없으며, 추상 메서드를 포함하여 자식 클래스에서 구현하도록 강제한다. Java와 Kotlin 모두 `abstract` 키워드를 사용하여 추상 클래스를 정의한다.

#### Java 코드:
```java
abstract class Animal {
    abstract void sound();  // 추상 메서드

    public void sleep() {
        System.out.println("Sleeping...");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Bark");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal dog = new Dog();
        dog.sound();
        dog.sleep();
    }
}
```

#### Kotlin 코드:
```kotlin
abstract class Animal {
    abstract fun sound()  // 추상 메서드

    fun sleep() {
        println("Sleeping...")
    }
}

class Dog : Animal() {
    override fun sound() {
        println("Bark")
    }
}

fun main() {
    val dog: Animal = Dog()
    dog.sound()
    dog.sleep()
}
```

---

### 15. Interfaces

#### 설명:
**인터페이스**는 클래스가 구현해야 하는 메서드와 속성을 정의하는 추상화된 형태이다. Java와 Kotlin 모두 인터페이스를 정의할 수 있으며, 다중 구현이 가능하다.

#### Java 코드:
```java
interface Animal {
    void sound();
}

interface Playable {
    void play();
}

class Dog implements Animal, Playable {
    @Override
    public void sound() {
        System.out.println("Bark");
    }

    @Override
    public void play() {
        System.out.println("Playing fetch");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.sound();
        dog.play();
    }
}
```

#### Kotlin 코드:
```kotlin
interface Animal {
    fun sound()
}

interface Playable {
    fun play()
}

class Dog : Animal, Playable {
    override fun sound() {
        println("Bark")
    }

    override fun play() {
        println("Playing fetch")
    }
}

fun main() {
    val dog = Dog()
    dog.sound()
    dog.play()
}
```

---

### 16. Casting

#### 설명:
**캐스팅**은 하나의 데이터 타입을 다른 데이터 타입으로 변환하는 과정이다. Java와 Kotlin에서 객체나 기본 타입을 명시적으로 또는 암시적으로 캐스팅할 수 있다. 기본 타입은 자동으로 변환되는 경우도 있지만, 객체 타입의 변환은 명시적으로 해야 한다.

#### Java 코드:
```java
public class Main {
    public static void main(String[] args) {
        int num = 10;
        double d = (double) num;  // 명시적 캐스팅 (int -> double)
        
        System.out.println(d);  // 출력: 10.0
    }
}
```

#### Kotlin 코드:
```kotlin
fun main() {
    val num: Int = 10
    val d: Double = num.toDouble()  // Kotlin에서는 .toDouble() 사용
    
    println(d)  // 출력: 10.0
}
```

---

### 17. Downcasting

#### 설명:
**다운캐스팅**은 상위 클래스 타입의 객체를 하위 클래스 타입으로 변환하는 과정이다. Java에서는 명시적으로 캐스팅이 필요하며, 안전하지 않다면 `ClassCastException`이 발생할 수 있다. Kotlin에서는 `as` 키워드를 사용하며, `as?`를 사용하면 안전한 캐스팅이 가능하다.

#### Java 코드:
```java
class Animal {}
class Dog extends Animal {
    public void bark() {
        System.out.println("Bark");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog();  // 업캐스팅
        if (animal instanceof Dog) {
            Dog dog = (Dog) animal;  // 다운캐스팅
            dog.bark();  // 출력: Bark
        }
    }
}
```

#### Kotlin 코드:
```kotlin
open class Animal
class Dog : Animal() {
    fun bark() {
        println("Bark")
    }
}

fun main() {
    val animal: Animal = Dog()  // 업캐스팅
    val dog = animal as? Dog  // 안전한 다운캐스팅
    dog?.bark()  // 출력: Bark
}
```

---

### 18. Anonymous Classes

#### 설명:
**익명 클래스**는 클래스를 정의하지 않고 즉석에서 일회성 클래스를 구현할 때 사용된다. Java에서는 인터페이스나 클래스를 상속받아 이를 구현할 수 있다. Kotlin에서는 `object` 키워드를 사용하여 익명 객체를 정의할 수 있다.

#### Java 코드:
```java
interface Greeting {
    void sayHello();
}

public class Main {
    public static void main(String[] args) {
        Greeting greeting = new Greeting() {  // 익명 클래스
            @Override
            public void sayHello() {
                System.out.println("Hello, World!");
            }
        };
        greeting.sayHello();  // 출력: Hello, World!
    }
}
```

#### Kotlin 코드:
```kotlin
interface Greeting {
    fun sayHello()
}

fun main() {
    val greeting = object : Greeting {  // 익명 객체
        override fun sayHello() {
            println("Hello, World!")
        }
    }
    greeting.sayHello()  // 출력: Hello, World!
}
```

---

### 19. Inner Classes

#### 설명:
**내부 클래스**는 클래스 내에서 정의된 클래스로, 외부 클래스의 멤버에 접근할 수 있다. Java에서는 `static` 키워드 없이 선언된 클래스는 내부 클래스이다. Kotlin에서도 기본적으로 중첩 클래스는 내부 클래스로 간주되며, `inner` 키워드를 사용하여 내부 클래스로 지정할 수 있다.

#### Java 코드:
```java
class Outer {
    private String message = "Hello from outer class";

    class Inner {
        public void displayMessage() {
            System.out.println(message);  // 외부 클래스 멤버 접근
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();  // 내부 클래스 객체 생성
        inner.displayMessage();  // 출력: Hello from outer class
    }
}
```

#### Kotlin 코드:
```kotlin
class Outer {
    private val message = "Hello from outer class"

    inner class Inner {
        fun displayMessage() {
            println(message)  // 외부 클래스 멤버 접근
        }
    }
}

fun main() {
    val outer = Outer()
    val inner = outer.Inner()  // 내부 클래스 객체 생성
    inner.displayMessage()  // 출력: Hello from outer class
}
```

---

### 20. The equals() method

#### 설명:
`equals()` 메서드는 두 객체가 동일한 값을 가지는지 비교할 때 사용된다. Java에서는 기본적으로 `Object.equals()` 메서드가 객체 참조를 비교하지만, 이 메서드를 재정의하여 값 비교를 수행할 수 있다. Kotlin에서는 `==` 연산자가 자동으로 `equals()` 메서드를 호출한다.

#### Java 코드:
```java
class Person {
    String name;

    Person(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Person person = (Person) obj;
        return name.equals(person.name);
    }
}

public class Main {
    public static void main(String[] args) {
        Person p1 = new Person("John");
        Person p2 = new Person("John");
        System.out.println(p1.equals(p2));  // 출력: true
    }
}
```

#### Kotlin 코드:
```kotlin
data class Person(val name: String)

fun main() {
    val p1 = Person("John")
    val p2 = Person("John")
    println(p1 == p2)  // Kotlin에서 ==는 equals() 호출. 출력: true
}
```

---

### 21. Enums

#### 설명:
**열거형(Enum)**은 상수 집합을 정의할 때 사용된다. Java와 Kotlin 모두 열거형을 정의할 수 있으며, 열거형 내에 필드와 메서드를 추가할 수도 있다. Java는 `enum` 키워드를 사용하고, Kotlin에서는 `enum class`를 사용하여 열거형을 정의한다.

#### Java 코드:
```java
enum Direction {
    NORTH, SOUTH, EAST, WEST
}

public class Main {
    public static void main(String[] args) {
        Direction dir = Direction.NORTH;
        System.out.println("Direction: " + dir);  // 출력: Direction: NORTH
    }
}
```

#### Kotlin 코드:
```kotlin
enum class Direction {
    NORTH, SOUTH, EAST, WEST
}

fun main() {
    val dir = Direction.NORTH
    println("Direction: $dir")  // 출력: Direction: NORTH
}
```

---

### 22. Common Java API

#### 설명:
**Java 표준 API**에는 다양한 클래스와 라이브러리가 포함되어 있으며, 이 중 자주 사용되는 몇 가지는 `java.util`, `java.lang`, `java.io`에 있다. Kotlin은 대부분의 Java 표준 API를 활용할 수 있으며, Kotlin 고유의 라이브러리도 제공한다.

#### Java 코드 (ArrayList):
```java
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        System.out.println(list);  // 출력: [Apple, Banana]
    }
}
```

#### Kotlin 코드 (ArrayList):
```kotlin
fun main() {
    val list = arrayListOf("Apple", "Banana")
    println(list)  // 출력: [Apple, Banana]
}
```

---

### 23. Exception Handling

#### 설명:
Java와 Kotlin 모두 **예외 처리**를 위해 `try-catch-finally` 블록을 사용한다. Java에서는 예외를 던지기 위해 `throw` 키워드를 사용하고, 예외 클래스는 `Exception`을 상속받는다. Kotlin에서도 유사하게 작동하며, 모든 예외는 `Throwable` 클래스를 상속받는다.

#### Java 코드:
```java
public class Main {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;  // ArithmeticException 발생
        } catch (ArithmeticException e) {
            System.out.println("Error: " + e.getMessage());
        } finally {
            System.out.println("Execution finished.");
        }
    }
}
```

#### Kotlin 코드:
```kotlin
fun main() {
    try {
        val result = 10 / 0  // ArithmeticException 발생
    } catch (e: ArithmeticException) {
        println("Error: ${e.message}")
    } finally {
        println("Execution finished.")
    }
}
```

---

### 24. Multiple Exceptions

#### 설명:
Java와 Kotlin 모두 여러 예외를 처리하기 위해 여러 `catch` 블록을 사용할 수 있다. Java는 다중 예외를 `|` 연산자로 하나의 `catch` 블록에서 처리할 수 있지만, Kotlin에서는 각 예외를 개별적으로 처리하거나 공통 상위 클래스에서 처리할 수 있다.

#### Java 코드:
```java
public class Main {
    public static void main(String[] args) {
        try {
            int[] arr = new int[3];
            System.out.println(arr[5]);  // ArrayIndexOutOfBoundsException 발생
        } catch (ArithmeticException | ArrayIndexOutOfBoundsException e) {
            System.out.println("Error: " + e.getMessage());
        } finally {
            System.out.println("Execution finished.");
        }
    }
}
```

#### Kotlin 코드:
```kotlin
fun main() {
    try {
        val arr = arrayOf(1, 2, 3)
        println(arr[5])  // ArrayIndexOutOfBoundsException 발생
    } catch (e: ArithmeticException) {
        println("Arithmetic Error: ${e.message}")
    } catch (e: ArrayIndexOutOfBoundsException) {
        println("Array Index Error: ${e.message}")
    } finally {
        println("Execution finished.")
    }
}
```

---

### 25. Threads + Coroutine

#### 설명:
Java에서 **스레드**는 동시성 프로그래밍을 위해 사용되며, `Thread` 클래스나 `Runnable` 인터페이스를 통해 정의할 수 있다. Kotlin에서는 더 간결하고 효율적인 **코루틴(Coroutine)**을 제공하며, 비동기 작업을 쉽게 처리할 수 있다.

#### Java 코드 (Thread):
```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running");
    }
}

public class Main {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start();  // 스레드 시작
    }
}
```

#### Kotlin 코드 (Coroutine):
```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    launch {
        println("Coroutine is running")
    }
}
```

Kotlin의 코루틴은 `launch`나 `async`를 통해 스레드보다 경량화된 방식으로 비동기 작업을 처리할 수 있으며, `runBlocking`은 메인 스레드를 차단하여 비동기 코드를 동기적으로 실행할 수 있게 한다.

---

### 26. Runtime vs Checked Exceptions

#### 설명:
**체크 예외(Checked Exception)**는 컴파일 시점에서 처리해야 하며, 반드시 `try-catch`로 처리하거나 `throws`로 선언해야 한다. **런타임 예외(Runtime Exception)**는 컴파일 시 체크되지 않으며, 선택적으로 처리할 수 있다. Java는 둘 다 지원하지만, Kotlin은 체크 예외를 강제하지 않는다.

#### Java 코드 (Checked Exception):
```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        try {
            Scanner scanner = new Scanner(new File("nonexistent.txt"));  // Checked Exception
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + e.getMessage());
        }
    }
}
```

#### Java 코드 (Runtime Exception):
```java
public class Main {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;  // Runtime Exception
        } catch (ArithmeticException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

#### Kotlin 코드 (Runtime Exception):
```kotlin
fun main() {
    try {
        val result = 10 / 0  // Runtime Exception
    } catch (e: ArithmeticException) {
        println("Error: ${e.message}")
    }
}
```

Kotlin에서는 체크 예외가 강제되지 않으며, 모든 예외가 런타임 예외처럼 처리된다.

---

### 27. Iterators

#### 설명:
**이터레이터(Iterator)**는 컬렉션을 순차적으로 탐색하는 인터페이스다. Java에서는 `Iterator`를 통해 컬렉션을 순회할 수 있고, Kotlin은 `iterator()` 함수와 `for` 루프를 통해 컬렉션을 간결하게 순회한다.

#### Java 코드:
```java
import java.util.ArrayList;
import java.util.Iterator;

public class Main {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        Iterator<String> it = list.iterator();
        while (it.hasNext()) {
            System.out.println(it.next());
        }
    }
}
```

#### Kotlin 코드:
```kotlin
fun main() {
    val list = listOf("Apple", "Banana")
    for (item in list) {
        println(item)
    }
}
```

Kotlin은 이터레이터를 명시적으로 사용하지 않고, `for` 루프를 통해 자동으로 이터레이터를 활용한다.

---

### 28. Working with Files

#### 설명:
Java와 Kotlin에서 파일 작업은 `java.io` 패키지를 사용한다. Java에서는 `File`, `Scanner`, `BufferedReader` 등을 사용해 파일을 읽고 쓰며, Kotlin에서는 더 간결하게 처리할 수 있는 확장 함수를 제공한다.

#### Java 코드 (파일 읽기):
```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        try {
            File file = new File("example.txt");
            Scanner scanner = new Scanner(file);
            while (scanner.hasNextLine()) {
                System.out.println(scanner.nextLine());
            }
            scanner.close();
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + e.getMessage());
        }
    }
}
```

#### Kotlin 코드 (파일 읽기):
```kotlin
import java.io.File

fun main() {
    val file = File("example.txt")
    file.forEachLine { println(it) }
}
```

Kotlin에서는 `File.forEachLine` 함수를 통해 간결하게 파일 내용을 읽을 수 있다.

#### Java 코드 (파일 쓰기):
```java
import java.io.FileWriter;
import java.io.IOException;

public class Main {
    public static void main(String[] args) {
        try {
            FileWriter writer = new FileWriter("output.txt");
            writer.write("Hello, world!");
            writer.close();
        } catch (IOException e) {
            System.out.println("An error occurred: " + e.getMessage());
        }
    }
}
```

#### Kotlin 코드 (파일 쓰기):
```kotlin
import java.io.File

fun main() {
    val file = File("output.txt")
    file.writeText("Hello, world!")
}
```

Kotlin의 `writeText()` 함수는 Java에 비해 매우 간단한 문법으로 파일에 텍스트를 기록할 수 있다.

---

1. Creating Classes & Objects

2. Class Attributes

3. Access Modifiers

4. Getters and Setters

5. Constructors

6. Value & Reference Types

7. Static

8. Final

9. Packages

10. Encapsulation

11. Inheritance

12. Polymorphism

13. Overriding & Overloading

14. Abstract Classes

15. Interfaces

16. Casting

17. Downcasting

18. Anonymous Classes

19. Inner Classes

20. The equals() method

21. Enums

22. Common Java API

23. Exception Handling

24. Multiple Exceptions

25. Threads

+ Coroutine

26. Runtime VS Checked Exceptions

27. Iterators

28. Working with Files