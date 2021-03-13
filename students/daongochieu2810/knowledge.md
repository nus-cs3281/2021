### Angular

Aspects: Components, Templates, Directives, Dependency Injection
Source: Official Docs

- Learnt about `ngTemplate` to make reusable widgets
- Learnt about specificity of CSS to change component styles without directives or Typescripts code changes

### RxJs
- Learnt about `observable` to receive APIs responses
- Learnt about ```concat``` and ```merge``` to call paginated API requests
- Learnt about ```catchError``` to handle errors without ending the stream of API requests allowing the FE to retry sending requests upon failures such as timeout

### Google App Engine

Aspects: Database access and modification, APIs, Cron jobs, Task Queue. 
Source: Teammates code base, Official Docs
- Learnt about GAE timeout and its 

### Java
- Learnt that high-level data structures should be prioritized over primitive ones to make the code more extendable e.g ArrayList should be used instead of array
- Revised best programming practices and OOP principles in Java 

### Objectify
- Learnt about server-side filtering feature

### Google Cloud Platform
- Learnt that parallelism might cause mutiple instances to be created and thus increase the cost
- Used Cloud Trace to find performance bottle-neck of API calls 
- Used Cloud Profiler to identify and analyze performance bottlenecks
- Used staging server to simulate a production environment for stress testing on performance bottlenecks

### Backend Techniques & Testing
- Paginatied API calls: break a huge request down into smaller requests. This allows for more data to be sent at the cost of additional performance overhead
- Created sripts to generate mock data and retrieve information about the database for the admin
- Created LnP test scripts to assess performance of heavily used APIs such as student enrolment
- Configured `test.properties` and `JMeterElements.java` for running LnP scripts against staging server by adding domain and port of the staging server as target point
- Learnt about `JMeter` as an application for LnP testing:
    - Create test data
    - Create csv test data to be send with APIs to BE
    - Construct LnP test plan: 
        - Add csrf token and login sampler for CRUD actions
        - Add request headers
        - Add http sampler to carry out the request to BE with created test data
        - Run the test concurrently by configuring the number of threads
    - Remove data from datastore and delete test files after the test
