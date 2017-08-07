1일차
-----

```
**Jsp페이지의 구성요소**

 웹페이지를 요청(request)-------------------->
   링크문자열 클릭,버튼클릭,url
                         html문서<--------------------응답(response)

1.스크립트릿->웹상에서 자바코드를 사용할 수 있도록 해주는 영역
                  <% ~  %> =>태그사용X(자바스크립트 X)
                                         표현식도 사용불가

  http://localhost:8090/JspStudy/abc/imsi.jsp
                                               =
                                            WebContent

2.표현식(expression)->간단하게 출력문 대용으로 사용
                                변수값 출력,메서드호출->결과값을 출력

 형식) <%=변수명%> or <%=객체명.일반메서드명(~)%>
                                     <%=정적메서드명(~)%>

3.선언문(Declaration)->스크립트릿과 모양이 유사하다.

<%!  ~ %> =>자바코드를 사용이 가능(정적 멤버변수 선언)
                          ->선언된 위치에 상관없이 변수를 불러다 사용 가능
                          ->메서드 작성이 가능->정적메서드 느낌
                          ->사용이 가능한데 현실적으로 사용X

 <%!
      String name="홍길동";

      public String getName(){ ===>따로 클래스를 만들어서 사용
          return name;                  DTO(Data Transfer Object)
                                                      DAO(Data Access Object)
      }
%>

 jsp페이지 내부에서 따로 메서드를 선언해서 호출하는 구문을 잘 사용X

 디자이너(html5,css,js)+소스코드(메서드 작성)=>화면이 복잡

=>메서드부분-->따로 클래스로 만들어서 웹에서 불러오는 경우
                         자바빈즈클래스에서 작업을 한다.->액션태그
----------------------------------------------------------------------

4.주석(comment) or 지시어
    comment.jsp로 작성

 JSP 주석->태그와 태그사이에 적절히 배치

1.<!-- 눈에 보이는 주석 --> =>html주석
2.<%-- 눈에 안보이는 주석 --%>
3.자바주석-> //, /*  ~ */

```

2일차
-----

```
***
contentType="text/html; charset=UTF-8"
  ==>텍스트형태의 html문서로 만들어서 보내주되 한글이 결과에 포함이 되어있다면
          한글이 깨지지 않은상태로 보내주세요

pageEncoding="UTF-8"=>한글처리해주세요


*** import=>외부에서 패키지를 불러올때 사용

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    import="java.util.Date" %>
<%@ page import="java.io.*,java.util.*" %> =>속성값을 여러개 나열->,,,
<%@ page import="java.sql.*" %> =>세로로 여러개의 import속성을 쓸 수있다.


**서블릿의 조건**

1.import javax.servlet.* //서블릿의 클래스
   import javax.servlet.http.*;//웹상에서의 접속에 관련된 정보
   import java.io.*;//입출력

  =>c:\tomcat8.5\lib->servlet-api.jar파일에 저장

2.반드시 publi class로 작성해야 된다.=>누구나 접속이 가능하게 해주기위해
3.반드시 httpServlet클래스를 상속받아야 된다.

httpServlet-->사이트에 접속할때 마다 처리해주는 기능을 가지고 있다.
                            get-------------->do Get()메서드를 사용하기위해서
                            post-------------->doPost()
```

3일차
-----

```
********차이점
<input type="submit" value="표만들기">
  =>자동적으로 action="box_proc.jsp로 전송한다.

<input type="button" value="표만들기" onclick="함수명()">
  =>자동적으로 action="box_proc.jsp로 (전송 X)
  =>document.폼객체명.submit()을 꼭 써줘야 된다.

<a href="JavaScript:호출할 함수명(~)">표만들기</a>
=>자동적으로 action="box_proc.jsp로  (전송 X)
  =>document.폼객체명.submit()을 꼭 써줘야 된다.
*********


웹프로그래밍=>******페이지 이동시키는 방법(jsp)******* =>기술 면접
                  =>오라클->join,subQuery(실습)


1.response객체->

                                    ->http:// 반드시 줄것
 //response.sendRedirect("http://www.chosun.com");  외부의 사이트로 이동
     response.sendRedirect("./req.jsp"); //상대경로를 지정(내부 파일이동)
    =>**URL창이 이동할 페이지로 전환이 되면서 이동이 된다.
    =>데이터를 서로 공유할 수없다.

2.forward액션태그==>모델2의 핵심
    =>외부의 사이트로 이동X
    =>자기 프로젝트의 다른 페이지로만 이동이 가능하다.(O)
    =>**URL창이 이동할 페이지로 전환이 X **
    결론) 데이터를 공유하면서 페이지를 이동시킬 수 가 있다.

회원로그인->예매페이지 1-->예매페이지 2--->예매페이지 3
                    지역                 상영관 시간        카드,무통장입금

 웹->페이지이동하면 전의 정보가 사라진다.=>세션처리

  A.jsp=>B.jsp=>C.jsp
 int su=100

3.자바스크립트->location.href="이동할 페이지명"
                        location.replace("http://~이동할 페이지명")
                        history.back()->전의 페이지로 이동
                        history.go(-1)->전의 페이지로 이동
                        history.forward()->앞의 페이지로 이동




*****
include액션태그를 사용(X) or (O)
include지시어의 차이점

공통점->둘다 외부의 파일의 내용을 불러와서 특정한 위치에 삽입한다.
차이점->include 액션태그=>동적으로 변경된 내용을 특정한 위치에 삽입
                                    =>angular or react (새로 고침 F5)

            include 지시어->정적으로 문자열만 복사->특정한 위치에 삽입

```

4일차
-----

```

 ********자바빈즈 3가지********

1.객체를 생성->useBean 태그

 //BeanTest bt=new BeanTest();
<jsp:useBean id="객체명"  class="패키지명.클래스명"  scope="사용범위" />
* page->현재 jsp내에서만(default)
* request->한페이지 이상
* session->로그인한 동안
* application->모든 페이지

<jsp:useBean id="bt" class="test.BeanTest" scope="page" />

2. ->Setter 호출

형식) <jsp:setProperty name="객체명" property="멤버변수명(str)" value="저장할값" />
  <jsp:setProperty name="bt" property="str"  value="<%=변수명%>" />

3.Getter Method를 대신 사용할 수 있는 태그
<%=bt.getStr() %>
형식)
<jsp:getProperty name="객체명 " property="멤버변수명" />
 <jsp:getProperty name="bt " property="str" />
```

5일차
-----

```
**
쿠키와 세션의 공통점->둘다 클라이언트
서버와의 연결을 일정시간동안
유지시켜주는 방법(로그인)

쿠키->파일로 저장->해킹의 소지,개인정보 유출->잘 사용X
세션->서버의 메모리에 저장->30분(default)유지->30분후 자동 종료
**

******************************
1.Java Application의 DB연동방법(JDBC)

 1)접속 ojdbc6.jar->C:\jdk1.8\jre\lib\ext=>복사를 할것(전역)=>추가적인 작업

  ->이클립스에서 경로를 지정 ojdbc6.jar를 불러오기

  ->다른 경로에 ojdbc6.jar를 복사->classpath환경변수를 선언->그 경로지정
     (지역)

2.Web Application의 DB연동방법=>

       1)Tomcat8.5.x\lib\ 접속 드라이버를 복사
                                      ojdbc6.jar (전역)
       2)JspMember
                |
                 -WebContent
                         |
                          -WEB-INF
                                 |
                                   -lib->ojdbc6.jar (지역)
                                            my~.jar

     ->이클립스에서 경로를 지정 ojdbc6.jar를 불러오기
----------------------------------------------------
*******************************
```

6일차
-----

```
**매개변수를 전달하는 방법**

1.Login.jsp->LoginProc.jsp

2.자바스크립트함수--->특정 jsp로 매개변수를 전달하는 경우
------------> 프린트 했음
```

8일차
-----

```
*****매개변수를 전달하는 방법****(기술면접시)
*************************************

1.링크문자열을 이용해서 페이지 전환할때 사용
 <a href="DelCheckForm.jsp?mem_id=<%=mem_id%>">회원탈퇴</a>

2.Request객체 또는 Session객체를 이용한 데이터 공유방법
<a href="MemberUpdate.jsp">회원수정</a>
 =>String mem_id=(String)session.getAttribute("idKey")

3.<form>태그내부에 <input type="hidden"~>값을 전달하는 경우
   형식) <input type="hidden" name="전달할매개변수명" value="전달할값">
ex)<input type="hidden" name="mem_id" value="<%=mem_id%>" >

  <input type="hidden" name="mem_id" value="<%=mem_id%>" >
    대신에 action속성값에
     action="이동할페이지명?매개변수명=전달할값&매개변수명2=값2,,,
4.action="delePro.jsp?mem_id=<%=mem_id%>
```

9일차
-----

```

1.show databases;==>현재 어떤 데이터베이스 목록확인

mysql>show databases; //탐색기의 폴더처럼 생각

2.특정 DB에 접속->들어가고 싶다.
 use 전환할 DB명

mysql> use mysql;  //DB에는 테이블이 들어있다.
Database changed

3.DB에 들어가 있는 테이블을 보여주세요(목록)
mysql>show tables;

mysql> show tables;

4.desc user; //구조확인
5.select * from user;
---------------------------
mysql>exit->종료
---------------------------
SQLGate for Mysql->30일

유저명:root
패스워드:1234
서버:localhost
포트:3306------>3307
데이터베이스:mysql ->처음에는 존재하는 DB에 접속
**캐릭터셋:utf8
**유니코드 사용->반드시 체크할것->한글사용 가능
  연결테스트->접속성공
```

11일차
------

```
***다양한방법으로 매개변수를 전달하는 경우***
------------------------------------------------------------------------------------                 
action="updatePro.jsp?num=<%=num%>&pageNum=<%=pageNum%>"
---------------------------------------------------------------------------------
action="updatePro.jsp"

<input type="hidden" name="num" value="<%=num%>">
<input type="hidden" name="pageNum" value="<%=pageNum%>">
```
