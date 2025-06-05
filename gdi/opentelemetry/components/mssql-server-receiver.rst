.. _mssql-server-receiver:

************************************
Microsoft SQL server receiver
************************************

.. meta::
      :description: The Microsft SQL server receiver grabs metrics from a Microsoft SQL Server instance. 

The Microsoft SQL Server receiver queries and retrieves metrics from Microsoft SQL Server instances. The receiver works by either using the Windows Performance Counters, or by directly connecting to the instance and querying it. The supported pipeline type is ``metrics``. See :ref:`otel-data-processing` for more information.

The following applies:

* Windows Performance Counters are only available when running on Windows. 
* Make sure to run the Collector as an administrator in order to collect all performance counters for metrics.

Get started
======================

Follow these steps to configure and activate the component:

1. Deploy the Splunk Distribution of the OpenTelemetry Collector to your host or container platform:

  - :ref:`otel-install-linux`
  - :ref:`otel-install-windows`
  - :ref:`otel-install-k8s`

2. Configure the MSSQL Server receiver as described in the next section.
3. Restart the Collector.

Sample configuration
----------------------

To activate the receiver, add ``sqlserver`` to the ``receivers`` section of your configuration file:

.. code:: yaml

  receivers:
      sqlserver:
        collection_interval: 10s
      sqlserver/1:
        collection_interval: 5s
        username: sa
        password: securepassword
        server: 0.0.0.0
        port: 1433

Next, include the receiver in the ``metrics`` pipeline of the ``service`` section of your configuration file:

.. code:: yaml

  service:
    pipelines:
      metrics:
        receivers:
          - sqlserver

.. caution:: To retrieve out-of-the-box content properly you need to explicitly enable and disable specific metrics and resource attributes in your configuration file. Read more at :ref:`mssql-server-receiver-ootb`.

Configure a named instance on Windows
--------------------------------------------

If you're using a named instance on Windows, you need to specify a computer and instance name, for example:

.. code:: yaml

    receivers:
      sqlserver:
        collection_interval: 10s
        computer_name: CustomServer
        instance_name: CustomInstance
        resource_attributes:
          sqlserver.computer.name:
            enabled: true
          sqlserver.instance.name:
            enabled: true

Advanced configuration
--------------------------------------------

The following settings are optional:

* ``collection_interval``. ``10s`` by default. The interval at which the receiver emits metrics.
* ``instance_name``. Optional. The instance name identifies the specific SQL Server instance to monitor. If unspecified, metrics are scraped from all instances. If configured, you must also set ``computer_name`` when running on Windows.

These are the optional direct connection options:

* ``username``. The username used to connect to the SQL Server instance.
* ``password``. The password used to connect to the SQL Server instance.
* ``server``. IP address or hostname of the SQL Server instance to connect to.
* ``port``. Port of the SQL Server instance to connect to.

The following are Windows-specific optional options:

* ``computer_name``. The computer name identifies the SQL Server name or IP address of the computer being monitored. If specified, ``instance_name`` is also required. This option is ignored in non-Windows environments.

.. _mssql-server-receiver-ootb:

Enable built-in content
====================================

Splunk Observability Cloud provides built-in dashboards with charts that give you immediate visibility into the technologies and services being used in your environment. Learn more at :ref:`collector-builtin-dashboard`.

.. caution:: For the MS SQL Server receiver out-of-the-box content to work properly you need to explicitly enable and disable specific metrics and resource attributes in your configuration file. 

For more information:

* See the configuration that enables built-in content at :new-page:`SQL Server discovery yaml <https://github.com/signalfx/splunk-otel-collector/blob/main/internal/confmapprovider/discovery/bundle/bundle.d/receivers/sqlserver.discovery.yaml>` in GitHub.

* See the list of default and optional metrics at :new-page:`SQL server default and optional metrics <https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/receiver/sqlserverreceiver/documentation.md#optional-metrics>` in GitHub. 

Enable metrics and resource attributes
--------------------------------------------

Some resource attributes, such as ``sqlserver.instance.name``, are disabled by default. 

To enable them, specify the option in your config file: 

.. code:: yaml

  receivers:
      sqlserver:
        collection_interval: 10s
      sqlserver/1:
        collection_interval: 5s
        username: sa
        password: securepassword
        server: 0.0.0.0
        port: 1433
        resource_attributes:
          sqlserver.instance.name:
            enabled: true

.. _mssql-server-receiver-settings:

Settings
======================

The following table shows the configuration options for the Microsoft SQL server receiver:

.. raw:: html

  <div class="metrics-standard" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/cfg-metadata/receiver/sqlserver.yaml"></div>

.. _mssql-server-receiver-metrics:

Metrics
=======================

The following metrics, resource attributes, and attributes, are available.

.. raw:: html

  <div class="metrics-component" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/metric-metadata/sqlserverreceiver.yaml"></div>



.. raw:: html

   <div class="include-start" id="activate-deactivate-native-metrics.rst"></div>

.. include:: /_includes/activate-deactivate-native-metrics.rst

.. raw:: html

   <div class="include-stop" id="activate-deactivate-native-metrics.rst"></div>




Troubleshooting
======================



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



