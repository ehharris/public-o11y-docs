.. _logging-exporter:

Logging exporter
**************************

.. meta::
      :description: Use the logging exporter to write traces, metrics, and logs to the console. Read on to learn how to configure the component.


.. note:: The Logging exporter will be deprecated in September 2024 in favor of the Debug exporter. For more information, see the GitHub repo and docs at :new-page:`Debug exporter <https://github.com/open-telemetry/opentelemetry-collector/blob/main/exporter/debugexporter/README.md>`.

The logging exporter allows the OpenTelemetry Collector to send traces, metrics, and logs directly to the console. The supported pipeline types are ``traces``, ``metrics``, and ``logs``. See :ref:`otel-data-processing` for more information.

Use the logging exporter to diagnose and troubleshoot issues with telemetry received and processed by the OpenTelemetry Collector, or to obtain samples for other purposes.

Get started
======================

.. note:: The Logging exporter sends pipeline activity to the console as logs. To control the verbosity of the OpenTelemetry Collector itself, use the ``service.logger`` setting.

By default, the Splunk Distribution of OpenTelemetry Collector includes the logging exporter with verbosity set to ``detailed`` when deploying in host monitoring (agent) mode. See :ref:`otel-deployment-mode` for more information.

.. code:: yaml

   exporters:
      # ...
      # Other exporters
      # ...
      logging:
         # loglevel is deprecated; use verbosity instead
         # Available levels are "basic", "normal", and "detailed"
         verbosity: detailed

To activate the logging exporter, add it to any pipeline you want to diagnose. For example:

.. code-block:: yaml

   :emphasize-lines: 9, 13, 20

   service:
     pipelines:
       traces:
         receivers: [jaeger, otlp, zipkin]
         processors:
         - memory_limiter
         - batch
         - resourcedetection
         exporters: [otlphttp, signalfx, logging]
       metrics:
         receivers: [hostmetrics, otlp, signalfx]
         processors: [memory_limiter, batch, resourcedetection]
         exporters: [signalfx, logging]
       logs:
         receivers: [fluentforward, otlp]
         processors:
         - memory_limiter
         - batch
         - resourcedetection
         exporters: [splunk_hec, logging]

Available verbosity levels are ``basic``, ``normal``, and ``detailed``. The correspondence between verbosity levels and log levels is the following:

.. list-table::
   :header-rows: 1
   :widths: 50 50
   :width: 100%

   * - Verbosity level
     - Log level (Deprecated)
   * - ``basic``
     - ``warn``, ``error``, ``panic``, ``fatal``
   * - ``normal``
     - ``info``
   * - ``detailed``
     - ``debug``
         
.. note:: The ``detailed`` verbosity level might increase resource consumption on the host. Deactivate the logging exporter after you've obtained sufficient samples.

Review collected logs
--------------------------

To review logs produced by the logging exporter, run the following command:

.. code-block:: bash

   journalctl -u splunk-otel-collector.service -f

Sample configurations
----------------------

The following example shows a logging exporter with detailed verbosity, which is equivalent to a ``debug`` log level. Initial sampling is five messages logged each second, logging every 200 messages after the initial sample. 

.. code:: yaml

   exporters:
     logging:
       verbosity: detailed
       sampling_initial: 5
       sampling_thereafter: 200

Settings
======================

The following table shows the configuration options for the logging exporter:

.. raw:: html

   <div class="metrics-standard" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/cfg-metadata/exporter/logging.yaml"></div>

Troubleshooting
======================



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



