
1. maven 설치

- IDE( eclipse , intelliJ 등)에 기본 내장되어 있을 수 있으나 터미널에서 독립적으로 실행할 수 있도록 설치를 한다.
- https://maven.apache.org/download.cgi 에서 다운로드
  - 이 글을 쓰는 시점에서는 가장 최신 버전인인 apache-maven-3.5.0-bin.tar.gz 를 다운로드 한다.
  - 특정 폴더에 압축을 해제한다. 필자는 /apps/apache-maven-3.3.9 로 압축을 해제하였다.
  - path에 /apps/apache-maven-3.3.9/bin 을 추가한다.
  - 터미널을 새롭게 열고 mvn -v 명령으로 명령이 실행되는지 확인한다.

2. 이클립스를 설치한 하 가장 먼저 해야할 일 (encoding 설정)
- 이클립스 -> 환경설정 -> encoding 으로 검색
workspace, CSS, HTML, JSP, XML 을 모두 utf-8 로 설정한다. ISO10646/Unicode(utf-8)

3. Spring MVC를 이용하여 Hello 출력하기

1) Eclipse를 실행한다.
2) new - project -> maven - maven project -> next 버튼 클릭
   -> Create a simple project 선택, use default workspace location 선택 -> next 버튼 클릭
   -> goal id : carami (보통 도메인 이름을 거꾸로 적음)
      Artifact id : todo (프로젝트 이름을 보통 소문자로 씀)
      packaging : war (웹 어플리케이션 프로젝트니깐) -> finish 버튼 클릭
3) 처음 프로젝트를 생성하면 프로젝트에 오류표시가 납니다.
   웹 어플리케이션인데 필요한 파일이 존재하지 않기 때문입니다.
   참고로 maven은 기본설정이 jdk 1.5 로 되어 있습니다.
   problems 부분에서 maven - update project를 하라는 메시지가 있다면, 이클립스에서 프로젝트를 선택 후에 우측버튼을 클릭하고 maven - udpate project를 선택합니다.
   maven 프로젝트의 설정파일은 pom.xml 입니다. 해당 파일을 다음과 같이 수정합니다.

Spring mvc 를 사용하기 위한 pom.xml

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>carami</groupId>
	<artifactId>todo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>war</packaging>

	<properties>
		<jdk-version>1.8</jdk-version>
		<source-encoding>UTF-8</source-encoding>
		<resource-encoding>UTF-8</resource-encoding>
		<deploy-path>deploy</deploy-path>

		<!-- spring framework -->
		<spring-framework.version>4.3.5.RELEASE</spring-framework.version>

		<logback.version>1.1.3</logback.version>
		<jcl.slf4j.version>1.7.12</jcl.slf4j.version>

	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring-framework.version}</version>
		</dependency>

		<!--Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
			<scope>provided</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.3</version>
				<configuration>
					<source>${jdk-version}</source>
					<target>${jdk-version}</target>
					<encoding>${source-encoding}</encoding>
					<useIncrementalCompilation>false</useIncrementalCompilation>
				</configuration>
			</plugin>

			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-clean-plugin</artifactId>
				<version>2.6.1</version>
			</plugin>

			<plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<version>2.2</version>
				<configuration>
					<port>8080</port>
					<path>/</path>
				</configuration>
			</plugin>

		</plugins>
	</build>

</project>
```

  src/main/webapp 폴더 아래에 WEB-INF폴더를 만들고, 그 안에 다음과 같은 web.xml 파일을 작성합니다.

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
         id="WebApp_ID" version="2.5">

    <display-name>spring-mvc</display-name>

    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>carami.todo.config.RootApplicationContextConfig</param-value>
    </context-param>
    <context-param>
        <param-name>contextClass</param-name>
        <param-value>
            org.springframework.web.context.support.AnnotationConfigWebApplicationContext
        </param-value>
    </context-param>

    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>carami.todo.config.ServletContextConfig</param-value>
        </init-param>
        <init-param>
            <param-name>contextClass</param-name>
            <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <filter>
        <filter-name>httpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>httpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>

```

src/main/java 아래에 carami.todo.config 패키지를 생성합니다.

```
package carami.todo.config;

import org.springframework.context.annotation.Configuration;

@Configuration
public class RootApplicationContextConfig {
}
```

```
package carami.todo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;
import org.springframework.web.servlet.view.InternalResourceViewResolver;
import org.springframework.web.servlet.view.JstlView;

@Configuration
@EnableWebMvc
@ComponentScan(basePackages = {"carami.todo.controller"})
public class ServletContextConfig extends WebMvcConfigurerAdapter {
    @Bean
    public ViewResolver viewResolver() {
         InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
        viewResolver.setViewClass(JstlView.class);
        viewResolver.setPrefix("/WEB-INF/views/");
        viewResolver.setSuffix(".jsp");
        return viewResolver;
    }
}
```

carami.todo.controller 패키지를 생성합니다.

```
package carami.todo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

/**
 * Created by 강경미 on 2017. 6. 3..
 */
@Controller
public class HelloController {
    @GetMapping(path = "/")
    public String hello(Model model){
        return "hello";
    }

}
```

src/main/webapp/WEB-INF/views 폴더를 생성한다. 해당 폴더 안에 hello.jsp 를 작성한다.

```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>hello</title>
</head>
<body>
<h1>Hello World</h1>
</body>
</html>
```

터미널을 실행한 후 해당 프로젝트 폴더로 이동한다. maven path설정을 하기 전에 연 터미널이라면 mvn이 실행이 안될 수 있다.
이때는 터미널을 닫았다가 다시 연다.

mvn clean install
mvn tomcat7:run

위와 같이 실행한 후 http://localhost:8080/ 을 브라우저로 연다. Hello가 출력되면 성공

아래는 mvn tomcat7:run을 했을때 나오는 화면. tomcat을 설치하지 않고 maven에 내장된 방식으로 실행할 수 있다.
어떤 값이 로그로 출력되었는지 살펴보도록 하자.
```
477 urstory:todo $ mvn tomcat7:run
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building todo 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] >>> tomcat7-maven-plugin:2.2:run (default-cli) > process-classes @ todo >>>
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ todo ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.3:compile (default-compile) @ todo ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] <<< tomcat7-maven-plugin:2.2:run (default-cli) < process-classes @ todo <<<
[INFO]
[INFO] --- tomcat7-maven-plugin:2.2:run (default-cli) @ todo ---
[INFO] Running war on http://localhost:8080/
[INFO] Creating Tomcat server configuration at /DEVEL/eclipse_workspace/todo/target/tomcat
[INFO] create webapp with contextPath:
Jun 24, 2017 3:04:02 PM org.apache.coyote.AbstractProtocol init
정보: Initializing ProtocolHandler ["http-bio-8080"]
Jun 24, 2017 3:04:02 PM org.apache.catalina.core.StandardService startInternal
정보: Starting service Tomcat
Jun 24, 2017 3:04:02 PM org.apache.catalina.core.StandardEngine startInternal
정보: Starting Servlet Engine: Apache Tomcat/7.0.47
Jun 24, 2017 3:04:04 PM org.apache.catalina.core.ApplicationContext log
정보: No Spring WebApplicationInitializer types detected on classpath
Jun 24, 2017 3:04:04 PM org.apache.catalina.core.ApplicationContext log
정보: Initializing Spring root WebApplicationContext
Jun 24, 2017 3:04:04 PM org.springframework.web.context.ContextLoader initWebApplicationContext
정보: Root WebApplicationContext: initialization started
Jun 24, 2017 3:04:04 PM org.springframework.web.context.support.AnnotationConfigWebApplicationContext prepareRefresh
정보: Refreshing Root WebApplicationContext: startup date [Sat Jun 24 15:04:04 KST 2017]; root of context hierarchy
Jun 24, 2017 3:04:04 PM org.springframework.web.context.support.AnnotationConfigWebApplicationContext loadBeanDefinitions
정보: Successfully resolved class for [carami.todo.config.RootApplicationContextConfig]
Jun 24, 2017 3:04:04 PM org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor <init>
정보: JSR-330 'javax.inject.Inject' annotation found and supported for autowiring
Jun 24, 2017 3:04:04 PM org.springframework.web.context.ContextLoader initWebApplicationContext
정보: Root WebApplicationContext: initialization completed in 376 ms
Jun 24, 2017 3:04:04 PM org.apache.catalina.core.ApplicationContext log
정보: Initializing Spring FrameworkServlet 'dispatcher'
Jun 24, 2017 3:04:04 PM org.springframework.web.servlet.DispatcherServlet initServletBean
정보: FrameworkServlet 'dispatcher': initialization started
Jun 24, 2017 3:04:04 PM org.springframework.web.context.support.AnnotationConfigWebApplicationContext prepareRefresh
정보: Refreshing WebApplicationContext for namespace 'dispatcher-servlet': startup date [Sat Jun 24 15:04:04 KST 2017]; parent: Root WebApplicationContext
Jun 24, 2017 3:04:04 PM org.springframework.web.context.support.AnnotationConfigWebApplicationContext loadBeanDefinitions
정보: Successfully resolved class for [carami.todo.config.ServletContextConfig]
Jun 24, 2017 3:04:04 PM org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor <init>
정보: JSR-330 'javax.inject.Inject' annotation found and supported for autowiring
Jun 24, 2017 3:04:05 PM org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping register
정보: Mapped "{[/],methods=[GET]}" onto public java.lang.String carami.todo.controller.HelloController.hello(org.springframework.ui.Model)
Jun 24, 2017 3:04:05 PM org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter initControllerAdviceCache
정보: Looking for @ControllerAdvice: WebApplicationContext for namespace 'dispatcher-servlet': startup date [Sat Jun 24 15:04:04 KST 2017]; parent: Root WebApplicationContext
Jun 24, 2017 3:04:05 PM org.springframework.web.servlet.DispatcherServlet initServletBean
정보: FrameworkServlet 'dispatcher': initialization completed in 673 ms
Jun 24, 2017 3:04:05 PM org.apache.coyote.AbstractProtocol start
정보: Starting ProtocolHandler ["http-bio-8080"]

```

4. git에 프로젝트 올리기

- todo프로젝트 아래에 .gitignore 파일을 생성한다.

```
.classpath
.project
.settings
target

```

사용자마다 다른 값을 가질 수 있는 eclipse 프로젝트 설정 파일과 빌드한 내용이 저장되는 target 폴더를 .gitignore에 추가하여 형상관리 서버에 올라가지 못하도록 한다.

- github.com 에서 todo 프로젝트를 생성한다.(public 프로젝트로 생성)

```
echo "# todo" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/carami/todo.git
git push -u origin master
```

프로젝트를 생성하면 보통 위와 같은 메시지가 나온다.

- elcipse todo 프로젝트 안에서 다음과 같이 명령을 수행한다.

```
git init
git add *
git commit -m "first commit"
git remote add origin https://github.com/carami/todo.git
git push -u origin master
```

실제 명령 실행과 결과
```
480 carami:todo $ git init
Initialized empty Git repository in /DEVEL/eclipse_workspace/todo/.git/
481 carami:todo (master #)$ git add *
The following paths are ignored by one of your .gitignore files:
target
Use -f if you really want to add them.
482 carami:todo (master +)$ git commit -m "first commit"
[master (root-commit) 93163cd] first commit
 6 files changed, 200 insertions(+)
 create mode 100644 pom.xml
 create mode 100644 src/main/java/carami/todo/config/RootApplicationContextConfig.java
 create mode 100644 src/main/java/carami/todo/config/ServletContextConfig.java
 create mode 100644 src/main/java/carami/todo/controller/HelloController.java
 create mode 100644 src/main/webapp/WEB-INF/views/hello.jsp
 create mode 100644 src/main/webapp/WEB-INF/web.xml
483 carami:todo (master)$ git remote add origin https://github.com/carami/todo.git
484 carami:todo (master)$ git push -u origin master
Counting objects: 18, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (18/18), 2.92 KiB | 0 bytes/s, done.
Total 18 (delta 0), reused 0 (delta 0)
To https://github.com/carami/todo.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
485 carami:todo (master)$
```

https://github.com/carami/todo 를 가보면 git에 올라가 있는 것을 확인할 수 있다.
물론, 이 예제를 따라하는 분들은 https://github.com/본인아이디/todo 로 확인을 해야합니다.

터미널로 git을 다루는 것은 불편하니, 여기까지 진행을 한 후에는 Source tree를 이용하여 관리하는 것을 추천합니다.
