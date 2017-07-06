

5. 테스트 코드 작성


pom.xml 파일에 다음과 같은 라이브러리를 추가한다.

```
<!-- mockito -->
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-all</artifactId>
    <version>1.10.19</version>
</dependency>

<!-- json path -->
<dependency>
    <groupId>com.jayway.jsonpath</groupId>
    <artifactId>json-path</artifactId>
    <version>2.2.0</version>
</dependency>

```

테스트 코드의 작성

@Before아래에 있는 설정 test코드끼지만 작성하고 테스트. 하나씩 메소드를 늘려가며 테스트한다.

```
package carami.jdbc;

import carami.todo.controller.MemberRestController;
import carami.todo.domain.Member;
import carami.todo.dto.MemberParam;
import carami.todo.service.MemberService;
import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;
import org.mockito.invocation.InvocationOnMock;
import org.mockito.runners.MockitoJUnitRunner;
import org.mockito.stubbing.Answer;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;

import static junit.framework.TestCase.assertTrue;
import static org.mockito.Matchers.any;
import static org.mockito.Matchers.anyLong;
import static org.mockito.Mockito.verify;
import static org.mockito.Mockito.when;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.delete;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.put;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@RunWith(MockitoJUnitRunner.class)
public class MemberRestControllerTest {

    // 컨트롤러에서 해당 Service는 완전하다고 가정하기 때문에 Mock으로 선언한다.
    @Mock
    MemberService memberService;

    @InjectMocks
    MemberRestController controller;

    MockMvc mvc;

    private static long id = 1L;

    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this);

        this.mvc = MockMvcBuilders.standaloneSetup(controller)
                .build();


        // memberService의 addMember에는 Member를 파라미터로 받아들이게 되는데,
        // thenAnswer는 Answer를 상속받는 객체를 반환한다.
        // Answer<Member>(){ ... } 는 Member를 반환하는 이름없는 객체를 생성을 하는 것이고
        // 그 안의 메소드는 Answer가 가지고 있는 메소드를 오버라이딩 한다.
        // InvocationOnMockdms Mock객체에 대한 레퍼런스를 전달하여 해당 Mock이 파라미터로 받은 값을 구할때 사용한다.
        // Mock객체가 받은 Member파라미터의 id값을 1부터 시작하는 값으로 자동으로 설정되게 하였다.
        when(memberService.addMember(any(Member.class))).thenAnswer(
                new Answer<Member>() {
                    @Override
                    public Member answer(InvocationOnMock invocation) throws Throwable {
                        Object[] args = invocation.getArguments(); // arguments
                        MemberService mock = (MemberService)invocation.getMock();
                        Member member = (Member)args[0];
                        member.setId(id++);
                        return member;
                    }
                }
        );


        // delete메소드는 long값을 받아들여 1을 반환한다.
        when(memberService.delete(anyLong())).thenReturn(1);

        // update메소드는 Member를 받아들여 1을 반환한다.
        when(memberService.update(any(Member.class))).thenReturn(1);

        Member member = new Member(1L, "강경미", "carami@nate.com", "1234");
        // get메소드는 long값을 받아들여 member객체를 반환한다.
        when(memberService.get(anyLong())).thenReturn(member);
    }


    @Test
    public void configTest(){
        assertTrue(true);
    }

    // jsonPath를 사용하려면 com.jayway.jsonpath.Predicate 에 대한 라이브러리가 설정되어있어야 한다.
    @Test
    public void shouldCreate() throws Exception {
        String requestBody = "{\"name\":\"강경미\", \"email\":\"carami@nate.com\", \"passwd\":\"1234\" }";

        mvc.perform(
                post("/api/members")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(requestBody)
        )
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.id").exists())
                .andExpect(jsonPath("$.name").value("강경미"))
                .andExpect(jsonPath("$.email").value("carami@nate.com"));
                //.andExpect(jsonPath("$.passwd").value("1234")); // passwd값은 @jsoinignore를 하였기 때문에 결과가 나오지 않아야 한다.

        // controller내부적으로 회원정보가 등록할 때 addMember가 호출되었는지 검사한다.
        verify(memberService).addMember(any(Member.class));

    }


    @Test
    public void getMember() throws Exception{
        mvc.perform(
                get("/api/members/{id}", 1)
                        .contentType(MediaType.APPLICATION_JSON)
        ).andExpect(status().isOk())
        .andExpect(jsonPath("$.id").exists())
        .andExpect(jsonPath("$.name").value("강경미"))
        .andExpect(jsonPath("$.email").value("carami@nate.com"));

        verify(memberService).get(anyLong());
    }

    @Test
    public void shoulDelete() throws Exception {
        MemberParam member = new MemberParam();
        member.setName("강경미");
        member.setEmail("carami@nate.com");
        member.setPasswd("1234");
        Member resultMember = controller.create(member);

        mvc.perform(
                delete("/api/members/{id}", resultMember.getId())
                        .contentType(MediaType.APPLICATION_JSON)
        )
                .andExpect(status().isOk());

        verify(memberService).delete(anyLong());
    }

    @Test
    public void shoulUpdate() throws Exception {
        MemberParam member = new MemberParam();
        member.setName("강경미");
        member.setEmail("carami@nate.com");
        member.setPasswd("1234");
        Member resultMember = controller.create(member);
        resultMember.setName("강경미2");
        resultMember.setEmail("carami2@nate.com");

        String requestBody = "{\"id\": " + resultMember.getId() + ", \"name\":\"강경미2\", \"email\":\"carami2@nate.com\" }";

        mvc.perform(
                put("/api/members")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(requestBody)
        )
                .andExpect(status().isOk());

        verify(memberService).update(any(Member.class));
    }
}

```
