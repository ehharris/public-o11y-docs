.. _redis:

Redis (deprecated)
=======================================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Redis monitor. See benefits, install, configuration, and metrics

.. note:: The Redis monitor is deprecated and will reach end of support on January 15, 2025. During this period, only critical security and bug fixes are provided. When the monitor reaches end of support, you won't be able to use it to send data to Splunk Observability Cloud.

   To monitor your Redis databases, you can instead use the native OpenTelemetry Redis receiver. See :ref:`redis-receiver` to learn more.

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the ``redis`` monitor type to capture the following metrics:

-  Memory used
-  Commands processed per second
-  Number of connected clients and followers
-  Number of blocked clients
-  Number of keys stored per database
-  Uptime
-  Changes since last save
-  Replication delay per follower

It accepts endpoints and allows multiple instances.

This integration is available on Kubernetes and Linux, and supports
Redis 2.8 and higher.

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

.. code:: yaml

   receivers:
     smartagent/redis:
       type: collectd/redis
       ...  # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/redis]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for the Redis
integration:

.. list-table::
   :widths: 6 3 11 52
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``host``
      - Yes
      - ``string``
      - 
   - 

      - ``port``
      - Yes
      - ``integer``
      - 
   - 

      - ``pythonBinary``
      - No
      - ``string``
      - Path to the Python binary. If you don't provide a path, the
         monitor uses its built-in runtime. The string can include
         arguments to the binary.
   - 

      - ``name``
      - No
      - ``string``
      - Name for the Redis instance. The maximum length is 64
         characters. The default value is “{host}:{port}”.
   - 

      - ``auth``
      - No
      - ``string``
      - Authentication
   - 

      - ``sendListLengths``
      - No
      - ``list of objects (see below)``
      - List of keys that you want to monitor for length. To learn more,
         see the section **Monitor the length of Redis lists**.
   - 

      - ``verbose``
      - No
      - ``bool``
      - Flag that controls verbose logging for the plugin. If ``true``,
         verbose logging is activated. The default value is\ ``false``.

The following table shows you the configuration options for the
``sendListLengths`` configuration object:

.. list-table::
   :widths: 4 2 2 63
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``databaseIndex``
      - Yes
      - ``integer``
      - The database index
   - 

      - ``keyPattern``
      - Yes
      - ``string``
      - A string or pattern to use for selecting keys. A string selects
         a single key. A pattern that uses ``*`` as a ``glob`` style
         wildcard processes all keys that match the pattern. Surround a
         pattern with single quotes ('), for example ``'mylist*'``

Monitor the length of Redis lists
---------------------------------

To monitor the length of list keys, you must specify the key and
database index in the configuration using the following syntax:

.. code-block:: yaml

   sendListLengths: [{databaseIndex: $db_index, keyPattern: "$key_name"}]

You can specify ``$key_name`` as a glob-style pattern. The only
supported wildcard is ``*`` . When you use a pattern, the configuration
processes all keys that match the pattern.

To ensure that the ``*`` is interpreted correctly, surround the pattern
with double quotes (``""``). When a nonlist key matches the pattern, the
Redis monitor writes an error to the agent logs.

in Splunk Observability Cloud, ``gauge.key_llen`` is the metric name for
Redis list key lengths. Splunk Observability Cloud creates a separate
MTS for each Redis list.

**Notes**:

1. The Redis monitor uses the ``KEYS`` command to match patterns.
   Because this command isn't optimized, you need to keep your match
   patterns small. Otherwise, the command can block other commands from
   executing.
2. To avoid duplicate reporting, choose a single node in which to
   monitor list lengths. You can use the main node configuration or a
   follower node configuration.

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/collectd/redis/metadata.yaml"></div>

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




Database Query Performance
~~~~~~~~~~~~~~~~~~~~~~~~~~

You can troubleshoot Redis command performance issues using Database
Query Performance in Splunk APM.

-  For a sample scenario, see :ref:`redis-scenario`
-  For more information on Database Query Performance support for Redis,
   see :ref:`redis-db-query-performance`
