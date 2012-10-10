!SLIDE subsection
# Spring MVC Test Framework

!SLIDE small incremental bullets
# Origins of Spring MVC Test
* Based on [spring-test-mvc](github.com/SpringSource/spring-test-mvc) project on Github
* Just added to `spring-test` module (Spring 3.2 RC1)
* Nearly identical code base
* Github project will continue to support Spring 3.1

!SLIDE small incremental bullets
# Differences with [spring-test-mvc](github.com/SpringSource/spring-test-mvc)
* Depends on Spring 3.2 vs. Spring 3.1
* Integrated with `@WebAppConfiguration`
* Provides Servlet 3 async support
* Different packages
* Straight-forward to migrate

!SLIDE small incremental bullets
# High Level Description
* 1st class support for testing Spring MVC apps
* Using a fluent API
* Server-side tests involve the __`DispatcherServlet`__
* Client-side tests __`RestTemplate`__-based

!SLIDE small incremental bullets
# Built on `spring-test`
* `TestContext` framework for loading Spring config
* `MockHttpServletRequest/Response`
* `MockFilterChain`
* Servlet container is not used

!SLIDE small incremental bullets
# Extent of Support
* HTTP message conversion fully supported<br> e.g. `@RequestBody`/`@ResponseBody`
* Most rendering technologies as well<br> JSON, XML, Freemarker/Velocity, Thyme, etc
* No JSP rendering and redirects<br> Assert selected JSP view or redirected URL

