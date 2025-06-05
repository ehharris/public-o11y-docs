.. _java-otel-performance:

***************************************************
Performance reference for Splunk OTel Java agent
***************************************************

.. meta::
   :description: Minimum requirements of the Splunk OTel Java agent, as well as potential constraints impacting performance, and guidelines to optimize and troubleshoot the performance of the agent.

The Splunk OTel Java agent instruments your application by running inside the same Java Virtual Machine (JVM). Like any other software agent, the Java agent requires system resources like CPU, memory, and network bandwidth. The use of resources by the agent is called agent overhead or performance overhead. The Splunk OTel Java agent has minimal impact on system performance when instrumenting JVM applications, although the final agent overhead depends on multiple factors.

Some factors that might increase agent overhead are environmental, such as the physical machine architecture, CPU frequency, amount and speed of memory, system temperature, and resource contention. Other factors include virtualization and containerization, the operating system and its libraries, the JVM version and vendor, JVM settings, the algorithmic design of the software being monitored, and software dependencies.

Due to the complexity of modern software and the broad diversity in deployment scenarios, it is impossible to come up with a single agent overhead estimate. To find the overhead of any instrumentation agent in a given deployment, you have to conduct experiments and collect measurements directly. Therefore, all statements about performance must be treated as general information and guidelines that are subject to evaluation in a specific system.

The following sections describe the minimum requirements of the Splunk OTel Java agent, as well as potential constraints impacting performance, and guidelines to optimize and troubleshoot the performance of the agent.


.. _java-overhead-requirements:

Minimum requirements for production deployments
=================================================================



.. raw:: html

   <div class="include-start" id="requirements/java.rst"></div>

.. include:: /_includes/requirements/java.rst

.. raw:: html

   <div class="include-stop" id="requirements/java.rst"></div>





.. _java-overhead-guidelines:

Guidelines to reduce agent overhead
=================================================================

The following best practices and techniques might help in reducing overhead caused by the Java agent.

Configure trace sampling
-----------------------------------------------------------------

The volume of spans processed by the instrumentation might impact agent overhead. You can configure trace sampling to adjust the span volume and reduce resource usage. See :ref:`advanced-java-otel-configuration` for more information on sampling settings.

.. _turn-off-java-instrumentations:

Turn off specific instrumentations
-----------------------------------------------------------------

Consider turning off instrumentations that you don't need or are producing too many spans to further reduce agent overhead and span volume. To turn off an instrumentation, use ``-Dotel.instrumentation.<name>.enabled=false`` or the ``OTEL_INSTRUMENTATION_<NAME>_ENABLED`` environment variable, where ``<name>`` is the name of the instrumentation.

For example, the following option turns off the JDBC instrumentation: ``-Dotel.instrumentation.jdbc.enabled=false``

.. note:: Use Trace Analyzer in Splunk APM to explore the spans from your application and identify instrumentations you don't need. See :ref:`trace-analyzer` for more information.

Allocate more memory for the application
----------------------------------------------------------------

Increasing the maximum heap size of the JVM using the ``--Xmx<size>`` option might help in alleviating agent overhead issues, as instrumentations can generate a large number of short-lived objects in memory.

Reduce manual instrumentation to a minimum
----------------------------------------------------------------

Manual instrumentation might introduce inefficiencies that increase agent overhead. For example, using ``@WithSpan`` on every method results in a high span volume, which in turn increases noise in the data and consumes more system resources.

Provision adequate resources
----------------------------------------------------------------

Make sure to provision enough resources for your instrumentation and for the Collector. The amount of resources such as memory or disk depend on your application architecture and needs. For example, a common setup is to run the instrumented application on the same host as the Splunk Distribution of OpenTelemetry Collector. In that case, consider rightsizing the resources for the Collector and optimize its settings. See :ref:`otel-sizing`.


.. _java-overhead-constraints:

Constraints impacting the performance of the Java agent
=================================================================

In general, the more telemetry you collect from your application, the bigger is the impact on agent overhead. For example, tracing methods that aren't relevant to your application can still produce considerable agent overhead because tracing such methods is computationally more expensive than running the method itself. Similarly, high cardinality tags in metrics might increase memory usage. Debug logging, if turned on, also increases write operations to disk and memory usage.

Some features of the Java agent, like AlwaysOn Profiling, increase resource consumption because Java Flight Recorder (JFR) recordings require heap space and memory profiling relies on TLAB events that might increase agent overhead significantly when produced in high numbers. Some instrumentations, for example JDBC or Redis, produce high span volumes that increase agent overhead. For more information on how to turn off unnecessary instrumentations, see :ref:`turn-off-java-instrumentations`.

.. note:: Experimental features of the Java agent might increase agent overhead due to the experimental focus on functionality over performance. Stable features are safer in terms of agent overhead.


.. _java-overhead-troubleshooting:

Troubleshooting agent overhead issues
====================================================================

When troubleshooting agent overhead issues, do the following:

- Check minimum requirements. See :ref:`java-overhead-requirements`.
- Use the latest compatible version of the Java agent.
- Use the latest compatible version of your JVM.

Consider taking the following actions to decrease agent overhead:

- If your application is approaching memory limits, consider giving it more memory.
- If your application is using all the CPU, you might want to scale it horizontally.
- Try turning off or tuning CPU or memory profiling. See :ref:`profiling-configuration-java`.
- Try turning off or tuning metrics. See :ref:`advanced-java-otel-configuration`.
- Tune trace sampling settings to reduce span volume. See :ref:`advanced-java-otel-configuration`.
- Turn off specific instrumentations. See :ref:`turn-off-java-instrumentations`.
- Review manual instrumentation for unnecessary span generation.


.. _java-overhead-measure-diy:

Guidelines for measuring agent overhead
=================================================================

Measuring agent overhead in your own environment and deployments provides accurate data about the impact of instrumentation on the performance of your application or service. The following guidelines describe the general steps for collecting and comparing reliable agent overhead measurements.

Decide what you want to measure
-----------------------------------------------------------------

Different users of your application or service might notice different aspects of agent overhead. For example, while end users might notice degradation in service latency, power users with heavy workloads pay more attention to CPU overhead. On the other hand, users who deploy frequently, for example due to elastic workloads, care more about startup time.

Reduce your measurements to factors that are sure to impact the user experience of your application, so as not to produce datasets that contain irrelevant information. Some examples of measurements include the following:

- User average, user peak, and machine average CPU usage
- Total memory allocated and maximum heap used
- Garbage collection pause time
- Startup time in milliseconds
- Average and percentile 95 (p95) service latency
- Network read and write average throughput

Prepare a suitable test environment
-----------------------------------------------------------------

By measuring agent overhead in a controlled test environment you can better control and identify the factors affecting performance. When preparing a test environment, complete the following:

1. Make sure that the configuration of the test environment resembles production.
2. Isolate the application under test from other services that might interfere.
3. Turn off or remove all unnecessary system services on the application host.
4. Ensure that the application has enough system resources to handle the test workload.

Create a battery of realistic tests
-----------------------------------------------------------------

Design the tests that you run against the test environment to resemble typical workloads as much as possible. For example, if some REST API endpoints of your service are susceptible to high request volumes, create a test that simulates heavy network traffic.

For Java applications, use a warm-up phase prior to starting measurements. The JVM is a highly dynamic machine that performs a large number of optimizations through just-in-time compilation (JIT). The warm-up phase helps the application to finish most of its class loading and gives the JIT compiler time to run the majority of optimizations.

Make sure to run a large number of requests and to repeat the test pass many times. This repetition helps to ensure a representative data sample. Include error scenarios in your test data. Simulate an error rate similar to that of a normal workload, typically between 2% to 10%.

Collect comparable measurements
-----------------------------------------------------------------

To identify which factors might be affecting performance and causing agent overhead, collect measurements in the same environment after modifying a single factor or condition.

For example, you can take three different sets of measurements where the only difference is the presence and settings of the instrumentation:

- Condition A: No instrumentation or baseline
- Condition B: Instrumentation without AlwaysOn Profiling
- Condition C: Instrumentation with AlwaysOn Profiling

Analyze the agent overhead data
------------------------------------------------------------------

After collecting data from multiple passes, you can compare averages using simple statistical tests to check for significant differences, or plot results in a chart.

Consider that different stacks, applications, and environments might result in different operational characteristics and different agent overhead measurement results.



How to get support
=================================================================



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



