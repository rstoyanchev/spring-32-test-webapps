!SLIDE subsection
# Server-side<br> Spring MVC Tests

!SLIDE small
# Example

!SLIDE smaller
    @@@ java
    @RunWith(SpringJUnit4ClassRunner.class)
    @WebAppConfiguration
    @ContextConfiguration("servlet-context.xml")
    public class SampleTests {



















    }

!SLIDE smaller
    @@@ java
    @RunWith(SpringJUnit4ClassRunner.class)
    @WebAppConfiguration
    @ContextConfiguration("servlet-context.xml")
    public class SampleTests {

      @Autowired
      private WebApplicationContext wac;

      private MockMvc mvc;

      @Before
      public void setup() {
        this.mvc = webAppContextSetup(this.wac).build();
      }









    }

!SLIDE smaller
    @@@ java
    @RunWith(SpringJUnit4ClassRunner.class)
    @WebAppConfiguration
    @ContextConfiguration("servlet-context.xml")
    public class SampleTests {

      @Autowired
      private WebApplicationContext wac;

      private MockMvc mvc;

      @Before
      public void setup() {
        this.mvc = webAppContextSetup(this.wac).build();
      }

      @Test
      public void getFoo() throws Exception {
        this.mvc.perform(get("/foo").accept("application/json"))
	        .andExpect(status().isOk())
	        .andExpect(content().mimeType("application/json"))
	        .andExpect(jsonPath("$.name").value("Lee"));
      }

    }

!SLIDE small bullets incremental
# Static Imports
* `MockMvcBuilders.*`, `MockMvcRequestBuilders.*`, `MockMvcResultMatchers.*`
* Add as _"favorite packages"_ in Eclipse prefs<br> for code completion
* Or simply search `"MockMvc*"`

!SLIDE small bullets incremental
# Set-Up Options
* The goal:<br>`MockMvc` instance ready to perform requests
* Prepare `DispatcherServlet` config
* Specify root of webapp
* Add servlet Filters

!SLIDE small bullets incremental
# Option 1: `TestContext` framework
* Load actual `DispatcherServlet` config
* Smart caching of `WebApplicationContext` instances
* Possibly inject controllers with mock services

!SLIDE smaller
# Declaring a Mock Service For Injection
    @@@ xml



    <bean class="org.mockito.Mockito" factory-method="mock">
	    <constructor-arg value="org.example.FooService"/>
    </bean>

!SLIDE small bullets incremental
# Option 2: "Standalone"
* Simply register one or more `@Controller` instances
* Config similar to MVC Java Config
* No Spring context is actually loaded
* "Stub" `WebApplicationContext` used to configure `DispatcherServlet`

!SLIDE smaller
# "Standalone" Setup Example 
    @@@ java


    standaloneSetup(new PersonController()).build()
      .mockMvc.perform(post("/persons").param("name", "Joe"))
        .andExpect(status().isMovedTemporarily())
        .andExpect(redirectedUrl("/persons/Joe"))
        .andExpect(model().size(1)) 
        .andExpect(model().attributeExists("name"));

!SLIDE small bullets incremental
# `@WebAppConfiguration` vs. "Standalone"
* "Standalone" more targetted<br> one controller at a time, explicit config
* `@WebAppConfiguration` verifies application config
* No right or wrong choice, different test styles
* May also mix and match

!SLIDE small bullets incremental
# Performing Requests
* Specify HTTP method and URI at a minimum
* Additional builder-style methods corresponding to `MockHttpServletRequest` fields

!SLIDE smaller
# Request Examples
    @@@ java


    mockMvc.perform(
      post("/hotels/{id}?n={n}", 42, "N"));

    mockMvc.perform(
      fileUpload("/doc").file("a1", "ABC".getBytes("UTF-8")));

!SLIDE small bullets incremental
# Specifying Parameters
* Query string in the URI<br> `get("/hotels?a=b")`
* Form params in the request body<br> `get("/hotels").body("a=b".getBytes("ISO-8859-1"))`
* Servlet parameters<br> `get("/hotels").param("a", "b")`

!SLIDE small bullets incremental
# Context/Servlet Path + PathInfo
* If you specify full path<br> `get("/app/main/hotels/{id}")`
* Then set paths accordingly<br> `get("").contextPath("/app").servletPath("/main")`
* Or leave out context/servlet path<br> `get("/hotels/{id}")`

!SLIDE small bullets incremental
# Default Request Properties
* Performed requests are often similar
* Same header, parameter, cookie
* Specify default request when setting up `MockMvc`
* Performed requests override defaults!

!SLIDE smaller
# Default Request Example
    @@@ java

    webAppContextSetup(this.wac).defaultRequest(get("/")
      .accept(MediaType.APPLICATION_JSON)
      .param("locale", "en_US"));

!SLIDE small bullets incremental
# Defining Expectations
* Simply add one or more `.andExpect(..)`
* After the call to `perform(..)`
* `MockMvcResultMatchers` provides many choices

!SLIDE small bullets incremental
# What Can Be Verified
* Response status, headers, content
* But also Spring MVC specific information
* Flash attrs, handler, model content, etc
* See lots of [sample tests](https://github.com/SpringSource/spring-framework/tree/master/spring-test-mvc/src/test/java/org/springframework/test/web/mock/servlet/samples/standalone/resultmatchers) for examples

!SLIDE small bullets incremental
# JSON and XML Content
* JSONPath
* XPath
* XMLUnit

!SLIDE small bullets incremental
# In Doubt?
* Print all details<br> `mvc.perform("/foo").andDo(print())`
* Or directly acccess the result<br> `MvcResult r = mvc.perform("/foo").andReturn()`

!SLIDE small bullets incremental
# Common Expectations
* Tests often have similar expectations
* Same status, headers
* Specify such expectations when setting up `MockMvc`

!SLIDE smaller
# Common Expectations Example
    @@@ java

    webAppContextSetup(this.wac)
        .andAlwaysExpect(status.isOk())
        .alwaysExpect(content().mimeType("application/xml"))
        .alwaysDo(print())
        .build();

!SLIDE small bullets incremental
# Filters
* Filters may be added when setting up `MockMvc`
* Executed as requests get peformed
* Various possible use case
* See `Spring Security` [sample tests](https://github.com/SpringSource/spring-framework/blob/master/spring-test-mvc/src/test/java/org/springframework/test/web/mock/servlet/samples/context/SpringSecurityTests.java)

!SLIDE small bullets incremental
# [HtmlUnit Integration](https://github.com/rwinch/spring-test-mvc-htmlunit)
* Adapts `HtmlUnit` request-response to `MockHttpServletRequest/Response`
* Enables use of `Selenium` and `Geb Spock` on top
* No running server
* Give it a try, feedback welcome!


