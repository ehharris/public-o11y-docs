.. _nagios:

Nagios (deprecated)
==============================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the nagios monitor. See benefits, install, configuration, and metrics

.. caution:: 
   
   The Nagios monitor is now deprecated and will reach of End of Support on October 31st, 2024. During this period only critical security and bug fixes are provided. When End of Support is reached, the monitor will be removed and no longer be supported, and you won't be able to use it to send data to Splunk Observability Cloud. 

   To monitor your system you can use native OpenTelemetry components instead. See more at :ref:`otel-components`. 

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the
Nagios monitor type to run existing Nagios status check scripts through
the Collector, which acts as the Nagios Remote Plugin Executor (NRPE) or
the Simple Network Management Protocol (SNMP) exec directive, and send
the state of the check, depending on the exit code of the command.

This integration is similar to the telegraf/exec monitor configured with
dataFormat:nagios integration, with the following exceptions:

-  It does not retrieve perfdata metrics. This integration only
   retrieves the state of the script for alerting purposes.
-  It overrides the state if the exit code ``== 0``, but the output
   string starts with ``warn``, ``crit``, or ``unkn`` (not
   case-sensitive).

This integration adds more context to the status check state by using
events. In addition to the ``state`` metric, it also sends an event that
includes the output and the error caught from the command execution.

Using this integration should make troubleshooting more efficient and
let you remain in Splunk Observability Cloud without connecting to your
Linux or Windows machine in case of an abnormal state to understand what
is happening. Using this integration also lets you create a dashboard
that is familiar to Nagios users.

.. note:: The last sent event is cached into memory and compared to new events to avoid repeatedly sending the same event for each collection interval. Restarting the OTel Collector erases its cache, so the  most recently sent event is sent again upon restart. If your check always “normally” produces a different output for each run, for example, the uptime check, you can use the ``FilterStdOut: true`` parameter to ignore it in comparison.

This integration is available on Kubernetes, Linux, and Windows.

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
     smartagent/nagios:
       type: nagios
       command: <command>
       service: <service>
       timeout: 7 #9 by default
       ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/nagios]

Event-sending functionality
~~~~~~~~~~~~~~~~~~~~~~~~~~~



.. raw:: html

   <div class="include-start" id="event-sending-functionality.rst"></div>

.. include:: /_includes/event-sending-functionality.rst

.. raw:: html

   <div class="include-stop" id="event-sending-functionality.rst"></div>




Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for this monitor:

.. list-table::
   :widths: 5 3 3 60
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``command``
      - **yes**
      - ``string``
      - The command to exec with any arguments like:
         ``"LC_ALL=\"en_US.utf8\" /usr/lib/nagios/plugins/check_ntp_time -H pool.ntp.typhon.net -w 0.5 -c 1"``
   - 

      - ``service``
      - **yes**
      - ``string``
      - Corresponds to the nagios ``service`` column and allows to
         aggregate all instances of the same service (when calling the
         same check script with different arguments)
   - 

      - ``timeout``
      - no
      - ``integer``
      - The max execution time allowed in seconds before sending SIGKILL
         (**default:** ``9``)
   - 

      - ``ignoreStdOut``
      - no
      - ``bool``
      - If ``false`` and change is detected on ``stdout`` compared to
         the last event it will send a new one (**default:** ``false``)
   - 

      - ``ignoreStdErr``
      - no
      - ``bool``
      - If ``false`` and change is detected on ``stderr`` compared to
         the last event it will send a new one (**default:** ``false``)

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/nagios/metadata.yaml"></div>

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



