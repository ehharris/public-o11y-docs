.. _vsphere:

VMware vSphere
==============

.. meta::
   :description: Use this Splunk Observability Cloud integration for the vSphere monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the ``vsphere`` monitor type to collect metrics from vSphere through the vSphere API.

This integration is available on Kubernetes, Linux, and Windows. You can
install it on the same server used by vSphere if it's running on Linux
or Windows.

This integration uses VMware ``govmomi`` SDK, which officially supports
vCenter 6.5, 6.7, and 7.0. While this monitor might work with vCenter
5.1, 5.5, and 6.0, these versions are not officially supported. Photon Operating System is not supported.    

.. note:: When you add a custom role, don't assign any privileges to it. The role is created as a read-only role with three system-defined privileges: ``System.Anonymous``, ``System.View``, and ``System.Read``. For more information, see the vSphere official documentation on user roles.

.. caution::  VMware does not allow any modifications to their Virtual Appliances, including vCenter. 

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
     smartagent/vsphere:
       type: vsphere
       ...  # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/vsphere]

See the following example with extended config options:

.. code:: yaml

   receivers:
     smartagent/vsphere:
       type: vsphere
       host: hostname
       username: user
       password: pass
       insecureSkipVerify: true
   exporters:
     signalfx:
       access_token: abc123
       realm: us2
   service:
     pipelines:
       metrics:
         receivers:
           - smartagent/vsphere
         exporters:
           - signalfx

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the main configuration options for the ``vsphere``
monitor. For more settings see :new-page:`vSphere monitor <https://github.com/signalfx/signalfx-agent/blob/main/docs/monitors/vsphere.md>` in GitHub.

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
      - No
      - ``string``
      - Hostname or IP address of the vSphere instance. For example,
         ``127.0.0.1``.
   - 

      - ``port``
      - No
      - ``integer``
      - Port of the vSphere instance. The default value is ``0``)
   - 

      - ``username``
      - No
      - ``string``
      - vSphere username.

   - 

      - ``password``
      - No
      - ``string``
      - vSphere password.
   - 

      - ``filter``
      - No
      - ``string``
      - * Limits the inventory traversed by the monitor. Leave blank or omit to traverse and get metrics for the entire vSphere inventory. Otherwise, this expression is evaluated per cluster. If ``true``, metrics are collected for the objects in the cluster, otherwise it's skipped. 
        * The expression can use the variables ``Datacenter`` and ``Cluster``. For example, to collect metrics for a specific datacenter and cluster, use ``filter: "Datacenter == 'MyDatacenter' && Cluster == 'MyCluster'"``. 
        * See :new-page:`expr <https://github.com/antonmedv/expr>` in GitHub for advanced syntax.
   - 

      - ``insecureSkipVerify``
      - No
      - ``bool``
      - Controls whether a client verifies the server's certificate
         chain and host name. The default value is ``false``.
   - 

      - ``inventoryRefreshInterval``
      - No
      - ``integer``
      - Controls how often to reload the inventory and inventory
         metrics. The default value is ``60s``.
   - 

      - ``perfBatchSize``
      - No
      - ``integer``
      - Controls the maximum number of inventory objects to be queried
         for performance data per request. Set this value to ``0`` to
         request performance data for all inventory objects at a time.
         The default value is ``10``.
   - 

      - ``tlsCACertPath``
      - No
      - ``string``
      - Path to the CA certificate file.
   - 

      - ``tlsClientCertificatePath``
      - No
      - ``string``
      - Path to the client certificate. Both ``tlsClientKeyPath`` and
         ``tlsClientCertificatePath`` must be present. The files must
         contain PEM encoded data.
   - 

      - ``tlsClientKeyPath``
      - No
      - ``string``
      - Path to the keyfile.
   - 

      - ``vmHostDimension``
      - No
      - ``string``
      - The host dimension value set for monitored VMs. The options are
         ``ip`` (default value, the VM IP if available), ``hostname``
         (the VM Hostname if available) , and ``disable`` (the vsphere
         monitor does not set the host dimension on the VM metrics).

To report metrics for a vSphere deployment, this monitor logs into a
vCenter Server and retrieves data about the deployment and real time
performance data on a regular interval. When the monitor first runs, it
logs in to the vCenter Server and traverses the inventory, gathering and
caching all of the hosts and virtual machines and their available
metrics.

After the initial sweep, the monitor queries the vCenter for performance
data and metrics. This query runs every 20 seconds, which is the
interval at which the vCenter makes real time performance data
available. As a result, regardless of the ``intervalSeconds`` value in
the agent configuration, this monitor runs every 20 seconds.

The monitor also refreshes, at a configurable interval, the cache of
hosts, virtual machines, and metrics. By default, this refresh takes
place every 60 seconds; however, this interval can be changed by
updating the configuration field ``InventoryRefreshInterval``.

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/vsphere/metadata.yaml"></div>


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



