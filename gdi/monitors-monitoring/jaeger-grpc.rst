.. _jaeger-grpc:

Jaeger gRPC
===========

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Jaeger gRPC monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of OpenTelemetry Collector uses the Smart Agent receiver with the
``jaeger-grpc`` monitor type to run a gRPC server that listens for
Jaeger trace batches and forwards them to Splunk Observability Cloud (or
the configured ingest host in the ``writer`` section of the agent
config). By default, the server listens on localhost port ``14250``, but
can be configured to anything.

.. note:: If you're using OpenTelemetry, consider using the native OpenTelemetry Jaeger receiver. To learn more, see :new-page:`the Jaeger receiver documentation in GitHub <https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/jaegerreceiver>`.

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




### Example

To activate this integration, add the following to your Collector
configuration:

.. code-block:: yaml

   receivers:
     smartagent/jaeger-grpc: 
       type: jaeger-grpc
       ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/jaeger-grpc]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for this monitor:

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``listenAddress``
      - no
      - ``string``
      - The host:port on which to listen for traces. The default value
         is ``0.0.0.0:14250``.
   - 

      - ``tls``
      - no
      - ``object (see below)``
      - TLS are optional tls credential settings to configure the GRPC
         server with

The **nested** ``tls`` config object has the following fields:

.. list-table::
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``certFile``
      - no
      - ``string``
      - The cert file to use for tls
   - 

      - ``keyFile``
      - no
      - ``string``
      - The key file to use for tls

Metrics
-------

The Splunk Distribution of OpenTelemetry Collector does not do any
built-in filtering of metrics for this monitor.

Troubleshooting
---------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



