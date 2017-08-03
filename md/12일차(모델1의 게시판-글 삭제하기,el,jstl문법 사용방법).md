12일차(모델1의 게시판-글 삭제하기,el,jstl문법 사용방법)
=======================================================

jstltest
--------

> page 921

-	액션태그를 사용할수 있도록 라이브러리 형태로 제공해 주는 것

-	jstl태그는 기본적으로 제공되지 않는다. 즉 제이쿼리처럼 따로 불러서 사용해야 함

-http://tomcat.apache.org/download-taglibs.cgi

```
형식) ${표현식}  =>out.println(변수명); out.println(str);
                             <%=str %> =>${변수명}

   ex) <%=article.getNum()%>=>${article.num}
                                                    ${article['num']}
                                                    ${article["num"]}
           out.println(4+5); <%=4+5%>  ${4+5}

```

fmt : 날짜,시간,통화 부분

core : 변수선언 ,객체의 값을 불러오기, 제어문

request.getParameter("name")==> name $(param.)

10장 nvc
