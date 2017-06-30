1. 서블릿의 라이프 사이클 확인하기.
 - HelloServlet class 작성
 ```
 public class HelloServlet extends HttpServlet {
 	private static final long serialVersionUID = 1L;
 	@Override
 	public void destroy() {
 		System.out.println("destory");
 	}

 	@Override
 	public void init() throws ServletException {
 		System.out.println("destory");
 	}

 	@Override
 	protected void service(HttpServletRequest arg0, HttpServletResponse arg1) throws ServletException, IOException {
 		System.out.println("service");
 	}
 }
 ```
 - web.xml 알아보기
 ```
 <servlet>
   <description></description>
   <display-name>HelloServlet</display-name>
   <servlet-name>HelloServlet</servlet-name>
   <servlet-class>kr.or.connect.HelloServlet</servlet-class>
 </servlet>
 <servlet-mapping>
   <servlet-name>HelloServlet</servlet-name>
   <url-pattern>/HelloServlet</url-pattern>
 </servlet-mapping>
 ```
 - init(), service(), destory()
 - Get vs Post   

 2. HttpServletRequest, HttpServletResponse 객체 이해하기  

 3. redirect vs foward

 4. Cookie 와 Session

 5. scope (page, request, sesssion, Application) 이해하기
