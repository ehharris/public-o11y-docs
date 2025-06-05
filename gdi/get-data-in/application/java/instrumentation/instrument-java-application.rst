.. _instrument-java-applications:

***************************************************************************
Instrument your Java application for Splunk Observability Cloud
***************************************************************************

.. meta::
   :description: Start sending metrics and log telemetry to Splunk Observability Cloud using the Splunk OpenTelemetry Java agent to automatically instrument your Java application or service. Follow these steps to get started.

The Java agent from the Splunk Distribution of OpenTelemetry Java can automatically instrument your Java application by injecting instrumentation to Java classes. To get started, use the guided setup or follow the instructions manually.



.. raw:: html

   <div class="include-start" id="zero-code-info.rst"></div>

.. include:: /_includes/zero-code-info.rst

.. raw:: html

   <div class="include-stop" id="zero-code-info.rst"></div>




Generate customized instructions using the guided setup
====================================================================

To generate all the basic installation commands for your environment and application, use the Java guided setup. To access the Java guided setup, follow these steps:

#. Log in to Splunk Observability Cloud.
#. Open the :new-page:`Java guided setup <https://login.signalfx.com/#/gdi/scripted/java-tracing/step-1?gdiState=%7B"integrationId":"java-tracing"%7D>`. Optionally, you can navigate to the guided setup on your own:

   #. In the navigation menu, select :menuselection:`Data Management`.

   #. Go to the :guilabel:`Available integrations` tab, or select :guilabel:`Add Integration` in the :guilabel:`Deployed integrations` tab.

   #. In the integration filter menu, select :guilabel:`By Product`.

   #. Select the :guilabel:`APM` product.

   #. Select the :guilabel:`Java` tile to open the Java guided setup.

Install the Splunk Distribution of OpenTelemetry Java manually
==================================================================

If you don't use the guided setup, follow these instructions to manually install the Splunk Distribution of OpenTelemetry Java:

- :ref:`install-enable-jvm-agent`
   - :ref:`enable_profiling_java`
   - :ref:`enable_automatic_metric_collection`
   - :ref:`ignore_endpoints_java`
- :ref:`configure-java-instrumentation`

.. _install-enable-jvm-agent:

Install and activate the Java agent
-----------------------------------------------------------

Follow these steps to automatically instrument your application using the Java agent:

#. Check that you meet the requirements. See :ref:`java-otel-requirements`.

#. Download the JAR file for the latest version of the agent:

   .. tabs::

      .. code-tab:: shell Linux

         curl -L https://github.com/signalfx/splunk-otel-java/releases/latest/download/splunk-otel-javaagent.jar \
         -o splunk-otel-javaagent.jar

      .. code-tab:: shell Windows PowerShell

         Invoke-WebRequest -Uri https://github.com/signalfx/splunk-otel-java/releases/latest/download/splunk-otel-javaagent.jar -OutFile splunk-otel-javaagent.jar

#. Set the ``OTEL_SERVICE_NAME`` environment variable:

   .. tabs::

      .. code-tab:: shell Linux

         export OTEL_SERVICE_NAME=<yourServiceName>

      .. code-tab:: shell Windows PowerShell

         $env:OTEL_SERVICE_NAME=<yourServiceName>

#. (Optional) Set the endpoint URL if the Splunk Distribution of OpenTelemetry Collector is running on a different host:

   .. tabs::

      .. code-tab:: shell Linux

         export OTEL_EXPORTER_OTLP_ENDPOINT=<yourCollectorEndpoint>:<yourCollectorPort>

      .. code-tab:: shell Windows PowerShell

         $env:OTEL_EXPORTER_OTLP_ENDPOINT=<yourCollectorEndpoint>:<yourCollectorPort>

#. (Optional) Set the deployment environment and service version:

   .. tabs::

      .. code-tab:: bash Linux

         export OTEL_RESOURCE_ATTRIBUTES='deployment.environment=<envtype>,service.version=<version>'

      .. code-tab:: shell Windows PowerShell

         $env:OTEL_RESOURCE_ATTRIBUTES='deployment.environment=<envtype>,service.version=<version>'

#. Set the ``-javaagent`` argument to the path of the Java agent:

   .. code-block:: bash
      :emphasize-lines: 1

      java -javaagent:./splunk-otel-javaagent.jar -jar <myapp>.jar

   .. note:: If your application runs on a supported Java server, see :ref:`java-servers-instructions`.

If no data appears in APM, see :ref:`common-java-troubleshooting`.

If you need to add custom attributes to spans or want to manually generate spans, instrument your Java application or service manually. See :ref:`java-manual-instrumentation`.

.. _enable_profiling_java:

Activate AlwaysOn Profiling
-------------------------------------

To activate AlwaysOn Profiling, use the following system property argument. You can also use the ``SPLUNK_PROFILER_ENABLED`` environment variable. For more information, see :ref:`profiling-intro`.

To activate memory profiling, set the ``splunk.profiler.memory.enabled`` system property or the ``SPLUNK_PROFILER_MEMORY_ENABLED`` environment variable to ``true`` after activating AlwaysOn Profiling.

.. note:: OpenJDK versions 15.0 to 17.0.8 are not supported for memory profiling. See :new-page:`https://bugs.openjdk.org/browse/JDK-8309862` in the JDK bug system for more information. 

The following example shows how to activate the profiler using the system property:

.. code-block:: bash
   :emphasize-lines: 2,3,4,5

   java -javaagent:./splunk-otel-javaagent.jar \
   -Dsplunk.profiler.enabled=true \
   -Dsplunk.profiler.memory.enabled=true \
   -Dotel.exporter.otlp.endpoint=http(s)://collector:4318 \
   -jar <your_application>.jar

See :ref:`get-data-in-profiling` for more information. For more settings, see :ref:`profiling-configuration-java`.

.. _enable_automatic_metric_collection:

Metrics collection
---------------------------------------

Starting from version 2.5.0, the Java agent collects metrics by default when instrumenting an application or service automatically. To migrate metric collection from version 1.x to 2.x, see :ref:`java-metrics-migration-guide`.

If your metrics endpoint is different than the default value, set the ``OTEL_EXPORTER_OTLP_METRICS_ENDPOINT`` environment variable. See :ref:`advanced-java-otel-configuration` for more information.

If you activate memory profiling, metrics collection is activated automatically and cannot be deactivated.

.. note:: Metrics ingest might result in increased data ingest costs. To deactivate metrics collection, set the ``OTEL_METRICS_EXPORTER`` environment variable or the ``-Dotel.metrics.exporter`` property to ``none``.

.. _enable_automatic_logs_collection:

Logs collection
---------------------------------------

By default, the Java agent injects trace and span metadata automatically into logs. The agent then sends the annotated logs to the OpenTelemetry Collector, which exports them to Splunk Observability Cloud. For more information on trace-log correlation, see :ref:`correlate-traces-with-logs-java`.

.. note:: Logs ingest might result in increased data ingest costs. To deactivate logs collection, set the ``OTEL_LOGS_EXPORTER`` environment variable or the ``-Dotel.logs.exporter`` property to ``none``.


.. _ignore_endpoints_java:

Ignore specific endpoints
------------------------------------------

By default, the Java agent collects traces from all the endpoints of your application. To ignore specific endpoints, use the ``rules`` sampler and define ``drop`` rules.

In the following example, the sampler drops all ``SERVER`` spans whose endpoints match ``healtcheck``, and sends the rest of spans using the fallback sampler, ``parentbased_always_on``:

.. tabs::

   .. code-tab:: bash Linux

      export OTEL_TRACES_SAMPLER=rules
      export OTEL_TRACES_SAMPLER_ARG=drop=/healthcheck;fallback=parentbased_always_on

   .. code-tab:: shell Windows PowerShell

      $env:OTEL_TRACES_SAMPLER=rules
      $env:OTEL_TRACES_SAMPLER_ARG=drop=/healthcheck;fallback=parentbased_always_on

See :ref:`advanced-java-otel-configuration` for more information.

.. _configure-java-instrumentation:

Configure the Java agent
-----------------------------------------------------------

You can configure the agent using environment variables or by setting system properties as runtime arguments. For more details about both methods, see :ref:`configuration-methods-java`.

For advanced configuration of the JVM agent, like changing trace propagation formats, correlating traces and logs, or activating custom sampling, see :ref:`advanced-java-otel-configuration`.

.. _kubernetes_java_agent:

Deploy the Java agent in Kubernetes
==================================================================

To deploy the Java agent in Kubernetes, follow these steps:

#. Edit the Dockerfile for your application image to add the following commands:

   .. code-block:: docker

      # Adds the latest version of the Splunk Java agent
      ADD https://github.com/signalfx/splunk-otel-java/releases/latest/download/splunk-otel-javaagent.jar .
      # Modifies the entry point
      ENTRYPOINT ["java","-javaagent:splunk-otel-javaagent.jar","-jar","./<myapp>.jar"]

#. Configure the Kubernetes Downward API to expose environment variables to Kubernetes resources.

   The following example shows how to update a deployment to expose environment variables by adding the agent configuration under the ``.spec.template.spec.containers.env`` section:

   .. code-block:: yaml

      apiVersion: apps/v1
      kind: Deployment
      spec:
      selector:
         matchLabels:
            app: your-application
      template:
         spec:
            containers:
            - name: myapp
               env:
                  - name: SPLUNK_OTEL_AGENT
                  valueFrom:
                     fieldRef:
                        fieldPath: status.hostIP
                  - name: OTEL_EXPORTER_OTLP_ENDPOINT
                  value: "http://$(SPLUNK_OTEL_AGENT):4318"
                  - name: OTEL_SERVICE_NAME
                  value: "<serviceName>"
                  - name: OTEL_RESOURCE_ATTRIBUTES
                  value: "deployment.environment=<environmentName>"

.. note:: You can also deploy instrumentation using the Kubernetes Operator. See :ref:`k8s-backend-auto-discovery` for more information.

.. _java-agent-cloudfoundry:

Deploy the Java agent in Cloudfoundry
=======================================================

To instrument a Java application in Cloudfoundry, use the Splunk OpenTelemetry Java Agent buildpack framework. The framework automatically instruments your application to send traces to Splunk Observability Cloud.

See :new-page:`https://github.com/cloudfoundry/java-buildpack/blob/main/docs/framework-splunk_otel_java_agent.md <https://github.com/cloudfoundry/java-buildpack/blob/main/docs/framework-splunk_otel_java_agent.md>` for instructions.

.. _export-directly-to-olly-cloud-java:

Send data directly to Splunk Observability Cloud
=======================================================

By default, the agent sends all telemetry to the local instance of the Splunk Distribution of OpenTelemetry Collector.

If you need to send data directly to Splunk Observability Cloud, set the following environment variables:

.. tabs::

   .. code-tab:: bash Linux

      export SPLUNK_ACCESS_TOKEN=<access_token>
      export OTEL_EXPORTER_OTLP_TRACES_PROTOCOL=http/protobuf
      export OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=https://ingest.<realm>.signalfx.com/v2/trace/otlp
      export OTEL_LOGS_EXPORTER=none

   .. code-tab:: shell Windows PowerShell

      $env:SPLUNK_ACCESS_TOKEN=<access_token>
      $env:OTEL_EXPORTER_OTLP_TRACES_PROTOCOL=http/protobuf
      $env:OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=https://ingest.<realm>.signalfx.com/v2/trace/otlp
      $env:OTEL_LOGS_EXPORTER=none

To obtain an access token, see :ref:`admin-api-access-tokens`.

To find your Splunk realm, see :ref:`Note about realms <about-realms>`.

For more information on the ingest API endpoints, see :new-page:`Send APM traces <https://dev.splunk.com/observability/docs/apm/send_traces/>`.

.. caution:: This procedure applies to spans and traces. To send AlwaysOn Profiling data, you must use the OTel Collector.

Set the source host 
-----------------------------------------------------------



.. raw:: html

   <div class="include-start" id="gdi/apm-api-define-host.rst"></div>

.. include:: /_includes/gdi/apm-api-define-host.rst

.. raw:: html

   <div class="include-stop" id="gdi/apm-api-define-host.rst"></div>





.. _instrument_aws_lambda_functions:

Instrument Lambda functions
==========================================================

You can instrument AWS Lambda functions using the Splunk OpenTelemetry Lambda Layer. See :ref:`instrument-aws-lambda-functions` for more information.

.. _upgrade-java-instrumentation:

Upgrade the Splunk Distribution of OpenTelemetry Java
============================================================

New releases of the Splunk Distribution of OpenTelemetry Java happen after a new upstream release, or when new features and enhancements are available.

Upgrade to each new version of the Splunk Distribution of OpenTelemetry Java after it's released. To find out about new releases, watch the GitHub repository at :new-page:`https://github.com/signalfx/splunk-otel-java/releases <https://github.com/signalfx/splunk-otel-java/releases>`

.. note:: See the :new-page:`versioning document <https://github.com/signalfx/splunk-otel-java/blob/main/VERSIONING.md>` in GitHub to learn more about version numbers. Major versions contain a large number of changes, which might result in increased risk to your production environment. Minor version changes indicate common releases, which contain a modest number of changes Patch releases are infrequent and contain specific fixes or enhancements.

Best practices for upgrades
-------------------------------------

To reduce the risk of issues with an upgrade, do the following:

- See the release notes and changelog for each release to determine if the release has changes that might affect your environment. Pay attention to mentions of libraries, frameworks, and tools that your software uses.
- Never put untested code into production. Verify that the new build works in a staging or pre-production environment before promoting it to production. Don't use snapshot builds in production.
- Use canary instances. Let the canaries operate with the code before releasing the code to production. Run the canaries for at least a few hours, and preferably for a few days.
- Minimize the number of dependencies, including instrumentation, that change in a given release. Determining the root cause of a problem after upgrading multiple dependencies at the same time can be difficult.
- Pin version numbers in your build pipeline. Don't use the ``latest`` URL in automated processes.
