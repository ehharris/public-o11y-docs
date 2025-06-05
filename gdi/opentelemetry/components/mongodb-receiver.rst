.. _mongodb-receiver:

***********************
MongoDB receiver
***********************

.. meta::
      :description: The MongoDB receiver fetches stats from a MongoDB instance using the golang mongo driver. 

The MongoDB receiver fetches metrics from standalone MongoDB clusters, including non-Atlas managed MongoDB servers. The supported pipeline type is ``metrics``. See :ref:`otel-data-processing` for more information. 

The receiver collects stats with MongoDB's ``dbStats`` and ``serverStatus`` commands, and uses the golang mongo driver. See more at :new-page:`Mongo Go driver documentation <https://github.com/mongodb/mongo-go-driver>`.

.. note:: Use the MongoDB receiver in place of the deprecated SignalFx Smart Agent ``mongodb`` monitor type.

Prerequisites
======================

The MongoDB receiver supports MongoDB versions 4.0+ and 5.0. 

MongoDB recommends to set up a least privilege user (LPU) with a ``clusterMonitor`` role in order to collect metrics. 

* For information on MongoDB's roles, see :new-page:`MongoDB built-in roles <https://www.mongodb.com/docs/v5.0/reference/built-in-roles/#mongodb-authrole-clusterMonitor>`.
* For an example of how to configure these permissions, see :new-page:`lpu.sh <https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/receiver/mongodbreceiver/testdata/integration/scripts/lpu.sh>`.

.. note:: If you're using automatic discovery with MongoDB, see :ref:`linux-third-party-mongodb`.

Get started
======================

Follow these steps to configure and activate the component:

1. Deploy the Splunk Distribution of the OpenTelemetry Collector to your host or container platform:
   
   - :ref:`otel-install-linux`
   - :ref:`otel-install-windows`
   - :ref:`otel-install-k8s`

2. Configure the receiver as described in the next section.
3. Restart the Collector.

Sample configurations
---------------------------

To activate the MongoDB receiver, add ``mongodb`` to the ``receivers`` section of your configuration file, as shown in the following example:

.. code:: yaml

  receivers:
    mongodb:
      hosts:
        - endpoint: localhost:27017
          transport: tcp
      username: otel
      password: ${env:MONGODB_PASSWORD}
      collection_interval: 60s
      initial_delay: 1s
      tls:
        insecure: true
        insecure_skip_verify: true

To complete the configuration, include the receiver in the ``metrics`` pipeline of the ``service`` section of your configuration file. For example:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [mongodb]

Configuration options
--------------------------------------------

The following settings are optional:

* ``hosts``. ``[localhost:27017]`` by default. List of ``host:port`` or Unix domain socket endpoints.

  * For standalone MongoDB deployments this is the hostname and port of the mongod instance.

  * For replica sets specify the hostnames and ports of the mongod instances that are in the replica set configuration. If the ``replica_set`` field is specified, nodes will be autodiscovered.

  * For a sharded MongoDB deployment, specify a list of the ``mongos`` hosts.

* ``username``: If authentication is required, provide the ``clusterMonitor`` permissions here.

* ``password``: If authentication is required, provide the password here.

* ``collection_interval``. ``1m`` by default. This receiver collects metrics on an interval. Valid time units are ``ns``, ``us`` (or ``µs``), ``ms``, ``s``, ``m``, ``h``. This value must be a string readable by Golang's time parseDuration. Learn more at :new-page:`ParseDuration <https://pkg.go.dev/time#ParseDuration>`.

* ``initial_delay``. ``1s`` by default. Defines how long this receiver waits before starting.

* ``replica_set``: If the deployment of MongoDB is a replica set, use this to specify the replica set name which allows for autodiscovery of other nodes in the replica set.

* ``timeout``. ``1m`` by default. The timeout of running commands against mongo.

* ``tls``: TLS control. By default insecure settings are rejected and certificate verification is on. See more at :new-page:`TLS Configuration Settings <https://github.com/open-telemetry/opentelemetry-collector/blob/main/config/configtls/README.md>`.

Settings
======================

The following table shows the configuration options for the MongoDB receiver:

.. raw:: html

   <div class="metrics-standard" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/cfg-metadata/receiver/mongodb.yaml"></div>

Metrics
=====================

The following metrics, resource attributes, and attributes are available.

.. raw:: html

   <div class="metrics-component" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/metric-metadata/mongodbreceiver.yaml"></div>

* ``mongodb.extent.count`` is available for versions under 4.4 with mmapv1 storage engine.



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



