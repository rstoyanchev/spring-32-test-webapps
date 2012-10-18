!SLIDE subsection
# Spring TestContext Framework

!SLIDE bullets center
* Introduced in Spring 2.5

!SLIDE bullets center
* Revised in Spring 3.2 ...

!SLIDE bullets center
* with a focus on web apps

!SLIDE bullets center
* Unit _and_ integration testing

!SLIDE bullets center
* Annotation-driven

!SLIDE bullets center
* Convention over Configuration

!SLIDE bullets center
* JUnit & TestNG

!SLIDE incremental small
# Spring & Integration Testing
* `ApplicationContext` management & caching
* Dependency injection of test instances
* Transactional test management
  * with default rollback semantics
* `JdbcTestUtils` (-`SimpleJdbcTestUtils`-)

!SLIDE incremental smaller
# Spring Test Annotations
* Application Contexts
  * `@ContextConfiguration`, `@DirtiesContext`
* Dependency Injection
  * `@Autowired`
* Transactions
  * `@Transactional`, `@TransactionConfiguration`, `@BeforeTransaction`
* Web
  * `@WebAppConfiguration`

!SLIDE incremental small
# Using the TestContext Framework
* Use the `SpringJUnit4ClassRunner` for __JUnit__ 4.5+
* Instrument test class with `TestContextManager` for __TestNG__
* Extend one of the base classes
  * `Abstract(Transactional)[JUnit4|TestNG]SpringContextTests`

!SLIDE small transition=fade
# Example: @POJO Test Class
	@@@ java




	public class OrderServiceTests {










	  @Test
	  public void testOrderService() { … }
	}


!SLIDE small
# Example: @POJO Test Class
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)



	public class OrderServiceTests {










	  @Test
	  public void testOrderService() { … }
	}


!SLIDE small
# Example: @POJO Test Class
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration


	public class OrderServiceTests {










	  @Test
	  public void testOrderService() { … }
	}


!SLIDE small
# Example: @POJO Test Class
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration // defaults to
	// OrderServiceTests-context.xml in same package

	public class OrderServiceTests {










	  @Test
	  public void testOrderService() { … }
	}


!SLIDE small
# Example: @POJO Test Class
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration // defaults to
	// OrderServiceTests-context.xml in same package

	public class OrderServiceTests {

	  @Autowired
	  private OrderService orderService;







	  @Test
	  public void testOrderService() { … }
	}


!SLIDE small
# Example: @POJO Test Class
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration // defaults to
	// OrderServiceTests-context.xml in same package
	@Transactional
	public class OrderServiceTests {

	  @Autowired
	  private OrderService orderService;







	  @Test
	  public void testOrderService() { … }
	}


!SLIDE small
# Example: @POJO Test Class
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration // defaults to
	// OrderServiceTests-context.xml in same package
	@Transactional
	public class OrderServiceTests {

	  @Autowired
	  private OrderService orderService;

	  @BeforeTransaction
	  public void verifyInitialDatabaseState() { … }




	  @Test
	  public void testOrderService() { … }
	}


!SLIDE small
# Example: @POJO Test Class
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration // defaults to
	// OrderServiceTests-context.xml in same package
	@Transactional
	public class OrderServiceTests {

	  @Autowired
	  private OrderService orderService;

	  @BeforeTransaction
	  public void verifyInitialDatabaseState() { … }

	  @Before
	  public void setUpTestDataWithinTransaction() { … }

	  @Test
	  public void testOrderService() { … }
	}


!SLIDE subsection small
# Core Components

!SLIDE incremental
# `TestContext`
* Tracks context for current test
* Delegates to a `ContextLoader`
* Caches the `ApplicationContext`

!SLIDE incremental
# `TestContextManager`
* Manages the `TestContext`
* Signals events to listeners

!SLIDE incremental
# `TestExecutionListener` SPI
* Reacts to test execution events
* Receives reference to current `TestContext`

!SLIDE center small transition=fade
# `TestExecutionListener` 2.5
![Testing-TEL-CD-2.5.png](Testing-TEL-CD-2.5.png)

!SLIDE incremental
# `SmartContextLoader` SPI
* Strategy for loading application contexts
* From `@Configuration` classes _or_ resource locations
* Supports environment profiles and context initializers

!SLIDE center small transition=fade
# `ContextLoader` 3.1
![Testing-ContextLoader-CD-3.1.png](Testing-ContextLoader-CD-3.1.png)

!SLIDE center small transition=scrollUp
# Putting it all together
![Testing-TCF-CoreComponents.png](Testing-TCF-CoreComponents.png)


!SLIDE subsection
# What's New in 3.2

!SLIDE bullets center
* Upgraded to JUnit 4.10<br /> and TestNG 6.5.2

!SLIDE bullets center
* `spring-test` now depends on `junit:junit-dep`

!SLIDE bullets center
* Now inferring return type of<br />generic factory methods

!SLIDE bullets center
* Updated Spring _mocks_

!SLIDE bullets center
* Simplified transaction manager configuration

!SLIDE bullets center
* Improved JDBC integration<br />testing support

!SLIDE bullets center
* Documented support for...

!SLIDE bullets center
* `@Bean` _lite_ mode

!SLIDE bullets center
* and

!SLIDE bullets center
* JSR-250 lifecycle annotations

!SLIDE bullets center
* Now supporting...

!SLIDE bullets center
* `WebApplicationContext`

!SLIDE bullets center
* session & request scoped beans

!SLIDE bullets center
* and

!SLIDE bullets center
* `ApplicationContextInitializer`

!SLIDE bullets center
* Planned support for...

!SLIDE bullets center
* context hierarchies

!SLIDE incremental small
# Generic Factory Methods
* Not specific to testing
* Sometimes used for creating a proxy
* But often used for mocking Spring beans

!SLIDE smaller
# Example: Mocking with EasyMock
	@@@ xml
	<beans ...>
	
		<!-- OrderService is autowired with OrderRepository -->
		<context:component-scan base-package="com.example"/>
	
		<bean id="orderRepository" class="org.easymock.EasyMock"
			factory-method="createMock"
			c:_="com.example.repository.OrderRepository" />
	
	</beans>

!SLIDE smaller
# EasyMock.createMock() Signature
	@@@ java
	
	
	
	
	public static <T> T createMock(Class<T> toMock) {...}

!SLIDE incremental small
# Generic Factory Methods - Improved
* Generic return types are now inferred
  * `public static <T> T mock(Class<T> clazz)`
  * `public static <T> T proxy(T obj)`
* Autowiring _by type_ now works
* And custom solutions are now chiefly unnecessary:
  * `MockitoFactoryBean`
  * `EasyMockFactoryBean`
  * [Springockito](https://bitbucket.org/kubek2k/springockito/wiki/Home)

!SLIDE incremental
# Spring Environment Mocks
* introduced `MockEnvironment` in 3.2
* complements `MockPropertySource` from 3.1

!SLIDE incremental
# New Spring HTTP Mocks
* Client:
  * `MockClientHttpRequest`
  * `MockClientHttpResponse`
* Server:
  * `MockHttpInputMessage`
  * `MockHttpOutputMessage`

!SLIDE incremental
# Enhanced Servlet API Mocks
* `MockServletContext`
* `MockHttpSession`
* `MockFilterChain`
* `MockRequestDispatcher`
* ...

!SLIDE incremental
# Transaction Manager Config
* support for single, _unqualified_ transaction manager
* support for `TransactionManagementConfigurer`
* `@TransactionConfiguration` is now rarely necessary

!SLIDE incremental smaller
# JDBC Testing Support
* deprecated `SimpleJdbcTestUtils` in favor of `JdbcTestUtils`
* introduced `countRowsInTableWhere()` and `dropTables()` in `JdbcTestUtils`
* introduced `jdbcTemplate`, `countRowsInTableWhere()`, and `dropTables()` in
  * `AbstractTransactionalJUnit4SpringContextTests`
  * `AbstractTransactionalTestNGSpringContextTests`

!SLIDE incremental
# Documentation
* support for _`@Bean` lite mode_ and annotated classes
* support for JSR-250 lifecycle annotations
