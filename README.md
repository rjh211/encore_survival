# encore_survival
🐣 신입병아리들의 살아남기
1. **Overload VS Override**

    : Overloading = 같은 이름의 메소드가 여러 개 가지면서, 매개변수의 유형과 개수가 다르도록 하는 것

    Overriding = 상위 클래스가 가지고 있는 메소드를 하위 클래스에서 재정의해서 사용한다.

2. **Process VS Thread**

    : 프로세스 (Process) = 메모리에 올라와 실행되고 있는 프로그램의 이스턴스

    스레드 (Thread)= 프로세스 내에서 실행되는 특정한 수행경로, 

    프로세스가 할당받은 자원을 이용하는 실행의 단위

- **Multi- Process VS Multi-Thread**
    1.  **Multi-Process** : 하나의 응용프로그램을 여러개의 프로세스가 처리
        - 장점

            : 자식 프로세스가 문제시 Kill 하는 것으로 문제해결

        - 단점

            : 프로세스 사이에서 공유하는 메모리가 없다. 그렇기 때문에 **Context-Switching** 발생시 모든 데이터를 리셋해야 한다.

            - **Context-Switching**

                : CPU에서 여러 프로세스를 돌아가면서 작업을 처리하는데 이 과정에서 캐쉬 메모리 초기화등 무거운 작업이 진행되면 Overhead가 발생한다.

    2. **Multi-Thread** : 하나의 응용프로그램을 여러개의 스래드로 구성해 하나의 작업을 처리한다.
        - 장점

            : 스래드는 프로세스 내 Stack 영역을 제외한 모든 메모리를 공유하기에 통신의 부담이 적다.

        - 단점

            : 하나의 스레드에 문제 발생시 전체 프로세스가 영향을 받는다.

        3. **Spring의 aop란?**

        :  애플리케이션 전체에 걸쳐 사용되는 기능을 재사용 하도록 지원하는 것

        ***공통처리를 위해 활용할 수 있는 것 3가지***

        AOP

        Filter

        Interceptor

        **AOP - 관점 지향 프로그래밍(크로스 컷팅 ; Cross Cutting)**

        : 공통된 기능 재사용

        그러니까 어플리케이션 저체에 흧어진 공통 기능이 하나의 장소에서 관리된다.

        ![image](https://user-images.githubusercontent.com/68880203/107168404-aa050f00-69fe-11eb-9d64-8c3513db8ab0.png)


        ![image](https://user-images.githubusercontent.com/68880203/107168426-b4bfa400-69fe-11eb-8d6e-25c3c15cf88c.png)


        - **타겟 (Target)**

            : 이전의 핵심기능 

            getBoards 혹은 getUsers를 하는 Service 들

        - **에스펙트 (Aspect)**

            : 객체지향 모듈을 오프젝트라 부르는것과 비슷하게 부가기능 모듈을 애스펙트라고 부르며, **핵심기능에 부가되어 의미를 갖는 특별한 모듈**

        - **어드바이스(Advice)**

            : 실질적으로 부가기능을 담은 구현체

            어드바이스의 경우 타겟 오프젝트에 종속되지 않기 때문에 순수하게 **부가기능에만 집중**할 수 있습니다.

            어드바이스는 애스펙트가 '무엇'을 '언제' 할지를 정의

        [추가 자료](https://jojoldu.tistory.com/71)

        4. Filter VS Interceptor

        : 

        ![image](https://user-images.githubusercontent.com/68880203/107168481-da4cad80-69fe-11eb-9cef-0acd7558cb02.png)


        - Interceptor와 Filter는 Servlet 단위에서 실행된다. <> 반면 AOP는 메소드 앞에 Proxy패턴의 형태로 실행된다.
        - 실행순서를 보면 Filter가 가장 밖에 있고 그안에 Interceptor, 그안에 AOP가 있는 형태이다.

        **Filter → Interceptor → AOP → Interceptor → Filter**

        1) **Filter**

        : 요청과 응답을 거른 뒤 정제하는 역할

        서블릿 필터는 DispatcherServlet 이전에 실행이 되는데 필터가 동작하도록 지정된 자원의 앞단에서 요청내용을 변경하거나,  여러가지 체크를 수행할 수 있다.

        또한 자원의 처리가 끝난 후 응답내용에 대해서도 변경하는 처리를 할 수가 있다.

        보통 web.xml에 등록하고, 일반적으로 인코딩 변환 처리, XSS방어 등의 요청에 대한 처리로 사용된다.

        EX)

        <!-- 한글 처리를 위한 인코딩 필터 -->

        <filter>

        <filter-name>encoding</filter-name>

        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>

        <init-param>

        <param-name>encoding</param-name>

        <param-value>UTF-8</param-value>

        </init-param>

        </filter>

        <filter-mapping>

        <filter-name>encoding</filter-name>

        <url-pattern>/*</url-pattern>

        </filter-mapping>

        해당 필터의 이름은 encoding, 값은 UTF-8인 파라미터를 정의하고 있다.

        필터의 URL-PATTERN을 /*로 정의하면 servlet, jsp뿐만 아니라 이미지와 같은 모든 자원의 요청에도 호출 된다.

        **[ 필터의 실행메서드 ]**

        ㆍinit() - 필터 인스턴스 초기화

        ㆍdoFilter() - 전/후 처리

        ㆍdestroy() - 필터 인스턴스 종료

        **2) Interceptor**

        : 요청에 대한 작업 전/후로 가로챈다

        스프링의 모든 빈 객체에 접근할 수 있다.

        인터셉터는 여러 개를 사용할 수 있고 로그인 체크, 권한체크, 프로그램 실행시간 계산작업 로그확인 등의 업무처리

        인터셉터의 실행메서드

        preHandler() - 컨트롤러 메서드가 실행되기 전

        postHanler() - 컨트롤러 메서드 실행직 후 view페이지 렌더링 되기 전

        afterCompletion() - view페이지가 렌더링 되고 난 후

        **3) AOP**

        :  Interceptor나 Filter와는 달리 메소드 전후의 지점에 자유롭게 설정이 가능하다.

         

        Interceptor와 Filter는 주소로 대상을 구분해서 걸러내야하는 반면, AOP는 주소, 파라미터, 어노테이션 등 다양한 방법으로 대상을 지정할 수 있다.

        AOP의 Advice와 HandlerInterceptor의 가장 큰 차이는 파라미터의 차이다.

        Advice의 경우 JoinPoint나 ProceedingJoinPoint 등을 활용해서 호출한다.

        반면 HandlerInterceptor는 Filter와 유사하게 HttpServletRequest, HttpServletResponse를 파라미터로 사용한다.

        AOP의 포인트컷

        @Before: 대상 메서드의 수행 전

        @After: 대상 메서드의 수행 후

        @After-returning: 대상 메서드의 정상적인 수행 후

        @After-throwing: 예외발생 후

        @Around: 대상 메서드의 수행 전/후

        5. mybatis의 Association VS Collection

        : [참고 링크](https://goodgid.github.io/Mybatis-Association-Collection-Part-1/)

        6.  param 값

        public class t1 {

        ```
        public static void main(String[] args) {
            // TODO Auto-generated method stub
            methodA();
        }
        public static void methodA() {
            Map<String, String> param = new HashMap<String, String>();
            param.put("ID", "guest");
            methodB(param);
            System.out.println(param);
            return;
        }
        public static void methodB(Map<String, String> map) {
            map.put("NAME", "게스트");
        		map.put("aaa", "bbb");
            return;
        }
        ```

        결과 : {aaa=bbb, ID=guest, NAME=게스트}

        7. 부서정보 조회

        Q) 상위 부서가 존재하는 계층형 구조를 조회하는 방법 설명

        **START WITH ... CONNECT BY 절을 사용한 계층형 쿼리**

        SELECT item_name, item_id, parent_id
        FROM bom
        **START WITH** parent_id IS NULL --루트노드를 지정,
        **CONNECT BY** **PRIOR** item_id = parent_id;--부모와 자식노드들간의 관계를 연결

        8. 결재 후 메일발송 요건을 처리하는 방법은?

        단, 메일발송의 실패와 상관없이 결재는 정상처리 되어야한다.

        결재기능은 이미 개발된 상태이며 결재기능의 수정없이 메일발송만 추가해야한다.

        9. 사용자의 권한체크를 위해 인터셉터를 활용하여 처리중입니다. 사용자의 권한이 없을 경우 false를 리턴합니다. 권한이 없는 경우 form의 서브밋을 호출한 경우와 ajax를 호출한 경우, 처리 방법은?

        10.  그리드 데이터를 한번에 멀티로 CRUD를 처리하기 위해 브라우저에서 서버로 보내는 방법과 서버에서 처리하는 방법은? 

        (예. 사용자 정보 : id, 이름, 이메일)

        **1. 문자열 json을 json object 또는 json array 로 변경해주어야함**

        **2. json array 및 json object에 대하여 쿼리를 가공하여 db처리를 해준다**

        - Reference ([https://roqkffhwk.tistory.com/104](https://roqkffhwk.tistory.com/104)) - ext.js

        11. 결재대상 A에 대해 승인자1이 해당 목록을 조회 중

        결재대상 A에 대해 승인자2가 해당 목록을 조회 중

        결재대상 A를 승인자1과 승인자2가 동시에 결재시도를 함

        승인자1은 승인을 승인자2는 반려이며 승인자1이 먼저 결재할 경우

        해결방법은?

        : 

        (제 의견) 결재 승인에 대하여 권한을 사용중인 승인자 1에게 락을 부여한다.

        이렇게 되면 승인자2는 결재 권한을 갖지 못함으로 동시에 두명의 승인자가 결재를 하는 문제를 해결할 수 있다.

        12. 주기적으로 DB를 백업하거나 특정시간 혹은 몇분 혹은 몇시간마다 동작하려 외부시스템에 접속하여 데이터를 가져오는 작업을 spring에서 처리하는 방법

        : **Spring Scheduler- crontab 사용**

        1) xml 추가 <beans> ++ task

        ```
        <?xml version="1.0" encoding="UTF-8"?>
        <beans
            xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:task="http://www.springframework.org/schema/task"
            xsi:schemaLocation="
                http://www.springframework.org/schema/beans
                http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
                http://www.springframework.org/schema/task
                http://www.springframework.org/schema/task/spring-task-3.0.xsd">
                
            <task:annotation-driven />
            
        </beans>
        ```

        2) scheduler

        스프링 task 네임스페이스에서 가장 강력한 기능은 스프링 애플리케이션 컨텍스트에서 스케줄링 되는 태스크를 설정하는 기능이다.  기본적으로 "ref" 속성은 스프링이 관리하는 어떤 객체라도 가리킬 수 있고 "method" 속성은 해당 객체에서 호출될 메서드명을 지정한다.

        ```
        <task:scheduled-tasks scheduler="myScheduler">
          <task:scheduled ref="someObject" method="someMethod" fixed-rate="5000"/>
          <task:scheduled ref="anotherObject" method="anotherMethod" cron="*/5 * * * * MON-FRI"/>
        </task:scheduled-tasks>

        <task:scheduler id="myScheduler" pool-size="10"/>

        <bean id="someObject" class="someMethod" />
        <bean id="anotherObject" class="anotherMethod" />
        ```

        예제에서 보듯이 외부 요소가 schedule를 참조하고 각 task는 트리거 메타데이터의 설정을 가지고 있다.  메타데이터는 고정된 지연시간을 가진 주기적인 트리거를 설정하거나(fixed-rate)  "cron"속성을 사용해서 더 세밀하게 제어할 수도 있다.

        [fixed-rate](https://www.notion.so/0195852b134d460c822936622651999e)

        - Reference ([https://jieun0113.tistory.com/115](https://jieun0113.tistory.com/115))
