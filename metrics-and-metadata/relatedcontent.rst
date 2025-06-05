.. _get-started-relatedcontent:

*****************************************************************
Related Content in Splunk Observability Cloud
*****************************************************************

.. meta::
   :description: Related Content functionality: introduction, requirements, how to use.

The Related Content panel automatically correlates and presents data between different views within Splunk Observability Cloud. In one view of Splunk Observability Cloud, such as APM, the Related Content bar shows and links to related content in other views. 

Splunk Cloud Platform also has the Related Content panel where users can see previews of observability data that correlates to search results in the Search app. For more information, see :new-page:`Preview Splunk Observability Cloud data <https://quickdraw.splunk.com/redirect/?product=Observability&location=related.content.observability&version=current>`.

Use Related Content 
==========================================================================================================

Select tiles in the Related Content bar to seamlessly navigate from one view to another in Splunk Observability Cloud. The following animation shows a user navigating from APM to Infrastructure Monitoring to Log Observer.

..  image:: /_images/get-started/Related1.gif
    :alt: Using Related Content in Splunk Observability Cloud.
    :width: 90%

In the preceding example, the user navigates through the following sequence:

1. The user starts in APM by exploring the service dependency map. They select the :strong:`Frontend` service because it shows a high error rate.

   In the Related Content bar at the bottom of the screen, the user sees an Infrastructure tile showing related EC2 instances and selects it. Results are grouped by component. For example, Infrastructure, Logs, APM. Hovering over the tile indicates whether there are any Related Content results to view.

2. Splunk Observability Cloud takes the user to Infrastructure where they select the first EC2 instance because it shows the highest CPU utilization. 

   In the Related Content bar, the user sees a tile showing logs related to the EC2 instance, so they click it.

3. Splunk Observability Cloud takes the user to Log Observer where they can drill down into the related logs to find the root cause of the problem.

.. note:: Related Content is different from data links, a separate capability, which lets you dynamically transfer contextual information about the property you're viewing to the resource, helping you get to relevant information faster. To learn more about data links, see :ref:`apm-create-data-links`.

.. _related-logs-in-apm:

Related logs in APM 
-----------------------------------------------------------------
Splunk APM displays related logs in the Related Content bar by default. To save resources, Unified Identity and Log Observer Connect customers can deactivate the display of related logs in Splunk APM. To deactivate Related Content for logs in APM, an administrator must go to :strong:`Settings` then select :strong:`General Settings` and toggle the switch to :strong:`Deactivate Related Content for logs`.

If you want to display Related Content for logs in APM again, go to :strong:`Settings`, select :strong:`General Settings`, and toggle the switch to :strong:`Activate Related Content for logs`. 

Where can I see Related Content?
-----------------------------------------------------------------

The following table describes when and where in Splunk Observability Cloud you can see Related Content:

.. list-table::
   :header-rows: 1
   :widths: 50, 50

   * - :strong:`Starting Point`
     - :strong:`Possible Destinations`

   * - APM services
     - Related Kubernetes clusters filtered by service, AWS EC2s, GCP GCEs, Azure VMs, all log lines for the service

   * - Database service
     - Related database host or instance

   * - Database instance
     - Related Database Query Performance, related APM services

   * - Host or Cloud compute instance (AWS EC2, GCP GCE, Azure VM)
     - Related APM services, log lines for the specific instance

   * - Kubernetes cluster, node, pod, container
     - Related log lines for the node

   * - Kubernetes pod or container
     - Related APM service in that pod or container, log lines for that pod or container

   * - Specific log line
     - Related APM service, trace, Kubernetes node/pod/container, Host or compute instance (AWS EC2, GCP GCE, Azure VM)

   * - Specific trace ID
     - Related log line

.. _relatedcontent-collector:

Use the Splunk Distribution of the OpenTelemetry Collector to enable Related Content
==========================================================================================================

Splunk Observability Cloud uses OpenTelemetry to correlate telemetry types. To do this, your telemetry field names or metadata key names must exactly match the metadata key names used by both OpenTelemetry and Splunk Observability Cloud.

Related Content works out-of-the-box when you deploy the Splunk Distribution of the OpenTelemetry Collector with its default configuration to send your telemetry data to Splunk Observability Cloud. With the default configuration the Collector automatically maps your metadata key names correctly. To learn more about the Collector, see :ref:`otel-intro`.

.. caution:: 
  
  If you don't use the Splunk Distribution of OpenTelemetry Collector, or you use a non-default configuration, or you use non-Splunk OpenTelemetry, your telemetry data might have metadata key names that are not consistent with those used by Splunk Observability Cloud and OpenTelemetry, and Related Content might not work. 
  
  If you're experiencing issues with Related Content, verify your metadata key names, and update them if necessary.

Configure the Collector to enable APM Related Content 
-----------------------------------------------------------------

The APM service dashboards include charts that indicate the health of the underlying infrastructure. The default configuration of the Splunk Distribution of the OpenTelemetry Collector automatically configures this for you, but if you're using a custom configuration, read :ref:`relatedcontent-collector-apm`.

Metadata compatibility example
-----------------------------------------------------------------

For example, say Splunk Observability Cloud receives the following telemetry data:

- Splunk APM receives a trace with the metadata key ``trace_id:2b78e7c951497655``

- Splunk Log Observer receives a log with the metadata key ``trace.id:2b78e7c951497655``

Although these refer to the same trace ID value, the log and the trace cannot be correlated in Splunk Observability Cloud because the field names, ``trace_id`` and ``trace.id``, do not match. 

To solve this, rename your log metadata key ``trace.id`` to ``trace_id`` using the field copy processor in Logs Pipeline Management. Alternatively, you can re-instrument your log collection to make metadata key names align. 

When the field names in APM and Log Observer match, the trace and the log with the same trace ID value can be correlated in Splunk Observability Cloud. Then, when you are viewing the trace in APM, you can select directly into the log with the same trace ID value and view the correlated log in Log Observer.

.. _relatedcontent-required-components:

Required Collector components
=================================================================

If you're using the Splunk Distribution of the OpenTelemetry Collector, any other distribution of the Collector, or the :ref:`upstream Collector <using-upstream-otel>` and want to ensure Related Content in Splunk Observability Cloud behaves correctly, verify that the  SignalFx exporter is included in your configuration. This exporter aggregates the metrics from the ``hostmetrics`` receiver and must be enabled for the ``metrics`` and ``traces`` pipelines. 

The Collector uses the correlation flag of the SignalFx exporter to make relevant API calls to correlate your spans with the infrastructure metrics. This flag is enabled by default. To adjust the correlation option further, see the SignalFx exporter's options at :ref:`signalfx-exporter-settings`.

.. _relatedcontent-required-fields:

Required metadata fields
=================================================================

Related Content relies on specific metadata that allow APM, Infrastructure Monitoring, and Log Observer to pass filters around Splunk Observability Cloud. 

The following sections list the metadata key names required to enable Related Content for each view in Splunk Observability Cloud. If your data does not have the field names listed here, Splunk Observability Cloud cannot correlate your related data.

Splunk APM
-----------------------------------------------------------------

To enable Related Content for APM use one of these span tags:

- ``service.name``
- ``trace_id``

Optionally, you can also use ``deployment.environment`` with ``service.name``.

The default configuration of the Splunk Distribution of the OpenTelemetry Collector already provides these span tags. To ensure full functionality of Related Content, do not change any of the metadata key names or span tags provided by the Splunk OTel Collector. 

Learn more:

* About span tags in Splunk APM, see :ref:`apm-traces-spans`.
* About deployment environments in Splunk APM, see :ref:`apm-environments`.

Leverage Related Content for APM and pod-specific Kubernetes data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For a Related Content tile in APM to link to data for a specific Kubernetes pod (``k8s.pod.name``), you must first filter on a specific Kubernetes cluster (``k8s.cluster.name``). APM cannot guarantee an accurate Related Content Kubernetes pod destination in Infrastructure Monitoring without both values because Kubernetes pod names are not required to be unique across clusters.

For example, consider a scenario in which Related Content needs to return data for a Kubernetes pod named :strong:`Pod-B`. As shown the following diagram, a Kubernetes implementation can have multiple pods with the same name. For Related Content to return the data for the correct :strong:`Pod-B`, you must also provide the name of the Kubernetes cluster the pod resides in. In this case, that name would be either :strong:`Cluster-West` or :strong:`Cluster-East`. This combination of filtering on cluster and pod names creates the unique combination that Related Content needs to link to the correct pod data in Infrastructure Monitoring.

.. source in figma: https://www.figma.com/file/sOEa3q92WJxB4uWb3Poftg/related-content-apm-k8s-constraint?node-id=0%3A1

.. image:: /_images/get-started/k8s-clusters-pods.png
  :width: 80%
  :alt: This diagram shows two uniquely named Kubernetes clusters, each containing pods that share names across clusters.

.. _enablerelatedcontent-imm:

Splunk Infrastructure Monitoring
-----------------------------------------------------------------

To enable Related Content for IM use one of these metadata combinations:

-  ``host.name``. It falls back on ``host``, ``aws_private_dns_name`` (AWS), ``instance_name`` (GCP), ``azure_computer_name`` (Azure)
- ``k8s.cluster.name``
- ``k8s.cluster.name`` + ``k8s.node.name``
- ``k8s.cluster.name`` + ``k8s.node.name`` (optional) + ``k8s.pod.name``
- ``k8s.cluster.name`` + ``k8s.node.name`` (optional) + ``k8s.pod.name`` (optional) + ``container.id``
- ``service.name``
- ``service.name`` + ``deployment.environment`` (optional) + ``k8s.cluster.name`` (optional)

If you're using the default configuration of the Splunk Distribution of the OpenTelemetry Collector for Kubernetes, the required Infrastructure Monitoring metadata is provided. See more at :ref:`otel-install-k8s`.

If you're using other distributions of the OpenTelemetry Collector or non-default configurations of the Splunk Distribution to collect infrastructure data, Related Content won't work out of the box.

.. _relatedcontent-log-observer:

Splunk logs
-----------------------------------------------------------------

To enable Related Content for logs use one of these fields:

- ``host.name``
- ``service.name``
- ``span_id``
- ``trace_id``

To ensure full functionality of both Log Observer and Related Content, verify that your log events fields are correctly mapped. Correct log field mappings enable built-in log filtering, embed logs in APM and Infrastructure Monitoring functionality, and enable fast searches as well as the Related Content bar.

If the key names in the preceding list use different names in your log fields, remap them to the key names listed here. For example, if you don't see values for :strong:`host.name` in the Log Observer UI, check to see whether your logs use a different field name, such as :strong:`host_name`. If your logs do not contain the default field names exactly as they appear in the preceding list, remap your logs using one of the methods in the following section. 



.. raw:: html

   <div class="include-start" id="log-observer-transition.rst"></div>

.. include:: /_includes/log-observer-transition.rst

.. raw:: html

   <div class="include-stop" id="log-observer-transition.rst"></div>




Kubernetes log fields
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The Splunk Distribution of the OpenTelemetry Collector injects the following fields into your Kubernetes logs. Do not modify them if you want to use Related Content. 

- ``k8s.cluster.name``
- ``k8s.node.name``
- ``k8s.pod.name``
- ``container.id``
- ``k8s.namespace.name``
- ``kubernetes.workload.name``

Use one of these tag combinations to enable Related Content:

- ``k8s.cluster.name`` + ``k8s.node.name``
- ``k8s.cluster.name`` + ``k8s.node.name`` (optional) + ``k8s.pod.name``
- ``k8s.cluster.name`` + ``k8s.node.name`` (optional) + ``k8s.pod.name`` (optional) + ``container.id``

Learn more about the Collector for Kubernetes at :ref:`collector-kubernetes-intro`.

.. _remap-log-fields:

Remap log fields
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following table describes the four methods for remapping log fields:

.. list-table::
   :header-rows: 1
   :widths: 50 50

   * - :strong:`Remapping Method`
     - :strong:`Instructions`

   * - Log Field Aliasing
     - Create and activate a field alias. See :ref:`logs-alias` to learn how. Learn when to use Log Field Aliasing in the next section.

   * - Client-side
     - Configure your app to remap the necessary fields.

When to use Log Field Aliasing
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use Log Field Aliasing to remap fields in Splunk Observability Cloud when you cannot or do not want to create a copy processor because any of the following are true:

- You use Log Observer Connect to get logs data and do not have access to Log Observer Pipeline Management.
- You do not want to use indexing capacity by creating additional log processing rules.
- You do not want to transform your data at index time.
- You want the new alias to affect every log message, even those that came in from a time before you created the alias.

How to change your metadata key names
=================================================================

Change metric and traces names
-----------------------------------------------------------------

Use the Splunk Distribution of the OpenTelemetry Collector to ensure that your metrics and traces have the metadata key names required to use Splunk Observability Cloud's Related Content feature. If you did not use the Collector and your metrics or traces do not include the required metadata key names, you can instrument your applications and serverless functions to include them. See the following pages to learn how:

- :ref:`get-started-application`
- :ref:`instrument-serverless-functions`
- :ref:`rum-gdi`

Change log names
-----------------------------------------------------------------

If the required key names use different names in your log fields, remap them using one of the methods listed in :ref:`remap-log-fields`.













