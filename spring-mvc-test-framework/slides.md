!SLIDE subsection
# Spring MVC Test Framework

!SLIDE small incremental bullets
# Background
* Recently added to `spring-test` as of Spring 3.2 RC1
* Originates from [spring-test-mvc](github.com/SpringSource/spring-test-mvc) separate project on Github
* Nearly identical code bases
* [spring-test-mvc](github.com/SpringSource/spring-test-mvc) will continue to support Spring 3.1

!SLIDE small incremental bullets
# Differences with [spring-test-mvc](github.com/SpringSource/spring-test-mvc)
* Dependency on Spring 3.2, not Spring 3.1
* Support for Spring 3.2 features (e.g. Servlet 3 async)
* Integration with `@WebAppConfiguration`
* Different packages
* Easy migration from spring-test-mvc to Spring 3.2

!SLIDE small incremental bullets
# What does it provide?
* 1st class support for testing Spring MVC apps
* Fluent API
* Server-side tests involve the __`DispatcherServlet`__
* Client-side tests are __`RestTemplate`__-based

!SLIDE small incremental bullets
# Built on `spring-test`
* `TestContext` framework used for loading Spring config
* `MockHttpServletRequest/Response`
* `MockFilterChain`
* Servlet container is not used

!SLIDE small incremental bullets
# Extent of support
* Pretty much everything should work as it does at runtime 
* HTTP message conversion<br> e.g. `@RequestBody`/`@ResponseBody`
* Most rendering technologies<br> JSON, XML, Freemarker/Velocity, Thymeleaf, Excel, etc

!SLIDE small incremental bullets
# Limitations

* We are not in a servlet container
* Foward and redirect not executed
* No JSP rendering

