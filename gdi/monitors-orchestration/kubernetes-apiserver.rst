.. _kubernetes-apiserver:

Kubernetes API server
=====================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the kubernetes-apiserver monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of OpenTelemetry Collector uses the Smart Agent receiver with the
Kubernetes API server monitor type to retrieve metrics from the API
server's Prometheus metric endpoint.

This integration is available on Kubernetes, Linux, and Windows.

This integration requires access to kube-apiserver pods to be able to
access certain pods in the control plane. Since several
Kubernetes-as-a-service distributions don't expose the control plane
pods to the end user, metric collection might not be possible in these
cases.

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
       smartagent/kubernetes-apiserver:
         type: kubernetes-apiserver
         ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/kubernetes-apiserver]

See the kubernetes-yaml examples in GitHub for the Agent and Gateway
YAML files.

Example: Kubernetes observer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following is an example YAML configuration:

.. code:: yaml

   receivers:
     smartagent/kubernetes-apiserver:
       type: kubernetes-apiserver
       host: localhost
       port: 443
       extraDimensions:
         metric_source: kubernetes-apiserver

The OpenTelemetry Collector has a Kubernetes observer (``k8sobserver``)
that can be implemented as an extension to discover networked endpoints,
such as a Kubernetes pod. Using this observer assumes that the
OpenTelemetry Collector is deployed in host monitoring (agent) mode,
where it is running on each individual node or host instance.

To use the observer, create a receiver creator instance with an
associated rule. For example:

.. code:: yaml

   extensions:
     # Configures the Kubernetes observer to watch for pod start and stop events.
     k8s_observer:

   receivers:
     receiver_creator/1:
       # Name of the extensions to watch for endpoints to start and stop.
       watch_observers: [k8s_observer]
       receivers:
         smartagent/kubernetes-apiserver:
           rule: type == "pod" && labels["k8s-app"] == "kube-apiserver"
           type: kubernetes-apiserver
           port: 443
           extraDimensions:
             metric_source: kubernetes-apiserver

   processors:
     exampleprocessor:

   exporters:
     exampleexporter:

   service:
     pipelines:
       metrics:
         receivers: [receiver_creator/1]
         processors: [exampleprocessor]
         exporters: [exampleexporter]
     extensions: [k8s_observer]

See :ref:`Receiver creator <receiver-creator-receiver>` for more
information.

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

      - ``httpTimeout``
      - no
      - ``int64``
      - HTTP timeout duration for both read and writes. This should be a
         duration string that is accepted by
         https://golang.org/pkg/time/#ParseDuration. (**default:**
         ``10s``)
   - 

      - ``username``
      - no
      - ``string``
      - Basic Auth username to use on each request, if any.
   - 

      - ``password``
      - no
      - ``string``
      - Basic Auth password to use on each request, if any.
   - 

      - ``useHTTPS``
      - no
      - ``bool``
      - If ``true``, the agent will connect to the server using HTTPS
         instead of plain HTTP. (**default:** ``false``)
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
      - If ``useHTTPS`` is ``true`` and this option is also ``true``,
         the exporter TLS cert will not be verified. (**default:**
         ``false``)
   - 

      - ``caCertPath``
      - no
      - ``string``
      - Path to the CA cert that has signed the TLS cert, unnecessary if
         ``skipVerify`` is set to ``false``.
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
      - Use pod service account to authenticate. (**default:**
         ``false``)
   - 

      - ``metricPath``
      - no
      - ``string``
      - Path to the metrics endpoint on the exporter server, usually
         ``/metrics`` (the default). (**default:** ``/metrics``)
   - 

      - ``sendAllMetrics``
      - no
      - ``bool``
      - Send all the metrics that come out of the Prometheus exporter
         without any filtering. This option has no effect when using the
         prometheus exporter monitor directly since there is no built-in
         filtering, only when embedding it in other monitors.
         (**default:** ``false``)

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/kubernetes/apiserver/metadata.yaml"></div>

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

   <div class="include-start" id="bind_address_error_msg.rst"></div>

.. include:: /_includes/bind_address_error_msg.rst

.. raw:: html

   <div class="include-stop" id="bind_address_error_msg.rst"></div>






.. raw:: html

   <div class="include-start" id="missing_pipeline_configuration.rst"></div>

.. include:: /_includes/missing_pipeline_configuration.rst

.. raw:: html

   <div class="include-stop" id="missing_pipeline_configuration.rst"></div>






.. raw:: html

   <div class="include-start" id="out_of_memory_error.rst"></div>

.. include:: /_includes/out_of_memory_error.rst

.. raw:: html

   <div class="include-stop" id="out_of_memory_error.rst"></div>






.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



