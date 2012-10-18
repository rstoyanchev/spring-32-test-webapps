!SLIDE subsection
# Application Context Hierarchies

!SLIDE bullets center
* __WARNING__
* _Support for context hierarchies<br />has __not__ yet been implemented._

!SLIDE incremental small
# Status Quo for Tests

!SLIDE bullets center
* Currently only flat, non-hierarchical<br />contexts are supported.

!SLIDE bullets center
* There is no easy way to create contexts<br />with parent-child relationships.

!SLIDE bullets center
* But hierarchies are supported in production.

!SLIDE bullets center
* So it would be nice to be able to test them. ;)

!SLIDE incremental small
# Context Hierarchy Goals
* Load a test application context with a parent context
* Support common hierarchies
  * Root WAC <-- Dispatcher WAC
  * EAR <-- Root WAC <-- Dispatcher WAC

!SLIDE incremental small
# Context Hierarchy Proposal
* Introduce `@ContextHierarchy` that contains nested `@ContextConfiguration` declarations
* Introduce a `name` attribute in `@ContextConfiguration` 
  * for _merging_ or _overriding_ named configuration in the context hierarchy

!SLIDE smaller
# Single Test with Context Hierarchy
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	
	@ContextHierarchy({
		@ContextConfiguration("parent.xml"),
		@ContextConfiguration("child.xml")
	})
	public class AppCtxHierarchyTests {}

!SLIDE smaller
# Root WAC & Dispatcher WAC
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	
	@WebAppConfiguration
	
	@ContextHierarchy({
	    @ContextConfiguration(
			name="root",
			classes = WebAppConfig.class),
	    @ContextConfiguration(
			name="dispatcher",
			locations="/spring/dispatcher-config.xml")
	})
	public class ControllerIntegrationTests {}

!SLIDE smaller
# Class & Context Hierarchies
	@@@ java
	@RunWith(SpringJUnit4ClassRunner.class)
	@WebAppConfiguration
	@ContextConfiguration("file:src/main/webapp/WEB-INF/applicationContext.xml")
	public abstract class AbstractWebTests {}
	
	@ContextHierarchy(@ContextConfiguration("/spring/soap-ws-config.xml")
	public class SoapWebServiceTests extends AbstractWebTests {}
	
	@ContextHierarchy(@ContextConfiguration("/spring/rest-ws-config.xml")
	public class RestWebServiceTests extends AbstractWebTests {}

!SLIDE incremental
# Feedback is Welcome
* [SPR-5613](https://jira.springsource.org/browse/SPR-5613): context hierarchy support
* [SPR-9863](https://jira.springsource.org/browse/SPR-9683): web context hierarchy support
