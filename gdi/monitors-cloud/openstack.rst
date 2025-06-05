.. _openstack:

OpenStack
=========

.. meta::
   :description: Use this Splunk Observability Cloud integration for the OpenStack monitor, based on the Python plugin. See benefits, install, configuration, and metrics

The Splunk Distribution of OpenTelemetry Collector uses the Smart Agent receiver with the
OpenStack monitor type to gather metrics from OpenStack instances.

This integration covers the following OpenStack components:

-  Nova (Compute)
-  Cinder (Block Storage)
-  Neutron (Network)

This integration is available on Linux and Kubernetes.

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

.. code:: yaml

   receivers:
     smartagent/openstack:
       type: collectd/openstack
       ...  # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/openstack]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for the OpenStack
monitor:

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``pythonBinary``
      - No
      - ``string``
      - Path to the Python binary. If not set, a built-in runtime is
         used. This setting can include arguments to the binary.
   - 

      - ``authURL``
      - Yes
      - ``string``
      - Keystone authentication URL or endpoint for the OpenStack cloud.
   - 

      - ``username``
      - Yes
      - ``string``
      - Username to authenticate with keystone identity.
   - 

      - ``password``
      - Yes
      - ``string``
      - Password to authenticate with keystone identity.
   - 

      - ``projectName``
      - No
      - ``string``
      - Specify the name of the project to be monitored. The default
         value is ``demo``.
   - 

      - ``projectDomainID``
      - No
      - ``string``
      - The project domain. The default value is ``default``.
   - 

      - ``regionName``
      - No
      - ``string``
      - The region name for URL discovery. The region name defaults to
         the first region if multiple regions are available.
   - 

      - ``userDomainID``
      - No
      - ``string``
      - The user domain ID. The default value is ``default``.
   - 

      - ``skipVerify``
      - No
      - ``bool``
      - Skips SSL certificate validation. The default value is
         ``false``.

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/collectd/openstack/metadata.yaml"></div>


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



