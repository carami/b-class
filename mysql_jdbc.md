1. mysql 에 데이터베이스와 사용자를 추가한다.

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

2. JDBC Connection Test

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


```



접속이 안될 경우 아래와 유사한 오류 결과가 출력된다.
```
/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/bin/java -ea -Didea.test.cyclic.buffer.size=1048576 "-javaagent:/Applications/IntelliJ IDEA.app/Contents/lib/idea_rt.jar=53382:/Applications/IntelliJ IDEA.app/Contents/bin" -Dfile.encoding=UTF-8 -classpath "/Applications/IntelliJ IDEA.app/Contents/lib/idea_rt.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/junit/lib/junit-rt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/charsets.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/deploy.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/cldrdata.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/dnsns.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/jaccess.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/jfxrt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/localedata.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/nashorn.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/sunec.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/sunjce_provider.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/sunpkcs11.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/zipfs.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/javaws.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/jce.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/jfr.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/jfxswt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/jsse.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/management-agent.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/plugin.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/resources.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/rt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/ant-javafx.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/dt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/javafx-mx.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/jconsole.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/packager.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/sa-jdi.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/tools.jar:/DEVEL/eclipse_workspace/todo/target/test-classes:/DEVEL/eclipse_workspace/todo/target/classes:/Users/urstory/.m2/repository/org/springframework/spring-core/4.3.5.RELEASE/spring-core-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-context/4.3.5.RELEASE/spring-context-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-expression/4.3.5.RELEASE/spring-expression-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-beans/4.3.5.RELEASE/spring-beans-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-aop/4.3.5.RELEASE/spring-aop-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-context-support/4.3.5.RELEASE/spring-context-support-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-jdbc/4.3.5.RELEASE/spring-jdbc-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-tx/4.3.5.RELEASE/spring-tx-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-web/4.3.5.RELEASE/spring-web-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-webmvc/4.3.5.RELEASE/spring-webmvc-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/javax/servlet/jstl/1.2/jstl-1.2.jar:/Users/urstory/.m2/repository/javax/servlet/javax.servlet-api/3.1.0/javax.servlet-api-3.1.0.jar:/Users/urstory/.m2/repository/mysql/mysql-connector-java/5.1.41/mysql-connector-java-5.1.41.jar:/Users/urstory/.m2/repository/org/apache/commons/commons-dbcp2/2.1.1/commons-dbcp2-2.1.1.jar:/Users/urstory/.m2/repository/org/apache/commons/commons-pool2/2.4.2/commons-pool2-2.4.2.jar:/Users/urstory/.m2/repository/commons-logging/commons-logging/1.2/commons-logging-1.2.jar:/Users/urstory/.m2/repository/org/springframework/spring-test/4.3.5.RELEASE/spring-test-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/junit/junit/4.12/junit-4.12.jar:/Users/urstory/.m2/repository/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar:/Users/urstory/.m2/repository/org/assertj/assertj-core/3.6.2/assertj-core-3.6.2.jar" com.intellij.rt.execution.junit.JUnitStarter -ideVersion5 carami.jdbc.JdbcTest,connectionTest
objc[24446]: Class JavaLaunchHelper is implemented in both /Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/bin/java (0x10e9824c0) and /Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/libinstrument.dylib (0x10ea724e0). One of the two will be used. Which one is undefined.
Sat Jul 01 13:31:54 KST 2017 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.

java.sql.SQLException: Access denied for user 'carami'@'localhost' (using password: YES)

	at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:964)
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3973)
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3909)
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:873)
	at com.mysql.jdbc.MysqlIO.proceedHandshakeWithPluggableAuthentication(MysqlIO.java:1710)
	at com.mysql.jdbc.MysqlIO.doHandshake(MysqlIO.java:1226)
	at com.mysql.jdbc.ConnectionImpl.coreConnect(ConnectionImpl.java:2205)
	at com.mysql.jdbc.ConnectionImpl.connectOneTryOnly(ConnectionImpl.java:2236)
	at com.mysql.jdbc.ConnectionImpl.createNewIO(ConnectionImpl.java:2035)
	at com.mysql.jdbc.ConnectionImpl.<init>(ConnectionImpl.java:790)
	at com.mysql.jdbc.JDBC4Connection.<init>(JDBC4Connection.java:47)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
	at com.mysql.jdbc.Util.handleNewInstance(Util.java:425)
	at com.mysql.jdbc.ConnectionImpl.getInstance(ConnectionImpl.java:400)
	at com.mysql.jdbc.NonRegisteringDriver.connect(NonRegisteringDriver.java:330)
	at java.sql.DriverManager.getConnection(DriverManager.java:664)
	at java.sql.DriverManager.getConnection(DriverManager.java:247)
	at carami.jdbc.JdbcTest.connectionTest(JdbcTest.java:15)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.junit.runners.model.FrameworkMethod$1.runReflectiveCall(FrameworkMethod.java:50)
	at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:12)
	at org.junit.runners.model.FrameworkMethod.invokeExplosively(FrameworkMethod.java:47)
	at org.junit.internal.runners.statements.InvokeMethod.evaluate(InvokeMethod.java:17)
	at org.junit.runners.ParentRunner.runLeaf(ParentRunner.java:325)
	at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:78)
	at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:57)
	at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
	at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
	at org.junit.runner.JUnitCore.run(JUnitCore.java:137)
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:68)
	at com.intellij.rt.execution.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:51)
	at com.intellij.rt.execution.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:242)
	at com.intellij.rt.execution.junit.JUnitStarter.main(JUnitStarter.java:70)
```

성공했을 때는 다음과 같은 결과가 화면에 출력된다.
```
/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/bin/java -ea -Didea.test.cyclic.buffer.size=1048576 "-javaagent:/Applications/IntelliJ IDEA.app/Contents/lib/idea_rt.jar=53486:/Applications/IntelliJ IDEA.app/Contents/bin" -Dfile.encoding=UTF-8 -classpath "/Applications/IntelliJ IDEA.app/Contents/lib/idea_rt.jar:/Applications/IntelliJ IDEA.app/Contents/plugins/junit/lib/junit-rt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/charsets.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/deploy.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/cldrdata.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/dnsns.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/jaccess.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/jfxrt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/localedata.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/nashorn.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/sunec.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/sunjce_provider.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/sunpkcs11.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/ext/zipfs.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/javaws.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/jce.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/jfr.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/jfxswt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/jsse.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/management-agent.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/plugin.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/resources.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/rt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/ant-javafx.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/dt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/javafx-mx.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/jconsole.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/packager.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/sa-jdi.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/lib/tools.jar:/DEVEL/eclipse_workspace/todo/target/test-classes:/DEVEL/eclipse_workspace/todo/target/classes:/Users/urstory/.m2/repository/org/springframework/spring-core/4.3.5.RELEASE/spring-core-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-context/4.3.5.RELEASE/spring-context-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-expression/4.3.5.RELEASE/spring-expression-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-beans/4.3.5.RELEASE/spring-beans-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-aop/4.3.5.RELEASE/spring-aop-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-context-support/4.3.5.RELEASE/spring-context-support-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-jdbc/4.3.5.RELEASE/spring-jdbc-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-tx/4.3.5.RELEASE/spring-tx-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-web/4.3.5.RELEASE/spring-web-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/org/springframework/spring-webmvc/4.3.5.RELEASE/spring-webmvc-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/javax/servlet/jstl/1.2/jstl-1.2.jar:/Users/urstory/.m2/repository/javax/servlet/javax.servlet-api/3.1.0/javax.servlet-api-3.1.0.jar:/Users/urstory/.m2/repository/mysql/mysql-connector-java/5.1.41/mysql-connector-java-5.1.41.jar:/Users/urstory/.m2/repository/org/apache/commons/commons-dbcp2/2.1.1/commons-dbcp2-2.1.1.jar:/Users/urstory/.m2/repository/org/apache/commons/commons-pool2/2.4.2/commons-pool2-2.4.2.jar:/Users/urstory/.m2/repository/commons-logging/commons-logging/1.2/commons-logging-1.2.jar:/Users/urstory/.m2/repository/org/springframework/spring-test/4.3.5.RELEASE/spring-test-4.3.5.RELEASE.jar:/Users/urstory/.m2/repository/junit/junit/4.12/junit-4.12.jar:/Users/urstory/.m2/repository/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar:/Users/urstory/.m2/repository/org/assertj/assertj-core/3.6.2/assertj-core-3.6.2.jar" com.intellij.rt.execution.junit.JUnitStarter -ideVersion5 carami.jdbc.JdbcTest,connectionTest
objc[24561]: Class JavaLaunchHelper is implemented in both /Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/bin/java (0x1008784c0) and /Library/Java/JavaVirtualMachines/jdk1.8.0_131.jdk/Contents/Home/jre/lib/libinstrument.dylib (0x1009684e0). One of the two will be used. Which one is undefined.
Sat Jul 01 13:37:59 KST 2017 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.

Process finished with exit code 0
```
