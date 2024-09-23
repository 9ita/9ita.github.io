---
author: "seonuk1218"
title: "Encoding"
image: ""
draft: false
date: 2024-05-07
description: ""
tags: ["encoding"]
archives: ["2024/05"]
---

### ASCII 기반 + Unicode 체계 = UTF-8

UTF-8은 ASCII 기반으로 시작된 인코딩 방식이며, 기본적으로 한 문자는 8비트(1바이트)로 표현되며 최대 3바이트까지 확장 가능하다. 이를 통해 ASCII를 포함한 전 세계의 문자들을 효율적으로 표현할 수 있다. 

### MS949

MS949는 EUC-KR 인코딩 방식에 기반을 두며, 한글을 2바이트로 표현한다. 주로 Windows 환경에서 사용되는 인코딩 방식으로, UTF-8과는 다른 체계를 가지고 있다.

## Java에서의 인코딩 변환

### Java에서의 문자열 처리와 인코딩 변환 원리

Java는 문자열을 **UTF-16**을 기반으로 내부에서 처리한다. `String` 클래스는 변경 불가능한(immutable) 유니코드 문자열을 관리하는데, 각 문자는 16비트(2바이트)의 유니코드 값으로 저장된다. Java의 기본 문자셋이 UTF-16이라는 점을 이해하면, 다양한 인코딩 간의 변환 과정이 어떻게 이루어지는지 명확해진다.

#### Java의 `String` 객체와 유니코드
Java에서 문자열을 다룰 때, `String` 객체는 문자들을 UTF-16 인코딩으로 관리한다. 이는 다음과 같은 과정을 거친다:

1. **문자 입력**: Java의 문자열 리터럴이나 `new String()`을 통해 문자열을 생성하면, 이 문자열은 자동으로 UTF-16으로 변환되어 `char[]` 배열에 저장된다. Java의 `char` 타입은 2바이트 크기를 가지며, UTF-16 코드 유닛을 저장한다.

2. **인코딩 변환**: `String.getBytes(Charset)` 메서드를 사용해 문자열을 특정 인코딩 형식의 바이트 배열로 변환할 수 있다. 이 과정은 유니코드(UTF-16)로 저장된 `char[]` 배열을, 지정한 인코딩 형식에 맞게 바이트 배열로 변환하는 작업이다.

```java
String utf8Str = "example";  // "example"은 내부적으로 UTF-16으로 관리됨
byte[] utf8Bytes = utf8Str.getBytes(StandardCharsets.UTF_8);  // UTF-16에서 UTF-8로 변환
```

위 코드에서 `getBytes` 메서드는 UTF-16에서 UTF-8로 변환하는데, 각 유니코드 문자를 UTF-8로 매핑하여 바이트 배열로 변환한다.

#### 인코딩 변환의 내부 동작

인코딩 변환 과정은 다음과 같이 이루어진다:

1. **UTF-16 to UTF-8 변환**: 
   - UTF-16은 고정 2바이트 크기로 문자를 표현하지만, UTF-8은 가변 길이 인코딩이다. ASCII 문자는 1바이트로, 유니코드 문자 중 일부는 2~3바이트로 인코딩된다.
   - Java의 `getBytes("UTF-8")`는 내부적으로 각 UTF-16 코드 유닛을 UTF-8로 변환하는 알고리즘을 사용하여 유니코드 문자의 범위에 따라 적절한 바이트 길이를 할당한다.

2. **UTF-8 to MS949 변환**:
   - UTF-8로 변환된 바이트 배열은 다시 `String` 생성자를 통해 MS949 같은 다른 문자셋으로 변환할 수 있다. 이 과정에서는 Java의 `CharsetEncoder`와 `CharsetDecoder`가 사용된다.
   - `CharsetDecoder`는 UTF-8 바이트 배열을 먼저 UTF-16으로 디코딩한 후, MS949에 맞게 다시 재인코딩한다. 즉, Java는 항상 중간에 유니코드(UTF-16)를 거쳐 변환을 처리한다.

```java
String ms949Str = new String(utf8Bytes, "MS949");  // UTF-8 바이트 배열을 MS949로 재인코딩
```

#### `Charset`과 `CharsetEncoder`/`CharsetDecoder`
Java에서 다양한 인코딩 변환은 **`java.nio.charset.Charset`**, **`CharsetEncoder`**, 그리고 **`CharsetDecoder`** 클래스들을 통해 이루어진다. 이러한 클래스들은 문자셋 인코딩 및 디코딩을 저수준에서 수행하며, 내부적으로는 `ByteBuffer`와 `CharBuffer`를 사용해 바이트와 문자 데이터를 주고받는다.

- **`CharsetEncoder`**: UTF-16의 `char[]` 데이터를 특정 문자셋(예: MS949)에 맞게 바이트로 변환하는 역할을 한다.
- **`CharsetDecoder`**: 특정 문자셋의 바이트 배열을 유니코드(UTF-16)의 `char[]`로 디코딩하는 역할을 한다.

Java에서 `String` 객체는 `Charset`에 의존하여 다른 문자셋으로 인코딩된 바이트 배열을 해석하고 변환하는데, 이 과정에서 인코딩이 지원하지 않는 문자가 있다면 예외가 발생하거나 해당 문자는 손실될 수 있다.

#### 인코딩 실패와 손실
인코딩 변환 중, 특정 문자셋에서 지원되지 않는 문자가 존재할 경우, Java는 기본적으로 해당 문자를 대체(`?` 또는 유사한 문자)하거나 예외(`MalformedInputException`, `UnmappableCharacterException`)를 던진다. 예를 들어, UTF-8에 존재하는 특수 문자나 이모지가 MS949에서 지원되지 않을 수 있다.

```java
CharsetEncoder encoder = Charset.forName("MS949").newEncoder();
encoder.onUnmappableCharacter(CodingErrorAction.REPLACE);  // 대체 문자로 처리
```

#### URL Encoding
URL 인코딩은 퍼센트 인코딩 방식으로, 특수 문자를 안전하게 URL로 전송하기 위한 인코딩이다. Java에서는 `URLEncoder`와 `URLDecoder`를 통해 이 작업을 수행하며, UTF-8 같은 문자셋을 명시적으로 설정할 수 있다.

```java
String encodedUrl = URLEncoder.encode("https://example.com?query=한글", "UTF-8");
```

Java에서 **MS949**와 **ISO 9941** 간의 인코딩 변환 역시 같은 원리로 가능하다. 핵심은 Java가 모든 문자열을 **UTF-16**으로 내부에서 처리한다는 점이다. 즉, MS949로 인코딩된 바이트 배열을 **UTF-16**으로 변환한 후, 이를 다시 **ISO 9941**로 변환하는 과정이 동일하게 적용된다.

### MS949 → ISO 9941 변환 원리

1. **MS949로 인코딩된 바이트 배열을 UTF-16으로 디코딩**:  
   먼저 MS949로 인코딩된 바이트 배열을 `CharsetDecoder`를 사용하여 UTF-16(즉, Java의 내부 `String` 객체)으로 디코딩한다.

2. **UTF-16에서 ISO 9941로 인코딩**:  
   그 다음, UTF-16으로 변환된 문자열을 `CharsetEncoder`를 사용하여 ISO 9941로 인코딩된 바이트 배열로 변환한다.

### 변환 과정 예시

```java
// MS949로 인코딩된 문자열을 ISO 9941로 변환하는 예시
import java.nio.charset.Charset;

public class EncodingConverter {
    public static void main(String[] args) throws Exception {
        // MS949로 인코딩된 문자열
        String originalStr = "한글 텍스트";  // Java 내부에서는 UTF-16으로 처리됨
        byte[] ms949Bytes = originalStr.getBytes("MS949");

        // MS949에서 UTF-16으로 변환
        String decodedStr = new String(ms949Bytes, "MS949");

        // UTF-16에서 ISO 9941로 변환
        byte[] iso9941Bytes = decodedStr.getBytes("ISO-8859-1");  // ISO 9941을 ISO 8859-1로 가정
        String iso9941Str = new String(iso9941Bytes, "ISO-8859-1");

        // 결과 출력
        System.out.println(iso9941Str);
    }
}
```

### 변환 과정 설명

1. **MS949 → UTF-16**:
   - `String.getBytes("MS949")`는 UTF-16로 관리되는 문자열을 MS949로 인코딩된 바이트 배열로 변환한다.
   - 다시 `new String(ms949Bytes, "MS949")`를 사용해 MS949 인코딩 바이트 배열을 UTF-16 문자열로 디코딩한다.

2. **UTF-16 → ISO 9941**:
   - `decodedStr.getBytes("ISO-8859-1")`는 UTF-16로 관리되는 문자열을 ISO 9941(여기서는 `ISO-8859-1`로 가정)로 인코딩된 바이트 배열로 변환한다.
   - `ISO-8859-1`은 가장 흔한 ISO 9941 문자셋 중 하나다. `ISO-8859-1`은 라틴 알파벳을 지원하는 ISO 표준이다. 만약 다른 ISO 9941 변형을 사용하고자 한다면, 대응하는 문자셋 이름을 사용하면 된다.

### 주의할 점

- **문자셋 불일치로 인한 손실**:  
  MS949는 주로 한글과 관련된 문자셋이고, ISO 9941(예: ISO 8859-1)은 라틴 문자를 주로 다룬다. 따라서 MS949에서 표현 가능한 한글 문자는 ISO 9941로 변환하는 과정에서 손실될 수 있다. 변환 과정에서 특정 문자가 잘못 인코딩되거나, 변환되지 않는 경우가 발생할 수 있다.

  예를 들어, `ISO-8859-1`로 인코딩할 때 MS949에서만 표현 가능한 문자는 손실되거나 대체 문자가 들어갈 수 있다. 이는 **문자셋이 지원하는 문자 범위**의 차이 때문이다.

- **오류 처리**:  
  Java에서는 인코딩 변환 중 잘못된 바이트 시퀀스가 발생할 경우 기본적으로 예외를 던지거나 대체 문자를 사용하는 방식으로 오류를 처리한다. `CharsetEncoder`와 `CharsetDecoder`의 `CodingErrorAction` 설정을 통해 이러한 문제를 다룰 수 있다.

  ```java
  CharsetDecoder decoder = Charset.forName("MS949").newDecoder();
  decoder.onMalformedInput(CodingErrorAction.REPORT);  // 잘못된 입력 시 예외 발생
  decoder.onUnmappableCharacter(CodingErrorAction.REPLACE);  // 매핑 불가능한 문자는 대체
  ```
---

### 한글 URL 인코딩 원리

URL 인코딩은 웹 브라우저가 특수 문자나 비-ASCII 문자를 안전하게 전송하기 위해 사용하는 표준 인코딩 방식이다. 일반적으로 URL에서 허용되지 않거나 특정 의미를 가지는 문자는 인코딩되어 전송되는데, 이 인코딩 방식은 **퍼센트 인코딩(percent-encoding)**이라고도 불린다.

한글을 포함한 비-ASCII 문자는 URL에 직접 포함될 수 없기 때문에 **UTF-8** 같은 문자셋으로 인코딩한 후, 그 바이트 값을 `%` 기호와 함께 16진수로 표기하는 방식으로 변환된다. 한글과 같은 다바이트 문자는 여러 바이트로 표현되며, 각 바이트는 `%XX` 형식으로 인코딩된다.

### URL 인코딩의 동작 원리

#### UTF-8 기반 인코딩

1. **한글의 유니코드 값**:  
   한글 "한"의 유니코드는 U+AC00이다. UTF-8에서 유니코드 U+AC00은 3바이트로 표현된다. UTF-8에서의 바이트 시퀀스는 다음과 같다:
   ```
   U+AC00 -> 0xEA 0xB0 0x80
   ```

2. **퍼센트 인코딩 변환**:  
   각 바이트는 `%`와 16진수 값으로 변환된다. 한글 "한"은 3바이트이므로 URL로 인코딩할 때는 다음과 같이 퍼센트 인코딩으로 변환된다:
   ```
   "한" -> %EA%B0%80
   ```

   예를 들어, `https://example.com/search?q=한글`을 URL 인코딩하면 다음과 같은 문자열이 된다:
   ```
   https://example.com/search?q=%ED%95%9C%EA%B8%80
   ```

#### URL 인코딩 과정의 내부 동작

Java에서는 URL 인코딩을 위해 **`URLEncoder`** 클래스를 사용한다. 이 클래스는 주어진 문자열을 **UTF-8**로 인코딩하고, 인코딩된 바이트 배열을 퍼센트 인코딩 형식으로 변환한다.

```java
import java.net.URLEncoder;

public class URLEncoderExample {
    public static void main(String[] args) throws Exception {
        String originalString = "한글";
        String encodedString = URLEncoder.encode(originalString, "UTF-8");
        System.out.println(encodedString);  // 결과: %ED%95%9C%EA%B8%80
    }
}
```

1. **`URLEncoder.encode`의 동작**:  
   - Java는 입력된 문자열을 먼저 **UTF-16**으로 처리하고, 이를 UTF-8로 변환한 후 각 바이트를 퍼센트 인코딩으로 변환한다.
   - UTF-8에서 한글은 3바이트로 표현되므로, "한글"은 총 6개의 퍼센트 인코딩된 값으로 변환된다(`%ED%95%9C%EA%B8%80`).

#### Java의 인코딩 원리 분석

1. **UTF-16에서 UTF-8로 변환**:  
   Java는 내부적으로 모든 문자열을 UTF-16으로 관리한다. 따라서 "한글"이라는 문자열을 처리할 때, 이 문자열은 UTF-16로 저장된다. 이후 URL 인코딩을 수행할 때, Java는 **UTF-16에서 UTF-8로 변환**을 거친다. 이 과정은 `String.getBytes("UTF-8")`로 표현될 수 있다.

2. **퍼센트 인코딩**:  
   UTF-8로 변환된 바이트 배열은 각 바이트를 퍼센트 인코딩으로 표현한다. 퍼센트 인코딩은 URL에 안전하지 않은 문자, 예를 들어 공백이나 특수 문자(`&`, `=`, `?`) 등을 `%`와 16진수 값으로 변환하여 표현하는 방식이다. 이는 주로 URL에서 안전하지 않은 문자를 안전하게 전송하기 위한 방식이다.

3. **한글 처리**:  
   한글을 포함한 비-ASCII 문자는 UTF-8에서 다바이트로 처리되기 때문에, 각 바이트는 개별적으로 퍼센트 인코딩된다. 한글 "한"은 UTF-8에서 3바이트(`0xED 0x95 0x9C`)로 표현되고, 각 바이트는 `%ED`, `%95`, `%9C`로 변환된다. 이 과정이 모든 한글 문자에 대해 동일하게 적용된다.

### 한글 URL 인코딩의 주의점

1. **브라우저와 서버 간의 인코딩 호환성**:  
   대부분의 브라우저와 서버는 기본적으로 **UTF-8**을 지원하기 때문에 한글 URL 인코딩이 문제없이 작동한다. 그러나 일부 시스템에서 **EUC-KR**이나 **ISO 8859-1**과 같은 다른 문자셋을 사용하는 경우에는 한글이 제대로 인코딩되지 않거나, 깨진 문자로 표시될 수 있다.

2. **URL 인코딩과 디코딩**:  
   URL 인코딩된 값은 서버에서 처리되기 전에 다시 디코딩되어야 한다. 서버는 수신한 URL을 디코딩하여 원래의 한글 문자열로 복구한다. Java에서는 `URLDecoder.decode`를 사용하여 인코딩된 URL을 원래의 UTF-8 문자열로 변환할 수 있다.

```java
import java.net.URLDecoder;

public class URLDecoderExample {
    public static void main(String[] args) throws Exception {
        String encodedString = "%ED%95%9C%EA%B8%80";
        String decodedString = URLDecoder.decode(encodedString, "UTF-8");
        System.out.println(decodedString);  // 결과: 한글
    }
}
```

## ISO 9941이란?

ISO 9941은 국제 표준 문자 인코딩 체계로, 아스키 외의 다른 문자 세트를 지원하기 위해 설계된 표준이다. 주로 유럽 및 국제 표준에서 사용된다. UTF-8과는 별개로 특정 용도에 맞춰 개발된 인코딩 체계다.

## HTTP 통신에서 문자열 인코딩

HTTP 통신 시, 서버와 클라이언트 간의 문자 데이터 전송은 항상 인코딩을 고려해야 한다. 특히 헤더나 바디에서 문자가 깨지지 않게 하기 위해 Content-Type 헤더에 인코딩 방식을 명시한다.

### WireShark로 패킷 분석

WireShark와 같은 네트워크 분석 도구를 사용하여 HTTP 통신 시 인코딩된 문자열을 패킷 레벨에서 확인할 수 있다. 이를 통해 실제로 전송되는 데이터가 어떤 방식으로 인코딩되어 전송되는지 파악할 수 있다.

```plaintext
GET /search?q=%ED%95%9C%EA%B8%80 HTTP/1.1
Host: example.com
```

위와 같이 URL이 UTF-8로 인코딩되어 전송되며, 패킷 내부에서 퍼센트 인코딩 방식으로 문자열을 확인할 수 있다.
```