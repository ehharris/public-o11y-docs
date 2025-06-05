.. _zookeeper:

Apache Zookeeper
================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Apache Zookeeper monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of OpenTelemetry Collector uses the Smart Agent receiver with the
Apache Zookeeper monitor type to keep track of an Apache Zookeeper
instance using the Zookeeper plugin.

This integration is only available on Kubernetes and Linux.

The plugin supports Zookeeper 3.4.0 and higher.

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

To activate this integration, add the following to your Collector
configuration:

.. code-block:: yaml

   receivers:
   smartagent/zookeeper:
   type: collectd/zookeeper
   ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/zookeeper]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for the Apache
Zookeeper monitor:

.. list-table::
   :widths: 4 4 3 60
   :header-rows: 1

   - 

      - **Option**
      - **Required**
      - **Type**
      - **Description**
   - 

      - ``pythonBinary``
      - no
      - ``string``
      - This string contains a path to a python binary that you want to
         use to execute the Python code. If you don't set it, the system
         uses a built-in runtime will be used. The string can include
         arguments to the binary.
   - 

      - ``host``
      - **yes**
      - ``string``
      - This string specifies the host or IP address of the Apache
         Zookeeper node.
   - 

      - ``port``
      - **yes**
      - ``integer``
      - This value specifies the main port of the Zookeeper node.
   - 

      - ``name``
      - no
      - ``string``
      - If you provide this string, the system inserts it as the value
         of the ``plugin_instance`` dimension for emitted

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/collectd/zookeeper/metadata.yaml"></div>


Notes
~~~~~



.. raw:: html

   <div class="include-start" id="metric-defs.rst"></div>

.. include:: /_includes/metric-defs.rst

.. raw:: html

   <div class="include-stop" id="metric-defs.rst"></div>




Group leader metrics
~~~~~~~~~~~~~~~~~~~~

All of the following metrics are part of the ``leader`` metric group. To
activate them, add ``leader`` to the ``extraGroups`` setting:

-  ``gauge.zk_followers``
-  ``gauge.zk_pending_syncs``
-  ``gauge.zk_synced_followers``

Troubleshooting
---------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



