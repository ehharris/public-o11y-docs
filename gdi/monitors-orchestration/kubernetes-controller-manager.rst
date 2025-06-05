.. _kube-controller-manager:

Kubernetes Controller Manager (deprecated)
==========================================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the kube-controller-manager monitor. See benefits, install, configuration, and metrics

.. note:: This monitor is deprecated in favor of the ``prometheus-exporter`` monitor. See :ref:`Prometheus Exporter <prometheus-exporter>` for more information.

The monitor queries path ``/metrics`` by default when no path is
configured. It converts the Prometheus metric types to Splunk
Observability Cloud metric types as described in the documentation for
:ref:`prometheus-exporter` All Prometheus labels are converted
directly to Infrastructure Monitoring dimensions.

Configuration settings
----------------------

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
         https://golang.org/pkg/time/#ParseDuration (**default:**
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
      - If useHTTPS is ``true`` and this option is also ``true``, the
         exporter's TLS cert will not be verified. (**default:**
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
      - Path to the client TLS cert to use for TLS required connections
   - 

      - ``clientKeyPath``
      - no
      - ``string``
      - Path to the client TLS key to use for TLS required connections
   - 

      - ``host``
      - **yes**
      - ``string``
      - Host of the exporter
   - 

      - ``port``
      - **yes**
      - ``integer``
      - Port of the exporter
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
         Prometheus exporter monitor directly since there is no built-in
         filtering, only when embedding it in other monitors.
         (**default:** ``false``)

Metrics
-------

These are the metrics available for this integration. All metrics are
custom and are only emitted if specified explicitly.

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/kubernetes/controllermanager/metadata.yaml"></div>


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



