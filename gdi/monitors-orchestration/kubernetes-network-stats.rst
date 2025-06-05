.. _kubelet-stats:

Kubernetes network stats (deprecated)
=====================================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the kubelet-stats / kubernetes network stats monitor. See benefits, install, configuration, and metrics

.. note:: This monitor is deprecated in favor of the native OTel ``kubeletstats`` receiver. See :ref:`Kubelet Stats Receiver <kubelet-stats-receiver>` for more information.

Configuration settings
----------------------

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

      - ``kubeletAPI``
      - no
      - ``object`` (see the following table)
      - Kubelet client configuration

The **nested** ``kubeletAPI`` configuration object has the following
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

      - ``url``
      - no
      - ``string``
      - URL of the Kubelet instance. This will default to
         ``http://<current node hostname>:10255`` if not provided.
   - 

      - ``authType``
      - no
      - ``string``
      - Can be ``none`` for no auth, ``tls`` for TLS client cert auth,
         or ``serviceAccount`` to use the pod's default service account
         token to authenticate. The default value is ``none``.
   - 

      - ``skipVerify``
      - no
      - ``bool``
      - Whether to skip verification of the Kubelet TLS cert. The
         default value is ``true``.
   - 

      - ``caCertPath``
      - no
      - ``string``
      - Path to the CA cert that has signed the Kubelet TLS cert,
         unnecessary if ``skipVerify`` is set to ``false``.
   - 

      - ``clientCertPath``
      - no
      - ``string``
      - Path to the client TLS cert to use if ``authType`` is set to
         ``tls``
   - 

      - ``clientKeyPath``
      - no
      - ``string``
      - Path to the client TLS key to use if ``authType`` is set to
         ``tls``
   - 

      - ``logResponses``
      - no
      - ``bool``
      - Whether to log the raw cadvisor response at the debug level for
         debugging purposes. The default value is ``false``.

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/kubernetes/kubeletmetrics/metadata.yaml"></div>


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



