.. _signalfx-forwarder:

SignalFx Forwarder (deprecated)
==================================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the SignalFx forwarder. See benefits, install, configuration, and metrics

.. note:: This integration is deprecated. If you're using the Splunk Distribution of the OpenTelemetry Collector and want to forward data to Splunk Observability Cloud, use the native OTLP receiver. See more at :new-page:`OTLP receiver <https://github.com/open-telemetry/opentelemetry-collector/tree/main/receiver/otlpreceiver>` GitHub documentation.

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the
``signalfx-forwarder`` monitor type to run an HTTP server that listens
for data points and trace spans, and forward them to Splunk
Observability Cloud. This integration supports the latest formats for
data points and spans that the Splunk ingest API endpoint supports.

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
     smartagent/signalfx-forwarder:
       type: signalfx-forwarder
       ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/signalfx-forwarder]
       traces:
         receivers: [smartagent/signalfx-forwarder]

Note that the ``defaultSpanTagsFromEndpoint`` and
``extraSpanTagsFromEndpoint`` config options are not compatible with the
``signalfx-forwarder`` receiver.

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for the
``signalfx-forwarder`` integration:

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
      - The host and port on which to listen for data points. The
         listening server accepts data points on the same HTTP path as
         the ingest endpoint or gateway. For example, ``/v2/datapoint``
         or ``/v1/trace``. Requests to other paths return errors with
         HTTP code 404. The default value is ``127.0.0.1:9080``.
   - 

      - ``serverTimeout``
      - no
      - ``int64``
      - HTTP timeout duration for both read and write operations.
         Accepts a duration string for
         https://golang.org/pkg/time/#ParseDuration. The default value
         is ``5s``.
   - 

      - ``sendInternalMetrics``
      - no
      - ``bool``
      - Whether to emit internal metrics about the HTTP listener. The
         default value is ``false``.

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



