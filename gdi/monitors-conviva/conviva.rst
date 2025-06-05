.. _conviva:

Conviva Real-Time/Live video play
=================================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Conviva monitor. See benefits, install, configuration, and metrics, including MetricLens

The Splunk Distribution of OpenTelemetry Collector uses the Smart Agent receiver with the
Conviva monitor type to pull ``Real-Time/Live`` video playing experience
metrics from Conviva.

This integration uses version 2.4 of the Conviva Experience Insights
REST APIs.

Only ``Live`` conviva metrics listed on the Conviva Developer Community
page are supported. All metrics are gauges. The Conviva metrics are
converted to Splunk Observability Cloud metrics with dimensions named
account and filter. The account dimension is the name of the Conviva
account and the filter dimension is the name of the Conviva filter
applied to the metric. In the case of MetricLenses, the constituent
MetricLens metrics and MetricLens dimensions are included. The values of
the MetricLens dimensions are derived from the values of the associated
MetricLens dimension entities.

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

.. code-block:: yaml

   receivers:
     smartagent/conviva:
       type: conviva
       ...  # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/conviva]

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

      - ``pulseUsername``
      - **yes**
      - ``string``
      - Conviva Pulse username required with each API request.
   - 

      - ``pulsePassword``
      - **yes**
      - ``string``
      - Conviva Pulse password required with each API request.
   - 

      - ``timeoutSeconds``
      - no
      - ``integer``
      - (**default:** ``10``)
   - 

      - ``metricConfigs``
      - no
      - ``list of objects (see below)``
      - Conviva metrics to fetch. The default is quality_metriclens
         metric with the “All Traffic” filter applied and all
         quality_metriclens dimensions.

The **nested** ``metricConfigs`` configuration object has the following
fields:

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``account``
      - no
      - ``string``
      - Conviva customer account name. The default account is fetched
         used if not specified.
   - 

      - ``metricParameter``
      - no
      - ``string``
      - (**default:** ``quality_metriclens``)
   - 

      - ``filters``
      - no
      - ``list of strings``
      - Filter names. The default is ``All Traffic`` filter.
   - 

      - ``metricLensDimensions``
      - no
      - ``list of strings``
      - MetricLens dimension names. The default is names of all
         MetricLens dimensions of the account
   - 

      - ``excludeMetricLensDimensions``
      - no
      - ``list of strings``
      - MetricLens dimension names to exclude.
   - 

      - ``maxFiltersPerRequest``
      - no
      - ``integer``
      - Max number of filters per request. The default is the number of
         filters. Multiple requests are made if the number of filters is
         more than maxFiltersPerRequest (**default:** ``0``)

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/conviva/metadata.yaml"></div>


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



