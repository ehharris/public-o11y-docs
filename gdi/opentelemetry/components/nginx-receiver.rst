.. _nginx-receiver:

***********************
NGINX receiver
***********************

.. meta::
      :description: The NGINX receiver fetches stats from a NGINX instance using the ``ngx_http_stub_status_module`` module's status endpoint.

The NGINX receiver fetches stats from a NGINX instance using the ``ngx_http_stub_status_module`` module's ``status`` endpoint. The supported pipeline type is ``metrics``. See :ref:`otel-data-processing` for more information. 

You need to configure NGINX to expose status information. To learn how, see the :new-page:`HTTP status module config guide <https://nginx.org/en/docs/http/ngx_http_stub_status_module.html>` in the NGINX documentation.

.. note:: Out-of-the-box dashboards and navigators aren't supported for the NGINX Server receiver yet, but are planned for a future release.

Get started
======================

Follow these steps to configure and activate the component:

1. Deploy the Splunk Distribution of the OpenTelemetry Collector to your host or container platform:
   
   - :ref:`otel-install-linux`
   - :ref:`otel-install-windows`
   - :ref:`otel-install-k8s`

2. Configure the receiver as described in the next section.
3. Restart the Collector.

Sample configuration
---------------------------

To activate the NGINX receiver, add ``nginx`` to the ``receivers`` section of your configuration file, as shown in the following example:

.. code:: yaml

  receivers:
    nginx:
      endpoint: "http://localhost:80/status"
      collection_interval: 10s

To complete the configuration, include the receiver in the ``metrics`` pipeline of the ``service`` section of your configuration file. For example:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [nginx]

Configuration options
-------------------------------------------------

The following settings are available:

* ``endpoint``. :strong:`Required`. ``http://localhost:80/status`` by default. The URL of the NGINX status endpoint.



.. raw:: html

   <div class="include-start" id="gdi/collector-settings-collectioninterval.rst"></div>

.. include:: /_includes/gdi/collector-settings-collectioninterval.rst

.. raw:: html

   <div class="include-stop" id="gdi/collector-settings-collectioninterval.rst"></div>






.. raw:: html

   <div class="include-start" id="gdi/collector-settings-initialdelay.rst"></div>

.. include:: /_includes/gdi/collector-settings-initialdelay.rst

.. raw:: html

   <div class="include-stop" id="gdi/collector-settings-initialdelay.rst"></div>




Settings
======================

The following table shows the configuration options for the NGINX receiver:

.. raw:: html

   <div class="metrics-standard" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/cfg-metadata/receiver/nginx.yaml"></div>

Metrics
=====================

The following metrics, resource attributes, and attributes are available.

.. raw:: html

   <div class="metrics-component" category="included" url="https://raw.githubusercontent.com/splunk/collector-config-tools/main/metric-metadata/nginxreceiver.yaml"></div>

.. caution:: By default the NGINX receiver doesn't provide the following metrics: ``nginx_connections.reading``, ``nginx_connections.waiting``, and ``nginx_connections.writing``.

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



