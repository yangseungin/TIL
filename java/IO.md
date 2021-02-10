# I/O란?

Input, Output의 약자로 입출력이라고도 한다. 키보드를 통해 입력을 받거나 System.out.println()과같이 화면에 출력하는 것들이 대표적인 예

## Stream

자바에서 모든 입출력은 스트림을 통해 이루어 지는데 여기서의 스트림은 자바8의 [스트림](https://docs.oracle.com/javase/8/docs/api/?java/util/stream/Stream.html)과는 다른 개념이다.  
스트림은 단방향 통신만 가능하며 하나의 스트림으로 입력과 출력을 동시해 수행할 수 없다.  
스트림은 먼저 보낸 데이터를 먼저받게되는 FIFO(First In First Out)구조로 되어있다.

# NIO?

JDK1.4부터 지원하게된 NIO는 New Input Output의 약자로 채널과 버퍼를 사용하는 새로운 입출력을 지원하게 되었다.

## Buffer

읽고 쓰기가 가능한 메모리 영역으로 데이터를 다른곳으로 전송하는동안 일시적으로 보관하기위해 사용한다. 채널과 상호작용하여 채널이 읽고 쓸 때 항상 버퍼를 통해서 읽고 쓴다.

## Channel

IO에서 스트림으로 데이터를 읽고 쓴다면 NIO에서는 채널을 통해 데이터를 읽고 쓴다. 이 둘의 차이점은 스트림과 달리 읽고 쓰는것이 모두 가능하며 비동기적으로 사용할 수 있다.

# IO와 NIO 비교

기존 IO와 NIO의 차이점을 비교해보자

## 스트림기반 vs 채널기반

NIO는 채널 기반으로 스트림과 다르게 양방향으로 입출력이 가능하며 이로인해 입력과 출력을 위한 별도의 채널을 만들 필요가 없다.

## 넌버퍼 vs 버퍼

IO에서도 `BufferedInputStream, BufferedOutputStream`으로 버퍼를 사용할 수 있지만, NIO는 기본적으로 버퍼를 사용한다.

## 블로킹 vs 논블로킹

IO는 스트림을 닫기전까지 스레드가 블로킹(대기)되지만, NIO는 블로킹과 논블로킹의 특성을 모두 가지고 있어 쓰레드를 인터럽트하여 빠져나갈 수 있다.

# InputStream과 OutputStream

위에서 `스트림은 단방향으로만 통신할 수 있다`고 했는데 이로인해 스트림은 입력스트림과 출력스트림으로 나뉜다..  
자바에서는 java.io 패키지를 통해 InputStream과 OutputStream 추상 클래스를 제공하고 있고 읽고 쓰기위한 read(), write()를 포함하고 있으며 용도에 맞게 구현을 해서 사용할 수 있다.

# Byte와 Character 스트림

## 바이트 기반 스트림

자바에서 스트림은 기본적으로 바이트단위로 데이터를 전송하고있으며 입출력 대상에 따라 다양한 스트림을 제공하고 있다.
<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/%EB%B0%94%EC%9D%B4%ED%8A%B8%EA%B8%B0%EB%B0%98%EC%8A%A4%ED%8A%B8%EB%A6%BC.png?raw=true" width="80%">  
 출처: [TCPSCHOOL](http://tcpschool.com/java/java_io_stream)

java.io의 InputStream과 OutputStream을 상속받고있음을 확인할 수 있었다.  
그런데 위의 사진처럼 TCPSCHOOL과 참고하고있는 책에서도 AudioOutputStream이 있다고 되어있는데 테스트 해보려니 찾을 수 없었다.

FileOutputStream 사용예제

```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamSample {
    public static void main(String[] args) {
        File file = new File("/Users/yangseung-in/Desktop/test.txt");

        try (FileOutputStream fos = new FileOutputStream(file)) {
            int data;
            while ((data = System.in.read()) != 49) { //1입력시 종료
                fos.write(data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

실행 결과  
<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/FOS%EC%82%AC%EC%9A%A9%EC%98%88%EC%A0%9C.png?raw=true" width="80%">

## 문자 기반 스트림

자바에서 스트림이 기본적으로 바이트 단위를 전송하고 있는데 char의 경우 2바이트여서 1바이트씩 전송되는 바이트 기반 스트림으로는 처리가 힘든 경우가 있다.  
따라서 바이트 뿐 아니라 문자 기반의 스트림을 별도로 제공하고 있다.  
<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/%EB%AC%B8%EC%9E%90%EA%B8%B0%EB%B0%98%EC%8A%A4%ED%8A%B8%EB%A6%BC.png?raw=true" width="80%">  
 출처: [TCPSCHOOL](http://tcpschool.com/java/java_io_stream)

사용 예제

```java
import java.io.*;
public class FileReaderWriter {
   public static void main(String[] args) {
       File readFile = new File("/Users/yangseung-in/Desktop/test.txt");
       File writeFile = new File("/Users/yangseung-in/Desktop/writeFile.txt");
       try (FileReader fr = new FileReader(readFile);
            FileWriter fw = new FileWriter(writeFile);
            InputStreamReader isr = new InputStreamReader(System.in)) {

           int read;
           //읽은 파일을 새로운 파일에 쓰기
           while ((read = fr.read()) != -1) {
               fw.write(read);
           }
//            입력받은 내용 파일에 쓰기
           while ((read = isr.read()) != 49) { //1입력시 종료
               fw.write(read);
           }
           fw.flush();

       } catch (IOException e) {
           e.printStackTrace();
       }
   }
}

```

실행 결과

 <img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/filereaderwriter.png?raw=true" width="80%">

# 표준 스트림 (System.in, System.out, System.err)

System은 자바에서 미리 정의한 표준 입출력 클래스로 콘솔에서 데이터 입,출력을 위해 사용 할 수 있다.  
static final로 되어있어 System 인스턴스를 생성하지 않고 사용할 수 있다.

<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/system%ED%81%B4%EB%9E%98%EC%8A%A4.png?raw=true" width="80%">

<!-- TODO 사진 변경 예정 -->

- System.in: 콘솔에서 데이터를 입력할 때 사용
- System.out: 콘솔에 데이터를 출력할 때 사용
- System.error: 콘솔에 에러를 출력할 때 사용

사용 예제

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
<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/system%ED%81%B4%EB%9E%98%EC%8A%A4%20%EC%8B%A4%ED%96%89%EA%B2%B0%EA%B3%BC.png?raw=true" width="40%">

# 파일 읽고 쓰기

FileReader와 BufferedREader, FileWriter와 BufferedWriter로 간단한 예제를 작성해보았다.

```java
import java.io.*;

public class FileReaderWriter {
    public static void main(String[] args) {
        File readFile = new File("/Users/yangseung-in/Desktop/readFile.txt");
        File writeFile = new File("/Users/yangseung-in/Desktop/writeFile.txt");
        try (FileReader fr = new FileReader(readFile);
             BufferedReader reader = new BufferedReader(fr);
             FileWriter fw = new FileWriter(writeFile);
             BufferedWriter writer = new BufferedWriter(fw)) {

            String read = null;
            //읽은 파일을 새로운 파일에 쓰기
            while ((read = reader.readLine()) != null) {
                System.out.println(read);
                writer.write(read);
                writer.newLine();
            }
            //writeFile에 새로운 스트링 추가
            String newString = "Yang";
            writer.write(newString);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

실행결과  
<img src="https://github.com/yangseungin/TIL/blob/master/java/%EC%82%AC%EC%A7%84/%ED%8C%8C%EC%9D%BC%EC%9D%BD%EA%B3%A0%EC%93%B0%EA%B8%B0.png?raw=true" width="100%">

# 참고 문서

Java의 정석 - 남궁 성  
http://www.tcpschool.com/java/intro
