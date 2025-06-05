.. _cloudfoundry-firehose-nozzle:

Cloud Foundry Loggregator Firehose
==================================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Cloud Foundry Loggregator Firehose monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the Cloud Foundry Loggregator Firehose monitor type to create a Cloud Foundry Firehose nozzle which connects to the Cloud Foundry Reverse Log Proxy (RLP) Gateway to extract metrics from Loggregator. 

.. note:: To monitor Cloud Foundry with the OpenTelemetry Collector using native OpenTelemetry refer to the :ref:`cloudfoundry-receiver` component.

The following applies:

* This integration is available on Linux only. 
* This integration uses the RLP Gateway model from Pivotal Cloud Foundry (PCF) version 2.4 and doesn't work with older releases.
* The monitor supports gauge and counter metrics. Learn more at :ref:`metric-types`.

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

   <div class="include-start" id="collector-installation-linux-only.rst"></div>

.. include:: /_includes/collector-installation-linux-only.rst

.. raw:: html

   <div class="include-stop" id="collector-installation-linux-only.rst"></div>




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
     smartagent/cloudfoundry-firehose-nozzle
       type: cloudfoundry-firehose-nozzle
       ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/cloudfoundry-firehose-nozzle]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for this
integration:

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``rlpGatewayUrl``
      - no
      - ``string``
      - The base URL to the RLP Gateway server. This is quite often of
         the form
         ``https://log-stream.<CLOUD CONTROLLER SYSTEM DOMAIN>`` if
         using PCF 2.4+.
   - 

      - ``rlpGatewaySkipVerify``
      - no
      - ``bool``
      - Whether to skip SSL/TLS verification when using HTTPS to connect
         to the RLP Gateway (**default:** ``false``)
   - 

      - ``uaaUser``
      - no
      - ``string``
      - The UAA username for a user that has the appropriate authority
         to fetch logs from the Firehose (usually the ``logs.admin``
         authority)
   - 

      - ``uaaPassword``
      - no
      - ``string``
      - The password for the above UAA user
   - 

      - ``uaaUrl``
      - no
      - ``string``
      - The URL to the UAA server. This monitor obtains an access token
         from this server that it uses to authenticate with the RLP
         Gateway.
   - 

      - ``uaaSkipVerify``
      - no
      - ``bool``
      - Whether to skip SSL/TLS verification when using HTTPS to connect
         to the UAA server (**default:** ``false``)
   - 

      - ``shardId``
      - no
      - ``string``
      - The nozzle's shard ID. All nozzle instances with the same ID
         receive an exclusive subset of the data from the Firehose. The
         default should suffice in the vast majority of use cases.
         (**default:** ``signalfx_nozzle``)

PCF configuration
~~~~~~~~~~~~~~~~~

Most of PCF Key Performance Indicators (KPIs) come through the Firehose.
Refer to PCF documentation for more information on KPIs to determine
when to scale up or down your cluster.

To create Cloud Foundry User Account and Authentication (UAA) user with
the proper permissions to access the RLP Gateway, run the following
command:

.. code-block:: yaml

   $ uaac client add my-v2-nozzle \
       --name signalfx-nozzle \
       --secret <signalfx-nozzle client secret> \
       --authorized_grant_types client_credentials,refresh_token \
       --authorities logs.admin

Set the ``uaaUsername`` config value to ``signalfx-nozzle`` and the
``uaaPassword`` field to the ``<signalfx-nozzle client secret>`` that
you select.

Metrics
-------

The gauge and counter metrics are collected from PCF Platform apps and
platform components in the following way:

-  Firehose gauge metrics are converted to Splunk Infrastructure
   Monitoring gauges.
-  Firehose counter metrics are converted to Infrastructure Monitoring
   cumulative counters metrics.
-  All of the tags in the Firehose envelopes are converted to dimensions
   when sending to Infrastructure Monitoring.

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/cloudfoundry/metadata.yaml"></div>


Notes
~~~~~



.. raw:: html

   <div class="include-start" id="metric-defs.rst"></div>

.. include:: /_includes/metric-defs.rst

.. raw:: html

   <div class="include-stop" id="metric-defs.rst"></div>




## Troubleshooting



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



