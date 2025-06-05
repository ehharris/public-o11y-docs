.. _mongodb:

MongoDB (deprecated)
=================================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the MongoDB monitor. See benefits, install, configuration, and metrics.

.. note:: The MongoDB monitor is deprecated and will reach end of support on January 15, 2025. During this period, only critical security and bug fixes are provided. When the monitor reaches end of support, you won't be able to use it to send data to Splunk Observability Cloud. To monitor your MongoDB databases, you can instead use the native OpenTelemetry MongoDB receiver. See :ref:`mongodb-receiver` to learn more.

The :ref:`Splunk Distribution of the OpenTelemetry Collector <otel-intro>` uses the :ref:`Smart Agent receiver <smartagent-receiver>` with the MongoDB monitor type to capture the following metrics about MongoDB:

-  Memory
-  Network input/output bytes count
-  Heap usage
-  DB connections
-  Operations count
-  Active client connections
-  Queued operations

The plugin also captures the following DB-specific metrics:

-  DB size
-  DB counters

Prerequisites
----------------

The following applies:

* This integration is only available on Kubernetes and Linux.
* This integration requires MongoDB 2.6 or higher.
* This integration is not supported for Splunk OTel Collector versions 0.99.0 or higher. Use the :ref:`mongodb-receiver` instead.

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




Authentication
~~~~~~~~~~~~~~

If you're monitoring a secured MongoDB deployment, create a MongoDB user
with minimal read-only roles, as follows:

.. code-block::

   db.createUser( {
     user: "<username>",
     pwd: "<password>",
     roles: [ { role: "readAnyDatabase", db: "admin" }, { role: "clusterMonitor", db: "admin" } ]
   });

.. note:: Only SCRAM-SHA-1 authentication is supported.

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
     smartagent/mongodb:
       type: collectd/mongodb
       ...  # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/mongodb]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for the MongoDB
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

      - ``host``
      - Yes
      - ``string``
      - Hostname or IP address of the MongoDB instance.
   - 

      - ``port``
      - Yes
      - ``integer``
      - Port of the MongoDB instance. The default value is ``27017``.
   - 

      - ``databases``
      - Yes
      - ``list of strings``
      - Name of the databases you want to monitor. The first database in
         this list must be ``admin``, as it's used to perform a
         ``serverStatus()`` call.
   - 

      - ``username``
      - No
      - ``string``
      - MongoDB user.
   - 

      - ``password``
      - No
      - ``string``
      - Password of the user defined in ``username``.
   - 

      - ``useTLS``
      - No
      - ``bool``
      - If ``true``, the monitor connects to MongoDB using TLS. The
         default value is ``false``.
   - 

      - ``caCerts``
      - No
      - ``string``
      - Path to a CA cert used to verify the certificate that MongoDB
         presents. Not needed if not using TLS or if MongoDB certificate
         is signed by a globally trusted issuer already installed in the
         default location on your system.
   - 

      - ``tlsClientCert``
      - No
      - ``string``
      - Path to a client certificate. Not needed unless your MongoDB
         instance requires x509 client verification.
   - 

      - ``tlsClientKey``
      - No
      - ``string``
      - Path to a client certificate key. Not needed unless your MongoDB
         instance requires x509 client verification, or if your client
         certificate defined in ``tlsClientCert`` includes the key.
   - 

      - ``tlsClientKeyPassPhrase``
      - No
      - ``string``
      - Passphrase for the TLS client key defined in ``tlsClientKey``.
   - 

      - ``sendCollectionMetrics``
      - No
      - ``bool``
      - Whether to send collection level metrics or not. The default
         value is ``false``.
   - 

      - ``sendCollectionTopMetrics``
      - No
      - ``bool``
      - Whether to send collection level top timing metrics or not. The
         default value is ``false``.

.. note:: When using TLS authentication, SCRAM-SHA-256 is not supported. Use SCRAM-SHA-1 authentication.

Troubleshooting
---------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



