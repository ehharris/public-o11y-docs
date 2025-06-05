.. _telegraf-logparser:

Logparser
=========

.. meta::
   
   :description: Use this Splunk Observability Cloud integration for the telegraf/logparser plugin monitor. See benefits, install, configuration, and metrics.

.. caution:: Smart Agent monitors are being deprecated. To tail log files use the OpenTelemetry Collector and the :new-page:`Telegraf Tail Input plugin <https://github.com/influxdata/telegraf/blob/master/plugins/inputs/tail/README.md>`. See how in :ref:`telegraf-generic`.

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the ``telegraf/logparser`` monitor type to tail log files.

This integration is based on the Telegraf logparser plugin, and all
emitted metrics have the plugin dimension set to ``telegraf-logparser``.

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

.. code:: yaml

   receivers:
     smartagent/logparser:
       type: telegraf/logparser
       ...  # Additional config

To complete the integration, include this monitor type as a member of a
``logs`` pipeline. Use the SignalFx exporter to make event submission
requests. Use the Resource Detection processor to ensure that host
identity and other useful information is made available as event
dimensions. For example:

.. code:: yaml

   service:
     pipelines:
       logs:
         receivers:
           - smartagent/logparser
         processors:
           - resourcedetection
         exporters:
           - signalfx

The following example shows a sample YAML configuration:

.. code:: yaml

   receivers:
     smartagent/logparser:
       type: telegraf/logparser
       files:
        - '$file'
       watchMethod: poll
       # Specifies the file watch method ("inotify" or "poll").
       fromBeginning: true     
       # Specifies to read from the beginning.
       measurementName: test-measurement 
       # This is the metric name prefix.
       patterns:
        - "%{COMMON_LOG_FORMAT}" 
       # Specifies the Apache Common Log Format (CLF).
       timezone: UTC

Configuration options
~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for this monitor
type:

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``files``
      - **yes**
      - ``list of strings``
      - Paths to files to be tailed
   - 

      - ``watchMethod``
      - no
      - ``string``
      - Method for watching changes to files (“inotify” or “poll”). The
         default value is ``poll``.
   - 

      - ``fromBeginning``
      - no
      - ``bool``
      - Whether to start tailing from the beginning of the file. The
         default value is ``false``.
   - 

      - ``measurementName``
      - no
      - ``string``
      - Name of the measurement
   - 

      - ``patterns``
      - no
      - ``list of strings``
      - A list of patterns to match.
   - 

      - ``namedPatterns``
      - no
      - ``list of strings``
      - A list of named grok patterns to match.
   - 

      - ``customPatterns``
      - no
      - ``string``
      - Custom grok patterns. (``grok`` only)
   - 

      - ``customPatternFiles``
      - no
      - ``list of strings``
      - List of paths to custom grok pattern files.
   - 

      - ``timezone``
      - no
      - ``string``
      - Specifies the timezone. The default is UTC time. Other options
         are ``Local`` for the local time on the machine, ``UTC``, and
         ``Canada/Eastern`` (unix style timezones).

Metrics
-------

The Splunk Distribution of OpenTelemetry Collector does not do any
built-in filtering of metrics coming out of this monitor type.

Troubleshooting
---------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



