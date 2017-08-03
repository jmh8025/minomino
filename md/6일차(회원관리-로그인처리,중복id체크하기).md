6일차(회원관리-로그인처리,중복id체크하기)
=========================================

### 자바빈즈의 3가지 종류

#### 1.DB Connection빈 : DB연결관리(연결 및 해제)

-	**DBConnectionMgr.Java** 를 응용하자

(1) B에 관련된 설정정보만 변경 시켜둔다.

```java
private String _driver = "oracle.jdbc.driver.OracleDriver",
    _url = "jdbc:oracle:thin:@localhost:1521:orcl",
    _user = "scott",
    _password = "tiger";

```

(2) DBConnectionMgr클래스의 객체를 생성 **getConnection() 필요**

```java
//커넥션풀객체를 선언
    private static DBConnectionMgr instance = null;

    public DBConnectionMgr() {
    }

    /** Use this method to set the maximum number of open connections before
     unused connections are closed.
     */

    //커넥션풀을 얻어오는 정적메소드
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

(3) freeConnection()을 이용해서 ->DB메모리 해제구문

```java
public void freeConnection(Connection c, PreparedStatement p, ResultSet r) {
        try {
            if (r != null) r.close();
            if (p != null) p.close();
            freeConnection(c);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
```

#### 2. DTO(데이터 저장빈) : Setter,Getter 메소드 구성(테이블의 필드와 연관)

-	테이블을 설계(20개이상)하여 필드별로 저장하고 조회함

(1) **Member** 테이블 생성

```sql
create table member(
id varchar2(20) primary key, /*회원id*/
passwd varchar2(20) not null,/*암호*/
name   varchar2(20) not null,/*회원명*/
e_mail varchar2(20) ,/*이메일*/
phone varchar2(30)  not null,/*전화번호*/
zipcode varchar2(10) ,/*우편번호*/
address varchar2(60) not null,/*주소*/
job  varchar2(30)/*직업*/
);

insert into member values('nup','1234','홍길동',
       'test@daum.net','02-123-0987','132-098',
       '서울시 강남구 대현빌딩 3층','프로그래머');

commit;
```

(2) 자바빈즈 **MemberDTO.java** 생성

```java
private String mem_id;
    private String mem_passwd;
    private String mem_name;
    private String mem_email;
    private String mem_phone;
    private String mem_zipcode;
    private String mem_address;
    private String mem_job;

  이하 getter setter 구문

```

#### 3. DAO(Data Access Object) : 자바빈즈의 메소드 선언(메소드를 호출)

-	DAO(Data Access Object) => DB접속 후 => 메소드 선언

-	insert,update,delete,select(join) 웹상으로 호출

```java
package hewon;

import java.util.*;
import java.sql.*;

//DB를 연결 시킨후에 웹상에 호출되는 메소드를 선언하는 클래스

//즉 , DBConnectionMgr와 MamberDAO를 서로 연결후에 DAO에서 DB가 연결되어있으면 메소드 선언

public class MemberDAO {
	// 1. DBConnetctionMgr객체를 선언
	private DBConnectionMgr pool = null;

	// 2. 생성자를 통해서 객체를 얻어온다. -> 서비스
	public MemberDAO() {// 어차피 MemberDAO호출시 바로 나오는것 (즉, 생성자에다가 DB객체를 넣어주는게 이상적)
		try {
			pool = DBConnectionMgr.getInstance();
			System.out.println("pool=>" + pool);
		} catch (Exception e) {
			System.out.println("DB연결 실패 : " + e);
		}
	}// 생성자

	// 1)요구분석에 따른 회원 로그인을 체크인 해주는 메소드 필요
	// int,boolean(true,false)
	public boolean loginCheck(String id, String passwd) {
		// ㄱ. DB연결코딩
		Connection con = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		boolean check = false;
		String sql = "";

		// ㄴ. 실행시킬 SQL구문이 필요
		try {
			con = pool.getConnection();
			System.out.println("con=>" + con);
			sql = "select id,passwd from member where id=? and passwd=?";
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, id);
			pstmt.setString(2, passwd);
			rs = pstmt.executeQuery();
			check = rs.next();
		} catch (Exception e) {
			System.out.println("loginCheck()에러" + e);
		} finally {// ㄷ.DB연결해제
			pool.freeConnection(con, pstmt, rs);
		}
		return check;
	}

	// 2)회원가입 : 중복 id 체크해주는 메소드 필요
	//select id from member where id='kkk';
		public boolean checkId(String id) {

			Connection  con=null;
			PreparedStatement pstmt=null;
			ResultSet rs=null;
			boolean check=false;
			String sql="";//실행시킬 sql구문
			//2.실행시킬 SQL구문이 필요
			try {
				con=pool.getConnection();
				sql="select id from member where id=?";
				pstmt=con.prepareStatement(sql);
				pstmt.setString(1, id);
				rs=pstmt.executeQuery();
				check=rs.next();//데이터가 존재->true ,없으면 ->false
			}catch(Exception e) {
				System.out.println("checkId()실행 에러유발->"+e);
			}finally {//3.DB연결해제
				pool.freeConnection(con, pstmt, rs);
			}
			return check;
		}

// 3)우편번호 : 우편번호를 검색(자동으로 입력)
//select zipcode from zipcode where area3 like '%수유3동%' -----------String
//select zipcode,area1 from zipcode where area3 like '%수유3동%'------
		public  Vector   zipcodeRead(String area3) {

			Connection  con=null;
			PreparedStatement pstmt=null;
			ResultSet rs=null;//select
			//추가
			Vector vecList=new Vector();
			String sql="";//실행시킬 sql구문
			//2.실행시킬 SQL구문이 필요
			try {
				con=pool.getConnection();
				sql="select * from zipcode where area3 like '"+area3+"%'";
				pstmt=con.prepareStatement(sql);
				rs=pstmt.executeQuery();
				//레코드가 한개 이상
				while(rs.next()) {
					//vector or ZipcodeDTO(필드별로 저장(Setter)->rs.getXXX(필드명)
					ZipcodeDTO tempZipcode=new ZipcodeDTO();
					tempZipcode.setZipcode(rs.getString("zipcode"));//("142-890");
					tempZipcode.setArea1(rs.getString("area1"));
					tempZipcode.setArea2(rs.getString("area2"));
					tempZipcode.setArea3(rs.getString("area3"));
					tempZipcode.setArea4(rs.getString("area4"));
					//vector or ArrayList에 담는 구문
					vecList.add(tempZipcode);//대략 13개의 레코드가 저장이 된다.
				}
			}catch(Exception e) {
				System.out.println("zipcodeRead() 실행 에러유발->"+e);
			}finally {//3.DB연결해제
				pool.freeConnection(con, pstmt, rs);
			}
			return vecList;
		}

// 4)회원가입

// 5)회원수정

// 6)회원탈퇴
}
```

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
