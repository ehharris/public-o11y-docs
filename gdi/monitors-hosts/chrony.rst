.. _chrony:

Chrony NTP (deprecated)
==============================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Chrony NTP monitor. See benefits, install, configuration, and metrics

.. caution:: This integration is deprecated. If you're using the Splunk Distribution of the OpenTelemetry Collector and want to monitor Chrony use the native OpenTelemetry component :ref:`chrony-receiver` instead.

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the
Chrony NTP monitor type to monitor NTP data from a chrony server, such
as clock skew and per-peer stratum. To talk to chronyd, this integration
mimics what the chronyc control program does on the wire.

This integration is only available on Kubernetes and Linux.

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
     smartagent/chrony:
       type: collectd/chrony
       ...  # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
    pipelines:
      metrics:
        receivers: [smartagent/chrony]

Configuration options
~~~~~~~~~~~~~~~~~~~~~

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

      - ``host``
      - **yes**
      - ``string``
      - The hostname of the chronyd instance.
   - 

      - ``port``
      - no
      - ``integer``
      - The UDP port number of the chronyd instance. Defaults to 323 in
         collectd if unspecified.
   - 

      - ``timeout``
      - no
      - ``unsigned integer``
      - How long to wait for a response from chronyd before considering
         it down. Defaults to 2 seconds in the collectd plugin if not
         specified.

Metrics
-------

The Splunk Distribution of OpenTelemetry Collector does not do any
built-in filtering of metrics coming out of this integration. ##
Troubleshooting



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



