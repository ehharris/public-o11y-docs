.. _spark:

Apache Spark
============

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Apache Spark clusters monitor. See benefits, install, configuration, and metrics

.. note:: If you're using the Splunk Distribution of the OpenTelemetry Collector and want to collect Apache Spark cluster metrics, use the native OTel component :ref:`apache-spark-receiver`.

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the Apache Spark monitor type to monitor Apache Spark clusters. It does not support fetching metrics from Spark Structured Streaming.

For the following cluster modes, the integration only supports HTTP
endpoints:

-  Standalone
-  Mesos
-  Hadoop YARN

This collectd plugin is not compatible with Kubernetes cluster mode. You need to select distinct monitor configurations and discovery rules for primary and worker processes. For the primary configuration, set ``isMaster`` to ``true``. When you run Apache Spark on Hadoop YARN, this integration can only report application metrics from the primary node.

This integration is only available on Linux.

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

   <div class="include-start" id="collector-installation-linux.rst"></div>

.. include:: /_includes/collector-installation-linux.rst

.. raw:: html

   <div class="include-stop" id="collector-installation-linux.rst"></div>




Configuration
-------------



.. raw:: html

   <div class="include-start" id="configuration.rst"></div>

.. include:: /_includes/configuration.rst

.. raw:: html

   <div class="include-stop" id="configuration.rst"></div>




Example
~~~~~~~

To activate this integration, add one of the following to your Collector
configuration:

.. code:: yaml

   receivers:
     smartagent/collectd_spark_master:
       type: collectd/spark
       ...  # Additional config

.. code:: yaml

   receivers:
     smartagent/collectd_spark_worker:
       type: collectd/spark
       ...  # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/collectd_spark_master]

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/collectd_spark_worker]

**Note:** The names ``collectd_spark_master`` and
``collectd_spark_worker`` are for identification purposes only and don't
affect functionality. You can use either name in your configuration, but
you need to select distinct monitor configurations and discovery rules
for primary and worker processes. For the primary configuration, see the
``isMaster`` field in the configuration settings section.

Configuration settings
----------------------

The following table shows the configuration options for this
integration:

.. list-table::
   :widths: 2 2 1 67
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``pythonBinary``
      - no
      - ``string``
      - This option specifies the path to a Python binary that executes
         the Python code. If you don't set this option, the system uses
         a built-in runtime. You can also include arguments to the
         binary.
   - 

      - ``host``
      - **yes**
      - ``string``
      - 
   - 

      - ``port``
      - **yes**
      - ``integer``
      - 
   - 

      - ``isMaster``
      - no
      - ``bool``
      - Set this option to ``true`` when you want to monitor a primary
         Spark node. The default is ``false``.
   - 

      - ``clusterType``
      - **yes**
      - ``string``
      - Set this option to the type of cluster you're monitoring. The
         allowed values are ``Standalone``, ``Mesos`` or ``Yarn``. The
         system doesn't collect cluster metrics for Yarn. Use the
         collectd/hadoop monitor to gain insights to your cluster's
         health.
   - 

      - ``collectApplicationMetrics``
      - no
      - ``bool``
      - The default is ``false``.
   - 

      - ``enhancedMetrics``
      - no
      - ``bool``
      - The default is ``false``.

Metrics
-------

These are the metrics available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/collectd/spark/metadata.yaml"></div>  


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



