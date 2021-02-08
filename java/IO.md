# I/O란?

Input, Output의 약자로 입출력이라고도 한다. 키보드를 통해 입력을 받거나 System.out.println()과같이 화면에 출력하는 것들이 대표적인 예

## Stream

자바에서 모든 입출력은 스트림을 통해 이루어 지는데 여기서의 스트림은 자바8의 [스트림](https://docs.oracle.com/javase/8/docs/api/?java/util/stream/Stream.html)과는 다른 개념이다.  
스트림은 단방향 통신만 가능하며 하나의 스트림으로 입력과 출력을 동시해 수행할 수 없다.  
스트림은 먼저 보낸 데이터를 먼저받게되는 FIFO(First In First Out)구조로 되어있다.

# NIO?

JDK1.4부터 지원하게된 NIO는 New Input Output의 약자로 채널과 버퍼를 사용하는 새로운 입출력을 지원하게 되었다.

## Buffer

내용 추가 필요

읽고 쓰기가 가능한 메모리 영역으로 데이터를 다른곳으로 전송하는동안 일시적으로 보관하기위해 사용한다.

# Channel

내용 추가 필요

# IO와 NIO 비교

기존 IO와 NIO의 차이점을 비교해보자

## 스트림기반 vs 채널기반

NIO는 채널 기반으로 스트림과 다르게 양방향으로 입출력이 가능하며 이로인해 입력과 출력을 위한 별도의 채널을 만들 필요가 없다.

## 넌버퍼 vs 버퍼

IO에서도 `BufferedInputStream, BufferedOutputStream`으로 버퍼를 사용할 수 있지만, NIO는 기본적으로 버퍼를 사용합니다.

## 블로킹 vs 논블로킹

IO는 스트림을 닫기전까지 스레드가 블로킹(대기)되지만, NIO는 블로킹과 논블로킹의 특성을 모두 가지고 있어 쓰레드를 인터럽트하여 빠져나갈 수 있습니다.

# InputStream과 OutputStream

내용 추가 필요

InputStream과 OutputStream은 모두 추상클래스로 읽고 쓰기위한 read(), write()를 포함하고 있으며 용도에 맞게 구현을 해서 사용할 수 있습니다.

# Byte와 Character 스트림

## 바이트 기반 스트림

FileInputStream, ByteArrayInputStream, PipedInputStream, AudioInputStream
FileOutputStream, ByteArrayOutputStream, PipedOutputStream, AudioOutputStream

## 문자 기반 스트림

FileReader, CharArrayReader, PipedReader, StringReader  
FileWriter, CharArrayWriter, PipedWriter, StringWriter

# 표준 스트림 (System.in, System.out, System.err)

System은 자바에서 미리 정의한 표준 입출력 클래스로 콘솔에서 데이터 입,출력을 위해 사용 할 수 있다.  
static final로 되어있어 System 인스턴스를 생성하지 않고 사용할 수 있다.

<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/run()%20start()%20%EC%B0%A8%EC%9D%B4.png?raw=true" width="80%">

<!-- TODO 사진 변경 예정 -->

- System.in: 콘솔에서 데이터를 입력할 때 사용
- System.out: 콘솔에 데이터를 출력할 때 사용
- System.error: 콘솔에 에러를 출력할 때 사용

간단한 사용법

```java
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {

        while (true) {
            int read = System.in.read();
            if (!(read > 0)) break;
            System.out.write(read);
        }
    }
}

```

실행결과  
<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/run()%20start()%20%EC%B0%A8%EC%9D%B4.png?raw=true" width="80%">

# 파일 읽고 쓰기

File 클래스..

# 참고 문서

Java의 정석 - 남궁 성  
http://www.tcpschool.com/java/intro
