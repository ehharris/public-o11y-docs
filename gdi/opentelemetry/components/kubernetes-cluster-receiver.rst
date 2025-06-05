.. _kubernetes-cluster-receiver:

****************************************
Kubernetes cluster receiver
****************************************

.. meta::
      :description: The Kubernetes cluster receiver allows the Splunk Distribution of OpenTelemetry Collector to collect cluster metrics from Kubernetes through its monitoring API.

The Kubernetes cluster receiver collects cluster metrics using the Kubernetes API server. You can use a single instance of this receiver to monitor an entire Kubernetes cluster. The supported pipeline type is ``metrics``. To filter in or out other Kubernetes elements, such as containers, pods, nodes, namespaces, or clusters, use the Filter processor instead. Learn more at :ref:`filter-processor`. See :ref:`otel-data-processing` for more information on the different types of pipelines.

Kubernetes version 1.21 and higher is compatible with the Kubernetes navigator. Using lower versions of Kubernetes is not supported for this receiver and might result in the navigator not displaying all clusters.

.. note:: This receiver replaces the ``kubernetes-cluster`` Smart Agent monitor.

Get started
======================

By default, the Kubernetes cluster receiver is already activated in the Helm chart of the Splunk OpenTelemetry Collectors. See :ref:`otel-kubernetes-config` for more information, including the default :ref:`ootb-metrics-k8s`.

To activate the Kubernetes cluster receiver manually in the Collector configuration, add ``k8s_cluster`` to the ``receivers`` section of your configuration file, as shown in the following example:

.. code:: yaml

   receivers:
     k8s_cluster:
       auth_type: kubeConfig
       collection_interval: 30s
       node_conditions_to_report: ["Ready", "MemoryPressure"]
       allocatable_types_to_report: ["cpu","memory"]
       metadata_exporters: [signalfx]

To complete the configuration, include the receiver in the ``metrics`` pipeline of the ``service`` section of your
configuration file. For example:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [k8s_cluster]

Sync metadata_exporters
-----------------------------

Use ``metadata_exporters`` as a list of metadata exporters to sync with metadata collected by the Kubernetes cluster receiver. For example:

.. code-block:: yaml


   receivers:
     k8s_cluster:
       auth_type: serviceAccount
       metadata_exporters:
       - signalfx

Exporters specified in this list need to implement the following interface. If an exporter doesn't implement the interface, startup fails.

.. code:: yaml

   type MetadataExporter interface {
     ConsumeMetadata(metadata []*MetadataUpdate) error
   }

   type MetadataUpdate struct {
     ResourceIDKey string
     ResourceID    ResourceID
     MetadataDelta
   }

   type MetadataDelta struct {
     MetadataToAdd    map[string]string
     MetadataToRemove map[string]string
     MetadataToUpdate map[string]string
   }

Set node_conditions_to_report
-----------------------------------

Use the following configuration to have the ``k8s_cluster`` receiver emit two metrics, ``k8s.node.condition_ready`` and ``k8s.node.condition_memory_pressure``, one for each condition in the configuration:

.. code:: yaml

   # ...
   k8s_cluster:
     node_conditions_to_report:
       - Ready
       - MemoryPressure
   # ...

The value is ``1`` if the ``ConditionStatus`` for the corresponding ``Condition`` is ``True``, ``0`` if it's ``False``, and ``-1`` if it's ``Unknown``. To learn more, search for “Conditions” in the Kubernetes documentation.

Settings
======================

The following table shows the configuration options for the receiver:

.. raw:: html

   <div class="metrics-standard" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/cfg-metadata/receiver/k8s_cluster.yaml"></div>

Metrics
=====================

.. caution:: The SignalFx exporter excludes some available metrics by default. Learn more about default metric filters in :ref:`list-excluded-metrics`. See :ref:`ootb-metrics-k8s` to see how the Collector processes Kubernetes metrics.

The following metrics, resource attributes, and attributes are available. For more information, including which resource attributes are included by default, refer to :new-page:`Kubernetes Cluster receiver metadata <https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/03e370a1bfbe8fbd24b0cc35c0756c8ba688e46c/receiver/k8sclusterreceiver/metadata.yaml>` in GitHub.

.. raw:: html

   <div class="metrics-component" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/metric-metadata/k8sclusterreceiver.yaml"></div>



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



