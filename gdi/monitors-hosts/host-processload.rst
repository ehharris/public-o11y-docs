.. _load:

Host process load (deprecated)
=================================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the load monitor. See benefits, install, configuration, and metrics

.. caution:: This integration is deprecated. If you're using the Splunk Distribution of the OpenTelemetry Collector and want to collect CPU load metrics use the native OTel component :ref:`host-metrics-receiver`.

Configuration options
---------------------

The following table shows the configuration options for this monitor
type:

.. list-table::
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``perCPU``
      - no
      - ``bool``
      - The default value is ``false``.

Troubleshooting
---------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



