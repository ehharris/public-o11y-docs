.. _kubernetes-scheduler:

Kubernetes scheduler
====================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Kubernetes scheduler monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the
``kubernetes-scheduler`` monitor type to export Prometheus metrics from the
kube-scheduler.

This monitor type is available on Kubernetes, Linux, and Windows.

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
     smartagent/kubernetes-scheduler
       type: kubernetes-scheduler
       ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
      pipelines:
        metrics:
          receivers: [smartagent/kubernetes-scheduler]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Config option
      - Required
      - Type
      - Description
   - 

      - ``httpTimeout``
      - no
      - ``int64``
      - HTTP timeout duration for both read and writes. This should be a
         duration string that is accepted by
         https://golang.org/pkg/time/#ParseDuration **Default** is
         ``10s``.
   - 

      - ``username``
      - no
      - ``string``
      - Basic authentication username to use on each request, if any.
   - 

      - ``password``
      - no
      - ``string``
      - Basic authentication password to use on each request, if any.
   - 

      - ``useHTTPS``
      - no
      - ``bool``
      - If true, the agent will connect to the server using HTTPS
         instead of plain HTTP. **Default** is ``false``.
   - 

      - ``httpHeaders``
      - no
      - ``map of strings``
      - A map of HTTP header names to values. Comma separated multiple
         values for the same message-header is supported.
   - 

      - ``skipVerify``
      - no
      - ``bool``
      - If useHTTPS is true and this option is also true, the exporter
         TLS cert will not be verified. **Default** is ``false``
   - 

      - ``sniServerName``
      - no
      - ``string``
      - If useHTTPS is true and skipVerify is true, the sniServerName is
         used to verify the hostname on the returned certificates. It is
         also included in the client's handshake to support virtual
         hosting unless it is an IP address.
   - 

      - ``caCertPath``
      - no
      - ``string``
      - Path to the CA cert that has signed the TLS cert, unnecessary if
         ``skipVerify`` is set to false.
   - 

      - ``clientCertPath``
      - no
      - ``string``
      - Path to the client TLS cert to use for TLS required connections.
   - 

      - ``clientKeyPath``
      - no
      - ``string``
      - Path to the client TLS key to use for TLS required connections.
   - 

      - ``host``
      - **yes**
      - ``string``
      - Host of the exporter.
   - 

      - ``port``
      - **yes**
      - ``integer``
      - Port of the exporter.
   - 

      - ``useServiceAccount``
      - no
      - ``bool``
      - Use pod service account to authenticate. **Default** is
         ``false``.
   - 

      - ``metricPath``
      - no
      - ``string``
      - Path to the metrics endpoint on the exporter server, usually
         ``/metrics``. **Default** is ``/metrics``.
   - 

      - ``sendAllMetrics``
      - no
      - ``bool``
      - Send all the metrics that come out of the Prometheus exporter
         without any filtering. This option has no effect when using the
         prometheus exporter monitor directly since there is no built-in
         filtering, only when embedding it in other monitors.
         **Default** is ``false``.

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/kubernetes/scheduler/metadata.yaml"></div>


Notes
~~~~~



.. raw:: html

   <div class="include-start" id="metric-defs.rst"></div>

.. include:: /_includes/metric-defs.rst

.. raw:: html

   <div class="include-stop" id="metric-defs.rst"></div>




Non-default metrics (version 4.7.0+)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To emit metrics that are not *default*, you can add those metrics in the
generic monitor-level ``extraMetrics`` config option. Metrics that are
derived from specific configuration options that do not appear in the
above list of metrics do not need to be added to ``extraMetrics``.

To see a list of metrics that will be emitted you can run
``agent-status monitors`` after configuring this monitor in a running
agent instance.

Troubleshooting
---------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



