!SLIDE subsection
# Client-side<br> `RestTemplate` Tests

!SLIDE smaller
# Example
    @@@ java
    
    RestTemplate restTemplate = new RestTemplate();
    
    MockRestServiceServer mockServer = 
        MockRestServiceServer.createServer(restTemplate);

    mockServer.expect(requestTo("/greeting"))
        .andRespond(withSuccess("Hello world", "text/plain"));


    // use RestTemplate ...


    mockServer.verify();

!SLIDE small incremental bullets
# Static Imports
* `MockRestRequestMatchers.*` and `MockRestResponseCreators.*`
* Add as _"favorite packages"_ in Eclipse prefs<br> for code completion
* Or simply search `"MockRest*"`

!SLIDE small bullets incremental
# `RestTemplate` Instrumentation
* `MockRestServiceServer.createServer(restTemplate)`
* Configures `RestTemplate` with custom `ClientHttpRequestFactory`
* Can be used further to ...
* Define expected requests and responses
* Verify expectations

!SLIDE small bullets incremental
# Define Expected Requests-Responses
* `mockServer.expect(..).andExpect(..)`
* Followed by `.andReturn(..)`
* Set up any number of requests with `mockServer.expect(..)`
    
!SLIDE small bullets incremental
# Each Time `RestTemplate` Is Used
* Request expectations asserted
* Stub response returned
* Exception raised if no more expected requests

!SLIDE small bullets incremental
# After `RestTemplate` Is Used
* `mockServer.verify(..)`
* Ensures all expected requests executed


