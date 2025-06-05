.. _kubernetes-events:

Kubernetes events
=================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Kubernetes events monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of OpenTelemetry Collector uses the Smart Agent receiver with the
``kubernetes-events`` monitor type to listen for Kubernetes events. The
integration calls the Kubernetes API running on manager nodes, and sends
Kubernetes events into Splunk Observability Cloud as Infrastructure
Monitoring events through the OpenTelemetry pipeline.

After it starts, the Kubernetes events monitor type sends all of the
events that Kubernetes has that are still persisted, and any new events
as they come in. The various agents decide which instance will lead and
sends event. If ``alwaysClusterReporter`` is set to ``true``, every node
emits the same data, and there is no additional querying of the manager
node.

This monitor type is available on Kubernetes, Linux, and Windows.

Benefits
--------



.. raw:: html

   <div class="include-start" id="benefits-events.rst"></div>

.. include:: /_includes/benefits-events.rst

.. raw:: html

   <div class="include-stop" id="benefits-events.rst"></div>




Installation
------------



.. raw:: html

   <div class="include-start" id="collector-installation.rst"></div>

.. include:: /_includes/collector-installation.rst

.. raw:: html

   <div class="include-stop" id="collector-installation.rst"></div>




Deploy with Helm
~~~~~~~~~~~~~~~~

To activate this monitor with the Helm chart, include this argument with
the helm install command:

.. code-block:: yaml

   -set splunkObservability.infrastructureMonitoringEventsEnabled='true'

Deploy without Helm
~~~~~~~~~~~~~~~~~~~

To deploy without Helm, include the following in the OTel configuration:

.. code-block:: yaml

   processors:
     resource/add_event_k8s:
       attributes:
         - action: insert
           key: kubernetes_cluster
           value: CHANGEME

   receivers:
     smartagent/kubernetes-events:
      type: kubernetes-events
      alwaysClusterReporter: true

   service:
     pipelines:
       logs/events:
         exporters:
           - signalfx
         processors:
           - memory_limiter
           - batch
           - resourcedetection
           - resource/add_event_k8s
         receivers:
           - smartagent/kubernetes-events        

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
      smartagent/kubernetes-events:
      type: kubernetes-events
      ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code-block:: yaml

   services:
     logs/events:
       receivers:
         - smartagent/kubernetes-events

Lastly, configure which events to send. You can see the types of events
happening in your cluster with the following command:

.. code-block:: yaml

   kubectl get events -o yaml --all-namespaces

To send all events, set the option ``_sendAllEvents`` to ``true`` in
your values.yaml, and remove the ``whitelistedEvents`` option.

From the output, combine **Reason** (Started, Created, Scheduled) and
**Kind** (Pod, ReplicaSet, Deployment…) to select which events to send.
- Specify a single **reason** and **involveObjectKind** individually for
each event rule you want to allow. - Events are placed in the
``whitelistedEvents`` configuration option as a list of events you want
to send. - Event names will match the reason name.

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``kubernetesAPI``
      - no
      - ``object (see below)``
      - Configuration of the Kubernetes API client.
   - 

      - ``whitelistedEvents``
      - no
      - ``list of objects (see below)``
      - A list of event types to send events for. Only events matching
         these items will be sent.
   - 

      - ``alwaysClusterReporter``
      - no
      - ``bool``
      - Whether to always send events from this agent instance or to do
         leader election to only send from one agent instance.
         **Default** is ``false``.

The **nested** ``kubernetesAPI`` config object has the following fields:

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``authType``
      - no
      - ``string``
      - To authenticate to the K8s API server: - ``none`` for no
         authentication. - ``tls`` to use manually specified TLS client
         certs (not recommended). - ``serviceAccount`` to use the
         standard service account token provided to the agent pod. -
         ``kubeConfig`` to use credentials from ``~/.kube/config``. -
         **Default** is ``serviceAccount``.
   - 

      - ``skipVerify``
      - no
      - ``bool``
      - Whether to skip verifying the TLS certificate from the API
         server. Almost never needed. **Default** is ``false``
   - 

      - ``clientCertPath``
      - no
      - ``string``
      - The path to the TLS client certificate on the pod's filesystem,
         if using ``tls`` authentication.
   - 

      - ``clientKeyPath``
      - no
      - ``string``
      - The path to the TLS client key on the pod's filesystem, if using
         ``tls`` authentication.
   - 

      - ``caCertPath``
      - no
      - ``string``
      - Path to a CA certificate to use when verifying the API server
         TLS certificate. Generally this is provided by K8s alongside
         the service account token, which will be picked up
         automatically, so this should rarely be necessary to specify.

The **nested** ``whitelistedEvents`` configuration object has the
following fields:

.. list-table::
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
   - 

      - ``reason``
      - no
      - ``string``
   - 

      - ``involvedObjectKind``
      - no
      - ``string``

Example YAML configuration:

.. code-block:: yaml

   receivers:
      smartagent/kubernetes-events:
        type: kubernetes-events
        whitelistedEvents:
          - reason: Created
            involvedObjectKind: Pod
          - reason: SuccessfulCreate
            involvedObjectKind: ReplicaSet

Troubleshooting
---------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



