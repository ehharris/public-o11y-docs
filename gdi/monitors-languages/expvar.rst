.. _expvar:

Expvar (Go)
===========

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Golang Expvar monitor. See benefits, install, configuration, and metrics".

The :ref:`Splunk Distribution of OpenTelemetry Collector <otel-intro>` uses the :ref:`Smart Agent receiver <smartagent-receiver>` with the ``expvar`` monitor type to scrape metrics exposed Expvar. See :new-page:`expvar <https://golang.org/pkg/expvar/>` to learn more.

The integration uses configured paths to get metric and dimension values from fetched JSON objects at an HTTP endpoint. The Metrics section in this document shows metrics derived from the :new-page:`memstats <https://golang.org/pkg/runtime/>` expvar variable. These ``memstat`` metrics are referred to as standard or default metrics and are exposed by default.

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
     smartagent/expvar:
       type: expvar
       ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/expvar]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following tables show the configuration options for this monitor:

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
      - Host of the expvar endpoint
   - 

      - ``port``
      - **yes**
      - ``integer``
      - Port of the expvar endpoint
   - 

      - ``useHTTPS``
      - no
      - ``bool``
      - If ``true``, the agent will connect to the host using HTTPS
         instead of plain HTTP. (**default:** ``false``)
   - 

      - ``skipVerify``
      - no
      - ``bool``
      - If useHTTPS is ``true``\ and this option is also ``true``, the
         host TLS cert will not be verified. (**default:** ``false``)
   - 

      - ``path``
      - no
      - ``string``
      - Path to the expvar endpoint, usually ``/debug/vars`` (the
         default). (**default:** ``/debug/vars``)
   - 

      - ``enhancedMetrics``
      - no
      - ``bool``
      - If ``true``, sends metrics memstats.alloc,
         memstats.by_size.size, memstats.by_size.mallocs and
         memstats.by_size.frees (**default:** ``false``)
   - 

      - ``metrics``
      - no
      - ``list of objects (see below)``
      - Metrics configurations

The **nested** ``metrics`` configuration object has the following
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

      - ``name``
      - no
      - ``string``
      - Metric name
   - 

      - ``JSONPath``
      - **yes**
      - ``string``
      - JSON path of the metric value
   - 

      - ``type``
      - **yes**
      - ``string``
      - SignalFx metric type. Possible values are “gauge” or
         “cumulative”
   - 

      - ``dimensions``
      - no
      - ``list of objects (see below)``
      - Metric dimensions
   - 

      - ``pathSeparator``
      - no
      - ``string``
      - Path separator character of metric value in JSON object
         (**default:** ``.``)

The **nested** ``dimensions`` configuration object has the following
fields:

.. list-table::
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``name``
      - **yes**
      - ``string``
      - Dimension name
   - 

      - ``JSONPath``
      - no
      - ``string``
      - JSON path of the dimension value
   - 

      - ``value``
      - no
      - ``string``
      - Dimension value

Metrics
-------

Do not configure the monitor for memstats metrics because they are
standard metrics provided by default. The following metrics are also
available for this integration:

.. raw:: html

   <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/expvar/metadata.yaml"></div>


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



