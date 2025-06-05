.. _collectd-php-fpm:

PHP FPM
=======

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Collectd PHP-FastCGI Process Manager FPM monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of OpenTelemetry Collector uses the Smart Agent receiver with the
``collectd/php-fpm`` monitor type to monitor PHP-FastCGI Process Manager
(FPM) using the pool status URL.

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




Install PHP
~~~~~~~~~~~

To configure the PHP-FPM service itself to expose status metrics, follow
these steps:

1. Activate the status path. See the PHP documentation for more
   information.

2. Configure access through the web server. The following example shows
   how to configure access for NGINX:

   ::

       location ~ ^/(status|ping)$ {
         access_log off;
         fastcgi_pass unix:/run/php/php-fpm.sock;
         include fastcgi_params;
         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
       }

3. Restart both the web server and PHP-FPM.

Make sure that the URL you provide to reach the FPM status page through
your web server ends in ``?json``. This returns the metrics as ``json``,
which this plugin requires.

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
     smartagent/ collectd/php-fpm:
       type: collectd/php-fpm
       ... # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/collectd/php-fpm]

Advanced configuration example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

See the following config options:

.. code:: yaml

   receivers:
     smartagent/ collectd/php-fpm:
       type: collectd/php-fpm
       host: localhost
       port: 80
       useHTTPS: true # will be ignored
       url: "http://{{.host}}:{{.port}}/fpm-status?json"    
       ... # Additional config

Configuration settings
~~~~~~~~~~~~~~~~~~~~~~

The following table shows the configuration options for
``collectd/php-fpm``:

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
      - no
      - ``string``
      - The host name of the web server. For example, ``127.0.0.1``.
   - 

      - ``port``
      - no
      - ``integer``
      - The port number of the web server. For example, ``80``. The
         default value is ``0``.
   - 

      - ``useHTTPS``
      - no
      - ``bool``
      - Whether the monitor connects to Supervisor using HTTPS instead
         of HTTP. The default value is ``false``.
   - 

      - ``path``
      - no
      - ``string``
      - The scrape URL for Supervisor. The default value is ``/status``.
   - 

      - ``url``
      - no
      - ``string``
      - URL or Go template that to be populated with the ``host``,
         ``port``, and ``path`` values.
   - 

      - ``name``
      - no
      - ``string``
      - The ``plugin_instance`` dimension. It can take any value.

Metrics
-------

The following metrics are available for this integration:

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/collectd/php/metadata.yaml"></div>


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



