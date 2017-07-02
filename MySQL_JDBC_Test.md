#  Mysql 설치
- http://dev.mysql.com/downloads/mysql/ 에서 다운로드   
- http://withcoding.com/26 설치시 참고
- path C:\Program Files\MySQL\MySQL Server 5.7\bin 추가
  - cmd에서 실행 가능
  - mysql -u 사용자명  -p   

  - 사용자 생성
     - create user userid
     - create user userid@localhost identified by 'password'
     - create user 'userid'@'%' identified by 'password'
  - 사용자 제거
    - drop user 'userid';
    - delete from user where user ='userid'
  - 권한
    - GRANT ALL on DB명.* TO userid@'localhost'
    - GRANT ALL on DB명.* TO userid;
    - GRANT ALL on DB명.* TO userid@'xxx.xxx.xxx.%'
  - 데이터베이스 생성
    - create database DB명

  - 명령들
    - show tables;
    - show databases;
    - use DB명

  - 테이블 생성
    - create table member(
id varchar(10) primary key,
name varchar(32) not null,
password varchar(10) not null);

  - 데이터입력
    - insert into member values ('carami','강경미','carami');
  - 데이터조회
    - select * from member;
  - 데이터 수정
    - update member set password = '1234';
  - 데이터삭제
    - delete from member where id = 'test';

    ## mysql 에 데이터베이스와 사용자를 추가한다.

    mysql -uroot -proot mysql

    // tododb 데이터베이스 생성
    create database tododb;

    // id : carami , password : carami
    create user 'carami'@'%' identified by 'carami';

    // carami에게 tododb에 대한 모든 권한을 줌
    GRANT ALL on tododb.* TO carami@'%'

    // 위에서 설정한 내용이 바로 적용되도록 한다.
    FLUSH PRIVILEGES;

    외부에서 접근 가능하도록 방화벽에서 3306 포트를 열어놓는다.

# JDBC 프로그래밍
- DB접속 테스트
  ```
  public class ConnectionTest {
	   public static void main(String[] args) throws SQLException {
		     Connection connection = null;		
		     connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/caramidb", "carami", "carami");
		     if(connection != null){
			        System.out.println("success");
			        connection.close();
		     }else
			      System.out.println("fail");						
	   }
   }
  ```
  - mysql-connector-java-5.1.42-bin.jar 라이브러리 추가



- Junit을 이용한 Conection 테스트

    JDBC Connection Test

    src/test/java

    에

    carmi.jdbc 패키지를 생성한다. 해당 패키지에 다음과 같은 클래스를 작성한다.

    ```
    package carami.jdbc;


    import org.junit.Assert;
    import org.junit.Test;

    import java.sql.Connection;
    import java.sql.DriverManager;

    public class JdbcTest {

    	@Test
    	public void connectionTest() throws Exception{
    		Connection connection = null;
    		connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/tododb", "carami", "carami");
    		Assert.assertNotNull(connection);
    	}
    }

    - DB입력 테스트
      ```
      public class InsertTest {
  public static void main(String[] args) throws Exception {
    Connection connection = null;
    PreparedStatement ps = null;
    connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/caramidb", "carami", "carami");
    String sql = "insert into member values (?,?,?)";
    ps = connection.prepareStatement(sql);
    ps.setString(1, "testID");
    ps.setString(2, "테스트");
    ps.setString(3, "test");

    int resultCount = ps.executeUpdate();		
    System.out.println(resultCount);
  }
  }
      ```
