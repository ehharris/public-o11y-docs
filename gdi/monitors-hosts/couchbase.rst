.. _couchbase:

Couchbase server
================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Couchbase monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of OpenTelemetry Collector uses the Smart Agent receiver with the
Couchbase server monitor type to collect metrics from Couchbase servers.

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




Configuration options
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following configuration options are available for this integration:

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Config option
      - Required
      - Type
      - Description
   - 

      - ``pythonBinary``
      - no
      - ``string``
      - Path to a Python binary that should be used to execute the
         Python code. If not set, a built-in runtime will be used. This
         can include arguments to the binary as well.
   - 

      - ``host``
      - **yes**
      - ``string``
      - 
   - 

      - ``port``
      - **yes**
      - ``integer``
      - 
   - 

      - ``collectTarget``
      - **yes**
      - ``string``
      - Define what the module block will monitor: ``NODE`` for a
         Couchbase node, or ``BUCKET`` for a Couchbase bucket.
   - 

      - ``collectBucket``
      - no
      - ``string``
      - If ``collectTarget`` is ``BUCKET``, ``collectBucket`` specifies
         the name of the bucket that this will monitor.
   - 

      - ``clusterName``
      - no
      - ``string``
      - Name of this Couchbase cluster. Defaults to ``default``.
   - 

      - ``collectMode``
      - no
      - ``string``
      - Change to ``detailed`` to collect all available metrics from
         Couchbase stats API. Defaults to ``default``, collecting a
         curated set that works well with SignalFx.
   - 

      - ``username``
      - no
      - ``string``
      - Username to authenticate with
   - 

      - ``password``
      - no
      - ``string``
      - Password to authenticate with

Example
~~~~~~~

To activate this integration, add the following to your Collector
configuration:

.. code-block:: yaml

   receivers:
     smartagent/couchbase:
       type: collectd/couchbase
       ...  # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   service:
    pipelines:
      metrics:
        receivers: [smartagent/couchbase]

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/collectd/couchbase/metadata.yaml"></div>


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



