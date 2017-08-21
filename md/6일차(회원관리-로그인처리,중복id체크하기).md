6일차(회원관리-로그인처리,중복id체크하기)
=========================================

### 자바빈즈의 3가지 종류

#### 1.DB Connection빈 : DB연결관리(연결 및 해제)

#### 2. DTO(데이터 저장빈) : Setter,Getter 메소드 구성(테이블의 필드와 연관)

#### 3. DAO(Data Access Object) : 자바빈즈의 메소드 선언(메소드를 호출)

-	DAO(Data Access Object) => DB접속 후 => 메소드 선언

-	insert,update,delete,select(join) 웹상으로 호출

---

### pool=DBConnectionMgr.getInstance();

메소드가 정적 메소드라서 MemberDAO에서 인스턴스를 생성하지 않고 아래처럼 사용한다.

```java
//커넥션을 10개 준비
  private int _openConnections = 10;
//커넥션풀객체를 선언
  private static DBConnectionMgr instance = null;

//
  public static DBConnectionMgr getInstance() {
          //커넥션풀이 생성이 안되어있다면
        if (instance == null) {
              synchronized (DBConnectionMgr.class) {
                  //생성이 안되어있다면
                if (instance == null) {
                    //객체생성
                      instance = new DBConnectionMgr();
                  }
              }
          }
          return instance;//호출한 클래스쪽으로 반환
      }
```

```java
pool=DBConnectionMgr.getInstance();
```

#### 커넥션풀

-	매번 부르는 객체들(디비접속같은 객체들)을 미리 생성을 해두어서 호출시간을 최소화 시키는 것

![cap](https://i.loli.net/2017/07/13/5966d39a1fd94.png)

---

### PreparedStatement Statement 차이

> http://devbox.tistory.com/entry/Comporison

| PreparedStatement                                     | Statement                   |
|-------------------------------------------------------|-----------------------------|
| 준비된 녀석임 즉, Statement생성시 sql구문을 실행함    | Query를 실행하여야 구문실행 |
| ?? 의 사용가능으로 변수를 알아보기 편하고 보안이 강함 | 걍 쏘쏘함                   |
| 결론은 Prepared를 사용하자                            | !!                          |

---

### 원래는

인증전의 화면디자인 : Login.jsp

인증후의 화면디자인 : LoginSuccess.jsp

으로 두개의 화면을 사용하였지만

```java
Login.jsp

if(mem_id!=null){ //인증된 사람이라면
   %>
   <b><%=mem_id %></b>님 환영합니다.<p>
     당신은 제한된 기능을 사용할 수가 있습니다.<p>
     <a href="Logout.jsp">로그아웃</a>
   <% }else {
     기존에 사용하던 구문 %>
```

으로 작성하면 기존의 Login.jsp 페이지에서 로그인후의 페이지까지 같이 구현이 가능하다 .<br><br><br>

나름 꿀팁들
-----------

#### document.location.href= "이동경로"

#### window.open();

```java
  url= "IdCheck.jsp?mem_id="+mem_id;
  window.open(url, "title", "width=300 height=500");
```

#### jsp파일에 script 포함

```java
<SCRIPT LANGUAGE="JavaScript" src="script.js">
```

#### javascript 에서 특정 필드 포커스 맞추기

```java
document.form_name.id.focus();
```

#### html 에서 javascript 창 닫기

```java
 onclick="self.close()"
```

#### Cf)

create 문은 자동 commit 입니다!

---
