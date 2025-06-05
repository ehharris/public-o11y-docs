.. _collectd-df:

Collectd df (deprecated)
===============================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Collectd df plugin monitor. See benefits, install, configuration, and metrics

.. note::  This integration is deprecated in favor of :ref:`filesystems`. 

The Splunk Distribution of OpenTelemetry Collector uses the Smart Agent receiver with the
Collectd df monitor type to track free disk space on the host using the
collectd df plugin.

This integration is only available on Linux. Note a file system must be
mounted in the same file system namespace that the agent is running in
for this monitor to be able to collect statistics about that file
system. This might be an issue when running the agent in a container.

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

   <div class="include-start" id="collector-installation-linux-only.rst"></div>

.. include:: /_includes/collector-installation-linux-only.rst

.. raw:: html

   <div class="include-stop" id="collector-installation-linux-only.rst"></div>




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
     smartagent/collectd/df:
       type: collectd/df
       ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/collectd/df]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for the collectd/df
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

      - ``hostFSPath``
      - no
      - ``string``
      - Path to the root of the host file system. Useful when running in
         a container and the host filesystem is mounted in some
         subdirectory under ``/.``
   - 

      - ``ignoreSelected``
      - no
      - ``bool``
      - If true, the file systems selected by ``fsTypes`` and
         ``mountPoints`` are excluded, and all others are included. The
         default value is ``true``.
   - 

      - ``fsTypes``
      - no
      - ``list of strings``
      - The file system types to include or exclude. The default values
         is
         ``[aufs overlay tmpfs proc sysfs nsfs cgroup devpts selinuxfs devtmpfs debugfs mqueue hugetlbfs securityfs pstore binfmt_misc autofs]``.
   - 

      - ``mountPoints``
      - no
      - ``list of strings``
      - The mount paths to include or exclude, interpreted as a regular
         expression if surrounded by ``/``. Note that you need to
         include the full path as the agent sees it, irrespective of the
         hostFSPath option. The default value is
         ``[/^/var/lib/docker/ /^/var/lib/rkt/pods/ /^/net// /^/smb//]``.
   - 

      - ``reportByDevice``
      - no
      - ``bool``
      - The default value is ``false``.
   - 

      - ``reportInodes``
      - no
      - ``bool``
      - The default value is ``false``.
   - 

      - ``valuesPercentage``
      - no
      - ``bool``
      - Whether true percent based metrics are reported. The default
         value is ``false``.

Troubleshooting
---------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



