.. _collectd-plugin:

Collectd custom plugin
======================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Collectd custom plugin monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the ``collectd/custom`` monitor type to customize the configuration of your managed collectd instances.

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
~~~~~~~~~~~~~~~~~~~~~~

To activate this integration, add the following to your Collector
configuration:

.. code-block:: yaml

   receivers:
     smartagent/custom:
       type: collectd/custom
       ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/custom/collectd]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

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
      - no
      - ``string``
      - This option is filled in by the agent if using service
         discovery. It can be accessed in the provided configuration
         template with ``{{.Host}}``. This option is set to the hostname
         or IP address of the discovered service. If you aren't using
         service discovery, you can hard code the host/port in the
         configuration template and ignore these fields.
   - 

      - ``port``
      - no
      - ``integer``
      - This option is filled in by the agent if using service
         discovery. It can be accessed in the provided configuration
         template with ``{{.Port}}``. This option is set to the port of
         the discovered service, if it is a TCP/UDP endpoint.
         (**default:** ``0``)
   - 

      - ``name``
      - no
      - ``string``
      - This option is filled in by the agent if using service
         discovery. It can be accessed in the provided configuration
         template with ``{{.Name}}``. This option is set to the name
         that the observer creates for the endpoint upon discovery. You
         can generally ignore this field.
   - 

      - ``template``
      - no
      - ``string``
      - A configuration template for collectd. You can include as many
         plugin blocks as you want in this value. It is rendered as a
         standard Go template, so be mindful of the delimiters ``{{``
         and ``}}``.
   - 

      - ``templates``
      - no
      - ``list of strings``
      - A list of templates, but otherwise equivalent to the above
         ``template`` option. This lets you have a single directory with
         collectd configuration files and load them all by using a
         globbed remote configuration value.
   - 

      - ``collectdReadThreads``
      - no
      - ``integer``
      - The number of read threads to use in collectd. This option
         defaults to the number of templates provided, capped at 10, but
         if you manually specify it, there is no limit. (**default:**
         ``0``)

Metrics
-------

The Splunk Distribution of the OpenTelemetry Collector does not do any
built-in filtering of metrics coming out of this integration.

Troubleshooting
---------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



