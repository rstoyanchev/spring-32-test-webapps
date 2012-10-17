!SLIDE subsection
# Web Testing Support in the TCF

!SLIDE incremental small
# New Web Testing Features
* First-class `WebApplicationContext` support
* `Request` and `Session` scoped beans

!SLIDE subsection
# Loading a `WebApplicationContext`

!SLIDE bullets center
* How do you tell Spring to load a `WebApplicationContext`?

!SLIDE bullets center
* Just annotate your test class with ...

!SLIDE incremental small
# `@WebAppConfiguration`
* Denotes that the `ApplicationContext` should be a `WebApplicationContext`
* Configures the resource path for the web app
  * used in the `MockServletContext`
* Paths are file-system folders, relative to the project root
  * not classpath resources
* Defaults to `"src/main/webapp"`
* The `classpath:` prefix is also supported

!SLIDE smaller
# Example: @WebAppConfiguration
	@@@ java
	@WebAppConfiguration // defaults to "file:src/main/webapp"
	@ContextConfiguration
	public class WacTests {
		//...
	}

!SLIDE smaller
# Example: @WebAppConfiguration
	@@@ java
	// file system resource
	@WebAppConfiguration("src/test/resources/web")
	// classpath resource
	@ContextConfiguration("/test-web-config.xml")
	public class WacTests {
		//...
	}

!SLIDE smaller
# Example: @WebAppConfiguration
	@@@ java
	// classpath resource
	@WebAppConfiguration("classpath:test-web-resources")
	// classpath resource
	@ContextConfiguration("/test-web-config.xml")
	public class WacTests {
		//...
	}

!SLIDE incremental small
# Web Context Loaders
* New `AbstractGenericWebContextLoader`
* And two concrete subclasses:
  * `XmlWebContextLoader`
  * `AnnotationConfigWebContextLoader`
* Plus a `WebDelegatingSmartContextLoader`

!SLIDE center small transition=fade
# `SmartContextLoader` 3.2
![SmartContextLoader-Type-Hierarchy.png](SmartContextLoader-Type-Hierarchy.png)

!SLIDE incremental smaller
# `WebTestExecutionListener`
* Sets up default thread-local state via `RequestContextHolder` before each test method
* Creates:
  * `MockHttpServletRequest`
  * `MockHttpServletResponse`
  * `ServletWebRequest`
* Ensures that the `MockHttpServletResponse` and `ServletWebRequest` can be injected into the test instance
* Cleans up thread-local state after each test method

!SLIDE center small
# `TestExecutionListener` 2.5
![Testing-TEL-CD-2.5.png](Testing-TEL-CD-2.5.png)

!SLIDE center small transition=fade
# `TestExecutionListener` 3.2
![Testing-TEL-CD-3.2.png](Testing-TEL-CD-3.2.png)

!SLIDE smaller
# Example: Injecting Mocks
	@@@ java
	@WebAppConfiguration
	@ContextConfiguration
	public class WacTests {
		
		@Autowired WebApplicationContext wac;
		
		@Autowired MockServletContext servletContext;
		
		@Autowired MockHttpSession session;
		
		@Autowired MockHttpServletRequest request;
		
		@Autowired MockHttpServletResponse response;
		
		@Autowired ServletWebRequest webRequest;
		
		//...
	}

!SLIDE incremental small
# `WebMergedContextConfiguration`
* Extension of `MergedContextConfiguration`
* Supports the `baseResourcePath` from `@WebAppConfiguration`
  * which is included in the _context cache key_

!SLIDE subsection
# Request and Session Scoped Beans

!SLIDE incremental small
# ???
* ???

