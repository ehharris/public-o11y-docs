.. _get-started-istio:

************************************************************
Send traces from Istio to Splunk Observability Cloud
************************************************************

.. meta::
   :description: Send telemetry from your Istio service mesh to Splunk Observability Cloud.

Istio 1.8 and higher has native support for observability. You can configure your Istio service mesh to send traces, metrics, and logs to Splunk Observability Cloud by configuring the Splunk OpenTelemetry Collector and Istio.

Requirements
==============================



.. raw:: html

   <div class="include-start" id="requirements/istio.rst"></div>

.. include:: /_includes/requirements/istio.rst

.. raw:: html

   <div class="include-stop" id="requirements/istio.rst"></div>




Install and configure the Splunk OpenTelemetry Collector
=============================================================

Deploy the Splunk OpenTelemetry Collector for Kubernetes in host monitoring (agent) mode. The required Collector components depend on product entitlements and the data you want to collect. See :ref:`otel-install-k8s`.

In the Helm chart for the Collector, set the ``autodetect.istio`` parameter to ``true`` by passing ``--set autodetect.istio=true`` to the ``helm install`` or ``helm upgrade`` commands.

You can also add the following snippet to your  values.yaml file, which you can pass using the ``-f myvalues.yaml`` argument:

.. code:: yaml

   autodetect:
      istio: true
   
Ensure that data forwarding doesn't generate telemetry
----------------------------------------------------------

Forwarding telemetry from Istio to the Collector might generate undesired telemetry. To avoid this, do one of the following:

- Run the Collector in a separate namespace that lacks the Istio proxy.
- Add a label to the Collector pods to prevent the injection of the Istio proxy. This is default configuration when the ``autodetect.istio`` parameter is set to ``true``.
- If you need the Istio proxy in the Collector pods, deactivate tracing in the Collector pods. For example:

   .. code-block:: yaml

      # ...
      otelK8sClusterReceiver:
         podAnnotations:
            proxy.istio.io/config: '{"tracing":{}}'
      otelCollector:
         podAnnotations:
            proxy.istio.io/config: '{"tracing":{}}'

.. note:: 
   The instrumentation pod is a DaemonSet and isn't injected with a proxy by default. If Istio injects proxies in instrumentation pods, deactivate tracing using a ``podAnnotation``.

Configure the Istio Operator
=============================================

Configure the Istio Operator following these steps:

- Set an ``environment.deployment`` attribute.
- Configure the Zipkin tracer to send data to the Splunk OpenTelemetry Collector running on the host. 

For example:

.. code-block:: yaml

   # tracing.yaml

   apiVersion: install.istio.io/v1alpha1
   kind: IstioOperator
   metadata:
   name: istio-operator
   spec:
      meshConfig:
         # Requires Splunk Log Observer
         accessLogFile: /dev/stdout
         # Requires Splunk APM
         enableTracing: true
         defaultConfig:
            tracing:
               max_path_tag_length: 99999
               sampling: 100
               zipkin:
                  address: $(HOST_IP):9411
               custom_tags:
                  environment.deployment:
                     literal:
                        value: dev

To activate the new configuration, run:

.. code-block:: shell

   istioctl install -f ./tracing.yaml

Restart the pods that contain the Istio proxy to activate the new tracing configuration.

Update all pods in the service mesh
===========================================

Update all pods that are in the Istio service mesh to include an ``app`` label. Istio uses this to define the Splunk service.

.. note:: 
   If you don't set the ``app`` label, identifying the relationship between the proxy and your service is more difficult.

Recommendations
=========================================

To make the best use of full-fidelity data retention, configure Istio to send as much trace data as possible by configuring sampling and maximum tag length as follows:

- Set a ``sampling`` value of ``100`` to ensure that all traces have correct root spans.
- Set a ``max_path_tag_length`` value of ``99999`` to avoid that key tags get truncated.

For example:

.. code-block:: yaml
   :emphasize-lines: 13,14

   # tracing.yaml

   apiVersion: install.istio.io/v1alpha1
   kind: IstioOperator
   metadata:
   name: istio-operator
   spec:
      meshConfig:
         # Requires Splunk Log Observer
         accessLogFile: /dev/stdout
         # Requires Splunk APM
         enableTracing: true
         defaultConfig:
            tracing:
               max_path_tag_length: 99999
               sampling: 100
               zipkin:
                  address: $(HOST_IP):9411
               custom_tags:
                  environment.deployment:
                     literal:
                        value: dev

For more information on how to configure Istio see the Istio distributed tracing installation documentation.



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



