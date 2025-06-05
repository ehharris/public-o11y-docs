.. _logstash-tcp:

Logstash TCP
============

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Logstash TCP monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of OpenTelemetry Collector uses the Smart Agent receiver with the
``logstash-tcp`` monitor type to monitor the health and performance of
Logstash deployments through Logstash Monitoring APIs. It fetches events
from the logstash tcp output plugin operating in either ``server`` or
``client`` mode and converts them to data points.

This integration is meant to be used in conjunction with the Logstash
Metrics filter plugin that turns events into metrics. You can only use
autodiscovery when this monitor is in ``client`` mode.

This receiver is available on Linux and Windows.

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




### Example

To activate this integration, add the following to your Collector
configuration:

.. code-block:: yaml

   receivers:
     smartagent/logstash-tcp:
       type: logstash-tcp
       ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/logstash-tcp]

Example: Logstash Metrics plugin configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following example shows how to use both ``timer`` and ``meter``
metrics from the Logstash Metrics filter plugin:

.. code-block:: yaml

   input {
     file {
       path => "/var/log/auth.log"
       start_position => "beginning"
       tags => ["auth_log"]
     }

     # A contrived file that contains timing messages
     file {
       path => "/var/log/durations.log"
       tags => ["duration_log"]
       start_position => "beginning"
     }
   }

   filter {
     if "duration_log" in [tags] {
       dissect {
         mapping => {
           "message" => "Processing took %{duration} seconds"
         }
         convert_datatype => {
           "duration" => "float"
         }
       }
       if "_dissectfailure" not in [tags] { # Filter out bad events
         metrics {
           timer => { "process_time" => "%{duration}" }
           flush_interval => 10
           # This makes the timing stats pertain to only the previous 5 minutes
           # instead of since Logstash last started.
           clear_interval => 300
           add_field => {"type" => "processing"}
           add_tag => "metric"
         }
       }
     }
     # Count the number of logins using SSH from /var/log/auth.log
     if "auth_log" in [tags] and [message] =~ /sshd.*session opened/ {
       metrics {
         # This determines how often metric events will be sent to the agent, and
         # thus how often data points will be emitted.
         flush_interval => 10
         # The name of the meter will be used to construct the name of the metric
         # in Splunk Infrastructure Monitoring. For this example, a data point called `logins.count` would
         # be generated.
         meter => "logins"
         add_tag => "metric"
       }
     }
   }

   output {
     # This can be helpful to debug
     stdout { codec => rubydebug }

     if "metric" in [tags] {
       tcp {
         port => 8900
         # The agent will connect to Logstash
         mode => "server"
         # Needs to be "0.0.0.0" if running in a container.
         host => "127.0.0.1"
       }
     }
   }

Once Logstash is configured with the above configuration, the
``logstash-tcp`` monitor collects ``logins.count`` and
``process_time.<timer_field>``.

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

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

      - ``host``
      - **yes**
      - ``string``
      - If ``mode: server``, the local IP address to listen on. If
         ``mode: client``, the Logstash host/ip to connect to.
   - 

      - ``port``
      - no
      - ``integer``
      - If ``mode: server``, the local port to listen on. If
         ``mode: client``, the port of the Logstash TCP output plugin.
         If port is ``0``, a random listening port is assigned by the
         kernel. (**default:** ``0``)
   - 

      - ``mode``
      - no
      - ``string``
      - Whether to act as a ``server`` or ``client``. The corresponding
         setting in the Logtash ``tcp`` output plugin should be set to
         the opposite of this. (**default:** ``client``)
   - 

      - ``desiredTimerFields``
      - no
      - ``list of strings``
      - (**default:** ``[mean, max, p99, count]``)
   - 

      - ``reconnectDelay``
      - no
      - ``integer``
      - How long to wait before reconnecting if the TCP connection
         cannot be made or after it gets broken. (**default:** ``5s``)
   - 

      - ``debugEvents``
      - no
      - ``bool``
      - If ``true``, events received from Logstash will be dumped to the
         agent's stdout in deserialized form (**default:** ``false``)

Metrics
-------

There are no metrics available for this integration.

Troubleshooting
---------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



