13일차(jstl문법 마무리 및 모델2의 개요및 작성법)
================================================

\\192.168.0.57\webtest\final 프로젝트자료\2.프로젝트자료2(오라클 모델링자료 및 스토리보드샘플예제)\정규화 예제

forEach.jsp
-----------

자바에서의 for문

```java
int sum=0;
for(int i=1;i<=100;i++){
	sum +=i;
}
```

jstl에서의 forEach문

```java
<c:forEach var="i" begin="1" end="100" step="2">
<c:set var="sum" value="${sum+i}" />
</c:forEach>
```

---

import.jsp
----------

-	다른 웹 리소스의 컨텐츠를 JSP 페이지에 삽입하는 것으로 jsp:include 강력한 버전이라고 부른다.
-	import 되어야하는 콘텐츠용 url은 url 속성을 통해 지정받는다. java.net.URL 클래스로 지칭되는 모든 프로토콜의 url 속성용으로 값으로 사용된다.

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
 <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>외부의 자원을 가져다주는 액션태그</title>
</head>
<body>
<%
//String url="http://www.naver.com"
%>

<c:set var="url" value="http://daum.net/" />
<!-- url 속성 --> 접속할 사이트 주소 u는 접속한 사이트의 정보(변수)-->
<c:import url="${url}" var="u"/>
<c:out value="${url}" /> 가져옵니다.
<hr>
<base href="<c:out value="${url}" />">
<c:out value="${u}"  escapeXml="false"/>
</base>
<h4>내부자원을 가져오기</h4>
<c:set var="url" value="chooseTag.jsp" />
<c:import url="${url}" var="u">
<c:param name="name" value="bk" />
</c:import>
<hr>
<c:out value="${u}"/>
</body>
</html>
```

### base

```java
<base href="<c:out value="${url}" />">
사이트주소
</base>
```

-	예를 들어 naver.com/logo.jpg 라는 파일을 네이버가 /logo.jpg로 설정했을수도 있음으로 base로 절대경로를 지정하는 것

---

<br><br><br>

fmt.jsp
=======

> 데이터의 숫자와 날짜를 포맷하는 액션을 정의한다. 국제화 된 리소스 번들을 사용하여 JSP 페이지의 국제화도 지원한다.

![fmt](/assets/fmt.jpg)

<br>

### fmt라이브러리의 커스텀태그는 총 4가지로 구성된다.

1.	로컬화 컨텐츠를 설정할 수 있는 태그
2.	데이터 태그
3.	Number 태그
4.	Message 태그

### 1. 로컬화 컨텐츠를 설정할 수 있는 태그

-	데이터를 포맷팅할 때 JSTL 태그에 의해 사용되는 로케일은 일반적으로 각 HTTP 요청의 일부로서 사용자의 브라우저에 의해 보내진 Accept-Language 헤더 검사하여 결정된다. 어떤 헤더도 존재하지 않으면 , JSP은 기본 로케일을 설정할 수 있는 JSP 설정 변수를 제공한다. 이러한 설정 변수들이 설정되지 않았다면, JVM은 기본 로케일이 사용되는데, 이것은 JSP 컨테이너가 실행되는 OS에서 얻을 수 있다.

```java
 <fmt:setLocale value="expression"  scope="scope" variant="expression"/>
```

㉠ value 애트리뷰트

-	반드시 필요하다.

-	이 애트리뷰트의 값은 로케일의 이름을 짓는 스트링(ko,ja,en) 혹은 java.util.Locale 클래스의 인스턴스가 되어야 한다.

-	fr(프랑스어), ca(캐나다)..... 등

㉡ scope 애트리뷰트

-	로케일의 범위를 지정하는데 사용되는데 애트리뷰트다.

-	page 범위는 이 설정이 현재 페이지에만 적용된다.

-	request 범위는 이것을 요청 동안 액세스 된 모든 JSP 페이지에 적용된다.

-	session 범위는 사용자 세션 과정 동안 액세스 된 모든 JSP 페이지에 적용된다.

-	application 범위는 웹 어플리케이션의 JSP 페이지와 그 어플리케이션의 모든 사용자에 대한 요청에 적용된다.

㉢ variant 애트리뷰트

-	특정 웹 브라우저 플랫폼이나 벤더들로 로케일을 커스터마이징 할 수 있다.

-	예) MAC과 WIN은 각각 Apple Macintosh와 Microsoft Windows 플랫폼에 대한 이름들이 이다.

##### setLocale 태그와 같은 액션으로 다른 fmt 커스텀 태그들에 의해 사용될 기본 시간(time zone) 값을 설정하는데 사용 되는 태그

```java
<fmt:setTimeZone value="expression" var="name" scope="scope"/>
```

㉠ value 애트리뷰트

-	value 애트리뷰트는 반드시 필요하다. 시간대의 이름 도는 java.util.TimeZone클래스의 인스턴스가 되어야 한다.

-	시간대(time zone)의 네이밍에 대한 표준 없다. <fmt:setTimezone) 태그의 value 애트리뷰트에 사용하는 시간대 이름들은 자바 플랫폼을 따른다. java.util.TimeZone 클래스의 getAvailableIDs() 정적 메소드를 호출함으로써 ,유효한 시간대 이름들을 가져올 수 있다.

> 예) US/Eastern, GMT+8, Pacific/Guam

㉡ scope 애트리뷰트

-	setLocale 태그 같음

㉢ variant 애트리뷰트

-	setLocale 태그 같음

##### 범위가 지정된 변수에 TimeZone 인스턴스의 값을 지정할 수 있는 태그

```java
<fmt:timeZone value="expression">
body content
</fmt:timeZone>
```

### 2. 데이터 태그

##### 날짜와 시간을 포맷 및 디스플레이 하는데 사용 태그

```java
<fmt:formatDate value="expression" timeZone="expression" type="field" dateStyle="style" timeStyle="style" pattern="expression" var="name" scope="scope"/>
```

![date](/assets/date.jpg)

##### 날짜와 시간을 포맷 및 디스플레이 하는데 사용 태그

```java
<fmt:parseDate  type="field" dateStyle="style" timeStyle="style" pattern="expression"  timeZone="expression" parseLocale="expression" var="name" scope="scope">
body content
</fmt:parseDate>
```

![time](/assets/time.jpg)

### 3.Number 태그

특정 로케일 방식으로 통화나 퍼센트를 포함한 숫자 데이터를 디스플레이 하는데 사용되는 태그 정수와 소수점을 구분하는데 마침표나 콤파를 사용할 것인지의 여부를 로케일에서 결정한다.

![number_01](/assets/number_01.jpg)

##### 바디 콘텐트를 통해서 제공된 숫자 값을 값을 파싱하고, 그결과를java.lang.Number 클래스의 인스턴스로서 리턴하는 태그

```java
<fmt:parseNumber type="type" pattern="expression" parseLocale="expression" integerOnly="expression" var="name" scope="scope">
body content
</fmt:parseNumber>
```

![number_02](/assets/number_02.jpg)

### 4. Message 태그

지역 고유의 리소스 번들에서 텍스트 메시지들을 가져다가 이를 JSP 페이지에 디스플레이 한다.

이 액션은 java.text.MessageFormat 클래스에서 제공하는 기능을 활용하기 때문에, 매개변수화 된 값들은 이 같은 텍스 메시지로 대체되어 로컬화 콘텐트를 동적으로 커스터마이징 하는 태그

```
<fmt:setBundle basename="expression" var="name" scope="scope"/>
```

<br>
----

<br>

MVC 구조(모델2) & 스프링(MVC구조 모양으로 되어있다.)
====================================================

### MVC가 뭔데?

| MVC | 내용                                                                                                                                 |
|-----|--------------------------------------------------------------------------------------------------------------------------------------|
| M   | 데이터 저장부분                                                                                                                      |
| V   | 처리결과를 받아서 출력만 담당<br>1)요청을 받는 부분 : Controller에게 전담<br>2)뷰에서 존재하는 자바코드 : 요청명령어 클래스에서 처리 |
| C   | 요청을 받아서 그요청에 맞는 요청명령어 클래스를 선택하여 처리                                                                        |

<br>

#### MVC구조로 되어있는 모델2를 알아보기전에 모델1과 모델2의 차이점을 알아보자!

| 분류  | 장점                                                                                     | 단점                                                                                                       |
|-------|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| 모델1 | 중소규모의 사이트 작성에 적합<br>적은인원으로도 구성이 가능                              | 백엔드와 프론트엔드가 합쳐져서 구별하기가 힘들어진다.즉, 유지보수가 어려워진다.<br>중복된 코드가 많아진다. |
| 모델2 | 대규모 사이트에 작성하는데 적합한 구조<br>역할이 분담이 나누어져 있어서 유지보수가 쉽다. | 서블릿이 중심이 되어 개개인의 실력이 향상을 요구한다.<br>구성원이 10인 이상을 요구한다.                    |

**모델2가 무적권 el,jstl을 사용한다는 것을 의미하지 않는다 단지 자주자주 쓰일뿐!!**

<br>

1.	요청명령어 : 컨트롤러에 요청명령어를 구분하는 소스코드를 계속해서 작성해야함

2.	컨트롤러를 하나만들면 계속해서 소스를 계속해서 만들지않아도됨**인터페이스를 이용**

---

<br>

<br>

JspBoard2
=========

### web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee"
 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
 id="WebApp_ID" version="3.1">
  <display-name>JspBoard</display-name>

 <!-- 컨트롤러 역할을 하는 서블릿의 이름,요청경로를 지정 -->
 <servlet>
 <servlet-name>서블릿클래스명 동일</servlet-name>
 <servlet-class>패키지명..실행시킬 서블릿클래스명</servlet-class>
 <init-param><!--매개변수에 따른 경로포함해서 불러올 파일명-->
 <param-name>propertyConfig</param-name>
 <param-value>C:/webtest/4.jsp/sou/JspBoard2/WebContent/WEB-INF/commeandPro.properties</param-value>
 </init-param>
 </servlet>

 <!-- 어떻게 요청이 들어왔을때 처리할 것인가 -->
 <servlet-mapping>
 <servlet-name>서블릿클래스명 동일</servlet-name>
 <url-pattern>요청명령어를 지정(*.do)</url-pattern>
 </servlet-mapping>

  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
</web-app>
```

commendAction.java(인터페이스)

```java
package action;

import javax.servlet.http.*;

//요청명령어에 따라서 처리해주는 모든 클래스의 공통메소드
public interface commendAction {
    //이동할 페이지의 경로와 페이지명이 필요->반환->ModelAndView(스프링)
public String requestPro(HttpServletRequest request, HttpServletResponse response) throws Throwable;

}
```

왜 인터페이스로 할까 ? 내일 설명해주심
