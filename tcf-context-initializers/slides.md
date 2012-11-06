!SLIDE subsection
# Application Context Initializers

!SLIDE incremental smaller
# `ApplicationContextInitializer`
* Introduced in Spring 3.1
* Used for programmatic initialization of a `ConfigurableApplicationContext`
* For example:
  * to register property sources
  * to activate profiles against the `Environment`
* Configured in `web.xml` by specifying `contextInitializerClasses` via
  * `context-param` for the `ContextLoaderListener`
  * `init-param` for the `DispatcherServlet`

!SLIDE incremental smaller
# Using Initializers in Tests
* Configured in `@ContextConfiguration` via the `initializers` attribute
* Inheritance can be controlled via the `inheritInitializers` attribute
* An `ApplicationContextInitializer` may configure the entire context
  * XML resource locations or annotated classes are no longer required
* Initializers are now part of the _context cache key_
* Initializers are _ordered_ based on Spring's `Ordered` interface or the `@Order` annotation

!SLIDE smaller
# Example: Single Initializer
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration(
	    locations = "/app-config.xml",
	    initializers = CustomInitializer.class)
	public class ApplicationContextInitializerTests {}

!SLIDE smaller
# Example: Multiple Initializers
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration(
	  locations = "/app-config.xml",
	  initializers = {
	    PropertySourceInitializer.class,
	    ProfileInitializer.class
	  })
	public class ApplicationContextInitializerTests {}

!SLIDE smaller
# Example: Merged Initializers
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration(
	    classes = BaseConfig.class,
	    initializers = BaseInitializer.class)
	public class BaseTest {}
	
	
	@ContextConfiguration(
	    classes = ExtendedConfig.class,
	    initializers = ExtendedInitializer.class)
	public class ExtendedTest extends BaseTest {}

!SLIDE smaller
# Example: Overridden Initializers
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	@ContextConfiguration(
	    classes = BaseConfig.class,
	    initializers = BaseInitializer.class)
	public class BaseTest {}
	
	
	@ContextConfiguration(
	    classes = ExtendedConfig.class,
	    initializers = ExtendedInitializer.class,
	    inheritInitializers = false)
	public class ExtendedTest extends BaseTest {}

!SLIDE smaller
# Example: Initializer w/o Resources
	@@@ java
	// does not declare 'locations' or 'classes'
	@ContextConfiguration(
	    initializers = EntireAppInitializer.class)
	public class InitializerWithoutConfigFilesOrClassesTest {}
