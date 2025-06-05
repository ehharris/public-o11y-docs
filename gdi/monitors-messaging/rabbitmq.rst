.. _rabbitmq:

RabbitMQ
========

.. meta::
   :description: Use this Splunk Observability Cloud integration for the RabbitMQ monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the ``rabbitmq`` monitor type to keep track of an instance of RabbitMQ.

.. note:: To monitor RabbitMQ instances with the OpenTelemetry Collector using native OpenTelemetry refer to the :ref:`rabbitmq-receiver` component.

The integration uses the RabbitMQ Python plugin and the RabbitMQ
Management HTTP API to poll for statistics on a RabbitMQ server, then
reports them to the agent.

This integration is available on Kubernetes and Linux, and requires
RabbitMQ 3.0 and higher.

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
     smartagent/rabbitmq:
       type: collectd/rabbitmq
       ...  # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/rabbitmq]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for the RabbitMQ
monitor:

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``pythonBinary``
      - No
      - ``string``
      - Path to the Python binary. If not set, a built-in runtime is
         used. This setting can include arguments to the binary.
   - 

      - ``host``
      - Yes
      - ``string``
      - Hostname or IP address of the RabbitMQ instance. For example,
         ``127.0.0.1``.
   - 

      - ``port``
      - Yes
      - ``integer``
      - The port of the RabbitMQ instance. For example, ``15672``.
   - 

      - ``brokerName``
      - No
      - ``string``
      - Name of the RabbitMQ instance. Can be a Go template using other
         configuration options. Used as the ``plugin_instance``
         dimension. The default value is ``{{.host}}-{{.port}}``.
   - 

      - ``collectChannels``
      - No
      - ``bool``
      - Whether to collect channels. The default value is\ ``false``.
   - 

      - ``collectConnections``
      - No
      - ``bool``
      - Whether to collect connections. The default value is\ ``false``.
   - 

      - ``collectExchanges``
      - No
      - ``bool``
      - Whether to collect exchanges. The default value is\ ``false``.
   - 

      - ``collectNodes``
      - No
      - ``bool``
      - Whether to collect nodes. The default value is\ ``false``.
   - 

      - ``collectQueues``
      - No
      - ``bool``
      - Whether to collect queues. The default value is\ ``false``.
   - 

      - ``httpTimeout``
      - No
      - ``integer``
      - HTTP timeout for requests.
   - 

      - ``verbosityLevel``
      - No
      - ``string``
      - Verbosity level.
   - 

      - ``username``
      - Yes
      - ``string``
      - API username of the RabbitMQ instance.
   - 

      - ``password``
      - Yes
      - ``string``
      - API password of the RabbitMQ instance.
   - 

      - ``useHTTPS``
      - No
      - ``bool``
      - Whether to activate HTTPS. The default value is\ ``false``.
   - 

      - ``sslCACertFile``
      - No
      - ``string``
      - Path to the SSL or TLS certificate of the root certificate
         authority implicitly trusted by this monitor.
   - 

      - ``sslCertFile``
      - No
      - ``string``
      - Path to this monitor's own SSL or TLS certificate.
   - 

      - ``sslKeyFile``
      - No
      - ``string``
      - Path to this monitor's private SSL or TLS key file.
   - 

      - ``sslKeyPassphrase``
      - No
      - ``string``
      - This monitor's private SSL or TLS key file password, if any.
   - 

      - ``sslVerify``
      - No
      - ``bool``
      - Whether the monitor verifies the RabbitMQ Management plugin SSL
         or TLS certificate. The default value is\ ``false``.

.. note:: You must activate each of the five ``collect*`` options to gather metrics pertaining to those facets of a RabbitMQ instance.

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/collectd/rabbitmq/metadata.yaml"></div>


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



