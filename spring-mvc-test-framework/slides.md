!SLIDE small incremental bullets
# Spring MVC Test Framework
* Based on [spring-test-mvc](github.com/SpringSource/spring-test-mvc) project on Github
* In `spring-test` module for Spring 3.2 RC1
* The code is close to identical

!SLIDE small incremental bullets
# Differences with [spring-test-mvc](github.com/SpringSource/spring-test-mvc)
* Spring 3.2 vs Spring 3.1 dependency
* Integration with `@WebAppConfiguration`
* Servlet 3 async support
* Updated package names

!SLIDE small incremental bullets
# What's In Spring MVC Test?
* 1st class support for testing Spring MVC apps
* Both server and client-side
* Server-side tests involve the `DispatcherServlet`
* Client-side tests call the `RestTemplate`

!SLIDE small incremental bullets
# Built on `spring-test`
* TestContext framework for loading configuration
* `MockHttpServletRequest/Response`
* `MockFilterChain`
* Servlet container not used



