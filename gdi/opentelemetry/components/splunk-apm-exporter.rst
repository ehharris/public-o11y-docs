.. _splunk-apm-exporter:

****************************************************
Splunk APM (SAPM) exporter (deprecated)
****************************************************

.. meta::
      :description: Use the Splunk APM (SAPM) exporter to send traces from multiple nodes or services in a single batch. Read on to learn how to configure the component.

.. caution:: The SAPM exporter is deprecated and will be removed in April 2025. To send traces to Splunk Observability Cloud use the :ref:`otlphttp-exporter` instead.

The Splunk APM (SAPM) exporter allows the OpenTelemetry Collector to send traces to Splunk Observability Cloud. The supported pipeline types are ``traces``. See :ref:`otel-data-processing` for more information.

Get started
======================

Follow these steps to configure and activate the component:

1. Deploy the Splunk Distribution of OpenTelemetry Collector to your host or container platform:
  
  - :ref:`otel-install-linux`
  - :ref:`otel-install-windows`
  - :ref:`otel-install-k8s`

2. Configure the exporter as described in this doc.
3. Restart the Collector.  

Sample configuration
----------------------

The following example shows a SAPM exporter instance configuration for a maximum of 100 connections and 8 workers:

.. code:: yaml

   exporters:
     sapm:
       access_token: <access_token>
       access_token_passthrough: true
       endpoint: https://ingest.<realm>.signalfx.com/v2/trace
       max_connections: 100
       num_workers: 8

Next, add the exporter to the ``services`` section of your configuration file:

.. code:: yaml

   service:
     pipelines:
       # To complete the configuration, include the exporter in a traces metrics pipeline. 
       traces:
           receivers: [nop]
           processors: [nop]
           exporters: [sapm]

Configuration example with all settings
----------------------------------------------

The following example shows all available settings:

.. code:: yaml

   exporters:
     sapm/customname:
       # Endpoint is the destination to where traces are sent in SAPM format.
       # The endpoint must be a full URL and include the scheme, port, and path. 
       # For example, https://ingest.us0.signalfx.com/v2/trace
       endpoint: test-endpoint
       # Authentication token provided by Splunk Observability Cloud.
       access_token: abcd1234
       # Number of workers that should be used to export traces.
       # The exporter can make as many requests in parallel as the number of workers.
       num_workers: 3
       # Used to set a limit to the maximum idle HTTP connections the exporter can keep open.
       max_connections: 45
       access_token_passthrough: false
       # Timeout for every attempt to send data to the back end.
       # The default value is 5s.
       timeout: 10s
       sending_queue:
         enabled: true
         num_consumers: 2
         queue_size: 10
       retry_on_failure:
         enabled: true
         initial_interval: 10s
         max_interval: 60s
         max_elapsed_time: 10m

   service:
     pipelines:
       traces:
           receivers: [nop]
           processors: [nop]
           exporters: [sapm]

In the endpoint URL, ``realm`` is the Splunk Observability Cloud realm, for example, ``us0``. To find your Splunk realm, see :ref:`Note about realms <about-realms>`.

.. note:: To send SAPM data through a proxy, configure proxy settings as environment variables. See :ref:`configure-proxy-collector` for more information.

Settings
======================

The following table shows the configuration options for the SAPM exporter:

.. raw:: html

   <div class="metrics-standard" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/cfg-metadata/exporter/sapm.yaml"></div>

Troubleshooting
======================



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



