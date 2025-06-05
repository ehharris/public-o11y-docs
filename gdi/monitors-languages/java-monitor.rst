.. _java-monitor:

Java metrics (deprecated)
====================================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Java monitor. See benefits, install, configuration, and metrics

.. caution:: 
   
   This integration is deprecated and will reach End of Support on February 15th, 2025. During this period only critical security and bug fixes are provided. When End of Support is reached, the monitor will be removed and no longer be supported, and you won't be able to use it to send data to Splunk Observability Cloud. 

   To forward metrics from a Java application to Splunk Observability Cloud use the :ref:`Splunk Distribution of OpenTelemetry Java <get-started-java>` instead. To activate metrics collection in the OpenTelemetry Java agent, see :ref:`Activate metrics collection <enable_automatic_metric_collection>`.

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the ``java-monitor`` to retrieve metrics from a Java application.

This integration is available on Linux and Windows.

Benefits
--------



.. raw:: html

   <div class="include-start" id="benefits.rst"></div>

.. include:: /_includes/benefits.rst

.. raw:: html

   <div class="include-stop" id="benefits.rst"></div>




Installation
------------



.. raw:: html

   <div class="include-start" id="collector-installation.rst"></div>

.. include:: /_includes/collector-installation.rst

.. raw:: html

   <div class="include-stop" id="collector-installation.rst"></div>




Configuration
-------------



.. raw:: html

   <div class="include-start" id="configuration.rst"></div>

.. include:: /_includes/configuration.rst

.. raw:: html

   <div class="include-stop" id="configuration.rst"></div>




Example
~~~~~~~

To activate this integration, add the following to your Collector
configuration:

.. code-block:: yaml

   receivers:
     smartagent/java-monitor:
       type: java-monitor
       ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
     pipelines:
       metrics:
         receivers: [java-monitor]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for the
``java-monitor``:

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``host``
      - no
      - ``string``
      - Host is filled in by auto-discovery if the receiver has a
         discovery rule.
   - 

      - ``port``
      - no
      - ``integer``
      - Port is filled in by auto-discovery if the receiver has a
         discovery rule. Default value is ``0``.
   - 

      - ``jarFilePath``
      - no
      - ``string``
      - Path to the jar file that implements the monitoring logic.
   - 

      - ``javaBinary``
      - no
      - ``string``
      - By default, the agent use its bundled Java runtime (Java 8). If
         you wish to use a Java runtime that already exists on the
         system, specify the full path to the ``java`` binary here in
         ``/usr/bin/java``.
   - 

      - ``mainClass``
      - no
      - ``string``
      - The class within the specified ``jarFilePath`` that contains a
         main method to execute.
   - 

      - ``classPath``
      - no
      - ``list of strings``
      - Additional class paths to set on the invoked Java subprocess.
   - 

      - ``extraJavaArgs``
      - no
      - ``list of strings``
      - Additional flags to the Java subprocess

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/integrations/main/java/metrics.yaml"></div>


Notes
~~~~~



.. raw:: html

   <div class="include-start" id="metric-defs.rst"></div>

.. include:: /_includes/metric-defs.rst

.. raw:: html

   <div class="include-stop" id="metric-defs.rst"></div>




Troubleshooting
---------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



