# X-Ray
-  Visual analysis of our applications.
- Troubleshoot performances, understand dependencies, pinpoint service issues, find errors and exceptions
- Leverages on tracing a request.
- Enabled via your code, import X-Ray SDK, it captures HTTP/S calls, DB calls, Queue and other AWS service calls
- Install the X-Ray daemon or enable X-Ray aws integration, Lambda already has deamon. Each app must have the rights to write data to x-ray.

## Instrumentation and concepts
- Instrumentation:- the measure of products performance, diagonise error, and write trace info
- **Segments**:- each app/serv will send them
- **SubSegments**:- if you need more details in your segment
- **Trace**:- segments collected together to form an e2e trace
- **Sampling**:- decr the amount of requests sent to X-Ray, reduce costs. 
    - by default, the X-ray SDK records the first req each second and five percent of any additional requests.

- **Annotations**:- Key Value pairs used to index traces and use with filters
- **Metadata**:- demo-launch-template