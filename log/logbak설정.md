###### 작성일: 2018-07-30  
###### 목적: 현재 운영하고 있는 시스템의 필요 없는 로그를 줄이기 위해 logbak.xml 재설정. 
----------------------
참고문서 사이트 
* <http://pshcode.tistory.com/124>
* <https://thinkwarelab.wordpress.com/2016/11/18/java%EC%97%90%EC%84%9C-logback%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%A1%9C%EA%B9%85logging-%EC%82%AC%EC%9A%A9%EB%B2%95/> 
* <http://totoro3040.blogspot.com/2015/05/logbackxml.html>
--------------------- 
#### **logback이란**? 
logback은 log4j를 토대로 새롭게 만든 Logging 라이브러리이다. 

#### **logback 장점**
* log4j보다 약 10배 정도 빠르게 수행되도록 내부가 변경되었고, 메모리 효율성도 좋아졌다. 
* 설정 파일을 변경하였을 경우, 서버 재기동 없이 변경 내용이 자동으로 갱신된다. 
* 서버 중지 없이 I/O Faliure에 대한 복구를 지원한다. 
* RollingFileAppender를 사용할 경우 자동적으로 오래된 로그를 지워주며 Rolling 백업을 처리한다. 
추가적인 내용 [logback 홈페이지](http://logback.qos.ch/reasonsToSwitch.html)

#### **logback 과 SLF4J**  
slf4j란, Simple Logging Facede for Java 의 약자로 log4J의 개발자가 logback과 함께 개발한 logging facade 즉, 로깅에 대한 인터페이스 
모음이라고 볼 수 있다. logbak을 사용하기 위해선 slf4j를 함께 사용해야 한다.

#### **필요한 라이브러리** 
|  <center>라이브러리명</center> |  <center>설명</center> | 
|:--------:|:--------:|
|logback-core.jar | logback 코어. |
|logback-classic.jar | slf4j에서 logback을 호출할 수 있도록 처리. |
|slf4j-api.jar | slf4j 라이브러리 |
|jcl-over-slf4j.jar(선택) | apache commons 로깅 -> slf4j 전환. |
|log4j-over-slf4j.jar(선택) | log4j 로깅 -> slf4j 전환. |

#### **설정파일 위치**
설정 파일에서는 **로그패턴**, **기록위치**, **출력레벨** 등을 필요에 맞게 설정할 수 있는 파일이다.  
가장 먼저 설정파일을 두어야 하는 위피는 Classpath 이다. 만일 Classpath를 resource/와 같이 설정하였다면 resource/logback.xml과 같이 위치 
시키면 된다. 

적용 가능한 설정 파일 3개  
* 1. **logback.groovy**
* 2. **logback-test.xml**
* 3. **logback.xml**

위 세개의 파일은 순서대로 높은 우선순위를 가진다. 
즉, LogBack은 Classpath에서 위의 설정파일을 순서대로 검색해 적용하며 상위 우선순위의 설정파일을 찾으면 하위 설정파일은 검색하지 않는다. 만일 
3개의 파일이 모드 없다면 기존 설정을 따른다. 

**ex)** logback-test.xml 과 logback.xml 파일이 둘다 있는 경우 >> logback-test.xml의 설정을 적용한다. 

#### **설정파일의 주요 항목** 
1. **Level**  
로그에 설정할 수 있는 레벨은 총 5가지이다. 
1) ERROR
2) WARN
3) INFO
4) DEBUG
5) TRACE 
위의 순서대로 높은 레벨을 가지며 출력 레벨의 설정에 따라 **설정 레벨 이상의 로그**를 출력 한다.    
**ex)** 출력레벨: INFO -> DEBUG, TRACE 레벨의 로그는 출력되지 않음.   

2. **Appender**  
로그를 출력 할 위치, 출력 형식 등을 설정 할 수 있다.   
**Logback-Core** 모듈을 통해 사용할 수 있는 기본적 Appender는 3가지이다.  
* 1) ConsoleAppender -로그를 OutputStream에 write 하여, 최종적으로 콘솔에 출력되도록 한다. 
* 2) FileAppender - 로그 내용을 지정된 File에 기록한다.  
* 3) RollingFileAppender - FileAppender로 부터 상속받은 Appender로 날짜, 최대 용량 등을 설정하여 지정한 파일명 **패턴에 따라 로그가 다른 파일에 기록** 되도록 한다. 이를 이용해 대량의 로그를 효과적으로 기록 할 수 있다. 

Logback-Core의 기본 Appender 외에도 Logback-Classic 모듈의 다양한 Appender(SSLSocketAppender, SMTPAppender, DBAppender 등)을 사용하여 로그를 원격위치에 기록 할 수도 있다. 

Appender들의 하위 항목으로 출력 형식(Layout Pattern)을 지정하여 각 Appender마다 원하는 내용을 출력시킬 수 있습니다. 
ex) %logger(Logger 이름), %thread(현재 스레드명), %level(로그 레벨), %msg(로그메시지), %n(new line) 등 

3. **Logger**   
실제 로그 기능을 수행하는 객체로 출력할 로그를 Appender에 전달한다.각 Logger마다 Name을 부여하여 사용한다. 패키지 별로 log를 출력할 때도 사용 할 수 있다. 

각 Logger 마다 원하는 출력 레벨값을 설정할 수 있으며, 0개 이상의 Appender를 지정할 수 있다. 각 소스로부터 입력받은 로깅 메시지는 로그 레벨에 따라 Appender로 전달 된다. 

기본적으로 최상위 로거인 Root Logger를 설정해 주어야하며, 추가로 필요한 로거에 대해 String 또는 클래스명 형식으로 Logger Name을 추가하여 사용할 수 있습니다. 또한 Logger의 Name은 .문자를 구분자로 사용하여 계층적으로 활용 할 수 있습니다.


