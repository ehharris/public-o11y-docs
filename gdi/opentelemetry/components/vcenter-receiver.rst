.. _vcenter-receiver:

***********************
vCenter receiver
***********************

.. meta::
      :description: The vCenter receiver supports ESXi and vCenter.

The vCenter receiver fetches metrics from a vCenter or ESXi host running VMware vSphere APIs.

Prerequisites
======================

This receiver supports ESXi and vCenter versions 7.0 and 8.

To retrieve data you need to assign a read-only user to vSphere with permissions to the vCenter server, cluster and all subsequent resources being monitored.

Get started
======================

Follow these steps to configure and activate the component:

1. Deploy the Splunk Distribution of the OpenTelemetry Collector to your host or container platform:
   
   - :ref:`otel-install-linux`
   - :ref:`otel-install-windows`
   - :ref:`otel-install-k8s`

2. Configure the receiver as described in the next section.
3. Restart the Collector.

Sample configuration
----------------------------------------------------------------------

To activate the vCenter receiver in the Collector, add ``vecenter`` to the ``receivers`` section of your configuration file, as shown in the following example:

.. code:: yaml

  receivers:
    vcenter:

To complete the configuration, include the receiver in the ``metrics`` pipeline of the ``service`` section of your configuration file. For example:

.. code:: yaml

  service:
    pipelines:
      metrics:
        receivers: [vcenter]

Configuration example
----------------------------------------------------------------------

See the following config example:

.. code:: yaml

  vcenter:
    endpoint: http://vcsa.host.localnet
    username: otelu
    password: ${env:VCENTER_PASSWORD}
    collection_interval: 5m
    metrics:
      vcenter.host.cpu.utilization:
        enabled: false

Configuration settings
-------------------------------------------------

The following settings are required:

* ``password``

* ``username``

* ``endpoint``. Endpoint to the vCenter Server or ESXi host that has the SDK path enabled. 

  * Use the <protocol>://<hostname> format, for example ``https://vcsa.hostname.localnet``.

The following settings are optional:

* ``collection_interval``. ``2m`` by default.  This receiver collects metrics on an interval. If the vCenter is large, increase this value. 

  * Valid time units are ``ns``, ``us`` (or ``µs``), ``ms``, ``s``, ``m``, ``h``.

* ``initial_delay``. ``1s`` by default. Defines how long the receiver waits before starting.

* ``tls``. TLS control. By default insecure settings are rejected and certificate verification is on. Will use defaults for configtls.ClientConfig. Learn more at :new-page:`TLS Configuration Settings <https://github.com/open-telemetry/opentelemetry-collector/blob/main/config/configtls/README.md>` in GitHub.

Settings
======================

The following table shows the configuration options for the vCenter receiver:

.. raw:: html

  <div class="metrics-standard" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/cfg-metadata/receiver/vcenter.yaml"></div>

.. _vcenter-receiver-metrics:

Metrics
=====================

The following metrics, resource attributes, and attributes are available.

.. raw:: html

  <div class="metrics-component" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/metric-metadata/vcenterreceiver.yaml"></div>

Troubleshooting
======================

.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



