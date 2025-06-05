.. _k8s-third-party:

***************************************************************************************
Automatic discovery for third-party services in Kubernetes
***************************************************************************************

.. meta:: 
    :description: Learn how to use automatic discovery to send data from your third-party applications in Kubernetes to Splunk Observability Cloud.

You can use automatic discovery to find third-party applications (such as databases and web servers) running in your Kubernetes environment. Automatic discovery gathers telemetry data from these applications and sends it to Splunk Observability Cloud.

.. note:: 
  
  Update the Collector to version 0.94.0 and higher to activate automatic service discovery.

  If you're using the Collector version 0.109.0 or higher, log in to Splunk Observability Cloud and go to the :guilabel:`Discovered services` tab in the :guilabel:`Data Management` menu to see which of your services and third-party applications the Collector has detected and can monitor automatically.   See :ref:`discovery-mode-k8s` for more details.

How automatic discovery works
================================================

When you run the Collector with automatic discovery, it tests built-in configurations for supported metric receivers against endpoints discovered on your platform by observer extensions. This happens before starting the Collector service.

For any dynamically instantiated receiver that retrieves metrics matching the success criteria, the Collector translates the discovery configuration to a receiver creator instance with the known working rules, as well as the required observer extension. See :ref:`receiver-creator-receiver` for more information. At the same time, the Collector adds the configuration to the ``metrics`` pipeline at runtime.

For any receiver that can establish a connection with a service, but not receive the expected metrics, discovery mode suggests which properties to set, or what extensions or settings to configure on the service to successfully retrieve telemetry. You can define any target-specific configuration values that are required, for example authentication information, using discovery properties to tune the discovery process.

When running in Kubernetes, discovery mode tests bundled metric receiver configurations against the endpoints discovered by the ``k8s_observer`` observer. Successfully discovered instances are then incorporated in the existing service configuration.

.. _discovery-mode-k8s:

Deploy the Collector with automatic discovery
=================================================

You can configure the DaemonSet from the Splunk Distribution of the OpenTelemetry Collector for Kubernetes to run in discovery mode. Edit the properties to add required credentials or service-specific information.

.. note:: If you're using the Collector version 0.109.0 or higher, activate auto-discovery in the UI using ``featureGates: splunk.continuousDiscovery``.    

The following example shows how to activate discovery mode in the Helm chart and adds authentication properties for PostgreSQL service discovery:

.. code-block:: yaml

   agent:

     #...

     discovery:
       enabled: true # Turned off by default
       properties:
         extensions:
           k8s_observer:
             config:
               auth_type: serviceAccount  # Default auth_type value
         receivers:
           postgres:
             config:
               # Environment variables populated by secret data
               username: '${env:POSTGRES_USER}'
               password: '${env:POSTGRES_PASSWORD}'
               tls:
                 insecure: true
     # Activates auto discovery in UI. Only available for the Collector version 0.109.0 or higher
     featureGates: splunk.continuousDiscovery             

   # ...

   extraEnvs:
      # Environment variables using a manually created secret
      - name: POSTGRES_USER
        valueFrom:
          secretKeyRef:
            name: postgres-monitoring
            key: username
      - name: POSTGRES_PASSWORD
        valueFrom:
          secretKeyRef:
            name: postgres-monitoring
            key: password
       

To check discovery progress and statement evaluations, see the agent startup logs or use kubectl. For example:

.. code-block:: shell

   $ kubectl -n monitoring logs splunk-otel-collector-agent | grep -i disco
   Discovering for next 10s...
   Successfully discovered "postgresql" using "k8s_observer" endpoint "k8s_observer/e8a10f52-4f2a-468c-be7b-7f3c673b1c8e/(5432)".
   Discovery complete.

.. note:: By default, the ``docker_observer`` and ``host_observer`` extensions are turned off for discovery in the Helm chart.