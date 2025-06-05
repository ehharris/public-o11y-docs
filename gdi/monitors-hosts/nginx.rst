.. _nginx:

NGINX
=====

.. meta::
   :description: Use this Splunk Observability Cloud integration for the NGINX monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the ``nginx`` monitor type to retrieve metrics from NGINX instances.

This integration is available on Linux and Windows.

.. note:: To monitor NGINX instances with the OpenTelemetry Collector using native OpenTelemetry refer to the :ref:`nginx-receiver` component.

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




Example
~~~~~~~

To activate this integration, add the following to your Collector
configuration:

.. code:: yaml

   receivers:
     smartagent/nginx:
       type: collectd/nginx
       host: <host>
       port: <port>
       ...  # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/nginx]

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for this monitor:

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``host``
      - Yes
      - ``string``
      - Hostname or IP address of the NGINX instance. For example,
         ``127.0.0.1``.
   - 

      - ``port``
      - Yes
      - ``integer``
      - The port of the NGINX instance. For example, ``8080``.
   - 

      - ``name``
      - no
      - ``string``
      - Name of the NGINX instance.
   - 

      - ``url``
      - no
      - ``string``
      - URL of the status endpoint. The default value is
         ``http://{{.Host}}:{{.Port}}/nginx_status``, which takes the
         values defined in ``host`` and ``port``.
   - 

      - ``username``
      - no
      - ``string``
      - Username for HTTP basic authentication, if needed.
   - 

      - ``password``
      - no
      - ``string``
      - Password for HTTP basic authentication, if needed.
   - 

      - ``timeout``
      - no
      - ``integer``
      - Timeout in seconds for the request. The default value is ``0``.

Nginx configuration
~~~~~~~~~~~~~~~~~~~

You can configure NGINX to expose status information by editing the
NGINX configuration. See ``ngx_http_stub_status_module`` on the NGINX
documentation site.

After you've set up the Collector, follow these steps to configure the
Nginx web server to expose status metrics:

1. Add the following configuration to your Nginx server. The default
   nginx server configuration is at /etc/nginx/sites-enabled/default.

   ::

      server {
        location /nginx_status {
          stub_status on;
          access_log off;
          allow 127.0.0.1; # The source IP address of OpenTelemetry Collector.
          deny all;
        }
      }

2. Restart the Nginx web server.

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/collectd/nginx/metadata.yaml"></div>


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



