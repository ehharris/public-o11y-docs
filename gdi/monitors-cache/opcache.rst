.. _opcache:

OPcache
=======

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Collectd OPcache monitor. See benefits, install, configuration, and metrics

The Splunk Distribution of the OpenTelemetry Collector uses the Smart Agent receiver with the ``collectd/opcache`` monitor type to retrieve metrics from OPcache using the ``opcache_get_status()`` function, which improves PHP performance by storing precompiled script bytecode in shared memory.

This integration is available on Kubernetes and Linux.

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




PHP setup
~~~~~~~~~

Install the following PHP script on your PHP web server:

.. code:: php

   <?php
   header('Content-Type: application/json');
   $status=opcache_get_status();
   echo json_encode($status,JSON_PRETTY_PRINT);

The following is an example using NGINX:

.. code-block:: yaml

   location ~ /monitoring/.*\.php$ {
       #access_log off;
       allow 127.0.0.1;
       allow ::1;
       deny all;
       include fastcgi_params;
       fastcgi_split_path_info ^(.+\.php)(/.*)$;
       fastcgi_param  PHP_ADMIN_VALUE "open_basedir=/var/log:/usr/bin:/srv/http/monitoring";
       fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
       fastcgi_pass php-handler;
   }

Put the ``opcache_stat.php`` script in ``/srv/http/monitoring`` and run
the following command:

.. code:: bash

   curl http://localhost/monitoring/opcache_stat.php
   {
   "opcache_enabled": true,
   "cache_full": false,
   "restart_pending": false,
   "restart_in_progress": false,
   "memory_usage": {
       "used_memory": 82614848,
       "free_memory": 183437232,
       "wasted_memory": 2383376,
       "current_wasted_percentage": 0.88787674903869629
   },
   #...

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
     smartagent/opcache:
       type: collectd/opcache
       ...  # Additional config

Next, add the monitor to the ``service.pipelines.metrics.receivers``
section of your configuration file:

.. code:: yaml

   service:
     pipelines:
       metrics:
         receivers: [smartagent/opcache]

Example: Configure an endpoint
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the status script results in an endpoint other than
``/opcache_stat.php``, use the ``path`` configuration option as in the
following example:

.. code:: yaml

   monitors:
    - type: collectd/opcache
      host: localhost
      port: 80
      path: "/opcache"

You can also define the full URL using the ``url`` configuration option,
as shown in the following example:

.. code:: yaml

   monitors:
    - type: collectd/opcache
      host: localhost
      port: 80
      useHTTPS: true
      url: "http://{{.host}}:{{.port}}/opcache"
      # useHTTPS is ignored.

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
      - No
      - ``string``
      - Host name of the web server. For example, ``127.0.0.1``.
   - 

      - ``port``
      - No
      - ``integer``
      - Port number of the web server. For example, ``80``. The default
         value is ``0``.
   - 

      - ``useHTTPS``
      - No
      - ``bool``
      - If true, the monitor uses an HTTPS connection. The default value
         is ``false``.
   - 

      - ``path``
      - No
      - ``string``
      - Path of the scrape URL for the OPcache script. The default value
         is ``/opcache_stat.php``.
   - 

      - ``url``
      - No
      - ``string``
      - Either a final URL or a Go template populated with the ``host``,
         ``port``, and ``path`` values.
   - 

      - ``name``
      - No
      - ``string``
      - Name of the monitor. Sent as the ``plugin_instance`` dimension.

Metrics
-------

.. raw:: html
 
      <div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/splunk-otel-collector/main/internal/signalfx-agent/pkg/monitors/collectd/opcache/metadata.yaml"></div>


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



