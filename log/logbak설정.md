#######작성일: 2018-07-30  
#######목적: 현재 운영하고 있는 시스템의 필요 없는 로그를 줄이기 위해 logbak.xml 재설정. 

참고문서 사이트 
* <http://pshcode.tistory.com/124>
* <https://thinkwarelab.wordpress.com/2016/11/18/java%EC%97%90%EC%84%9C-logback%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%A1%9C%EA%B9%85logging-%EC%82%AC%EC%9A%A9%EB%B2%95/>
--------------------- 
**logback이란**?

logback은 log4j를 토대로 새롭게 만든 Logging 라이브러리이다. 

**logback 장점**
* log4j보다 약 10배 정도 빠르게 수행되도록 내부가 변경되었고, 메모리 효율성도 좋아졌다. 
* 설정 파일을 변경하였을 경우, 서버 재기동 없이 변경 내용이 자동으로 갱신된다. 
* 서버 중지 없이 I/O Faliure에 대한 복구를 지원한다. 
* RollingFileAppender를 사용할 경우 자동적으로 오래된 로그를 지워주며 Rolling 백업을 처리한다. 

추가적인 내용 [logback 홈페이지](http://logback.qos.ch/reasonsToSwitch.html)

**logback 과 slf4j** 

**SLF4J**란, Simple Logging Facede for Java 의 약자로 log4J의 개발자가 logback과 함께 개발한 logging facade 즉, 로깅에 대한 인터페이스 
모음이라고 볼 수 있다. logbak을 사용하기 위해선 slf4j를 함께 사용해야 한다.

#### **필요한 라이브러리** 
|  <center>라이브러리명</center> |  <center>설명</center> | 
|:--------|:--------:|
|** logback-core.jar** | <center>cell 1x2 </center> |
|**logback-classic.jar** | <center>cell 2x2 </center> |
|**slf4j-api.jar** | <center>cell 3x2 </center> |

