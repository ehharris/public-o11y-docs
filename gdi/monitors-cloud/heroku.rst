.. _heroku:

Heroku
======

.. meta::
   :description: Use this Splunk Observability Cloud integration for the Heroku monitor. See benefits, install, configuration, and metrics.

The Splunk OpenTelemetry Connector for Heroku is a buildpack for the Splunk Distribution of the OpenTelemetry Collector. The buildpack installs
and runs the Splunk OpenTelemetry Connector on a Dyno to receive, process and export metric and trace data for Splunk Observability Cloud:

-  Splunk APM through the ``otlphttp`` exporter.
-  Splunk Infrastructure Monitoring through the ``signalfx`` exporter.

See :ref:`otel-intro` to learn more.

Prerequisites
----------------

Before you can install the Heroku buildpack to collect metrics, you need
to have a Heroku app. To learn how to install the Heroku CLI and create
an app, see the Heroku documentation for developers.

Installation steps
------------------------

Follow these steps to collect metrics with the Heroku buildpack for the
Splunk Distribution of the OpenTelemetry Collector:

1. In the command-line interface, navigate to the Heroku project
   directory.

   .. code:: bash

      cd <HEROKU_APP_DIRECTORY>

   **Note:** Running ``heroku`` command outside of project directories
   results in unexpected behavior.

2. Configure the Heroku app to expose Dyno metadata, which is required
   by Splunk OpenTelemetry Connector to set global dimensions such as
   ``app_name``, ``app_id`` and ``dyno_id``. See here for more
   information.

   .. code:: bash

      heroku labs:enable runtime-dyno-metadata

3. Run both of the following commands together to add the Heroku
   buildpack.

   .. code:: bash

      heroku buildpacks:add https://github.com/signalfx/splunk-otel-collector-heroku.git#\
      $(curl -s https://api.github.com/repos/signalfx/splunk-otel-collector-heroku/releases | grep '"tag_name"' | head -n 1 | cut -d'"' -f4)

   **Note:** If you want to use an explict version number for production
   environment, replace the first command with the following command:

   .. code:: bash

      heroku buildpacks:add https://github.com/signalfx/splunk-otel-collector-heroku.git#<TAG_NAME>

4. Set the required environment variables.

   .. code:: bash

      heroku config:set SPLUNK_ACCESS_TOKEN=<YOUR_ACCESS_TOKEN>
      heroku config:set SPLUNK_REALM=<YOUR_REALM>

5. (Optional) Define custom configuration file in your Heroku project
   directory.

   .. code:: bash

      heroku config:set SPLUNK_CONFIG=/app/mydir/myconfig.yaml

6. To add the buildpack to an existing project, you must create an empty
   commit before deploying the app.

   .. code:: bash

      git commit --allow-empty -m "empty commit"

7. Run the following command to deploy the app.

   .. code:: bash

      git push heroku main

8. Run the following command to check the logs.

   .. code:: bash

      heroku logs -a <app-name> --tail

Configuration
-------------

Use the following environment variables to configure the Heroku
buildpack.

.. list-table::
   :widths: 14 5 5 48
   :header-rows: 1

   - 

      - Environment Variable
      - Required
      - Default
      - Description
   - 

      - ``SPLUNK_ACCESS_TOKEN``
      - Yes
      - 
      - Splunk access token.
   - 

      - ``SPLUNK_REALM``
      - Yes
      - 
      - Splunk realm
   - 

      - ``SPLUNK_API_URL``
      - No
      - ``https://api.SPLUNK_REALM.signalfx.com``
      - The Splunk API base URL.
   - 

      - ``SPLUNK_CONFIG``
      - No
      - ``/app/config.yaml``
      - The configuration to use. ``/app/.splunk/config.yaml`` used if
         default not found.
   - 

      - ``SPLUNK_INGEST_URL``
      - No
      - ``https://ingest.SPLUNK_REALM.signalfx.com``
      - The Splunk Infrastructure Monitoring base URL.
   - 

      - ``SPLUNK_LOG_FILE``
      - No
      - ``/dev/stdout``
      - Specify location of agent logs. If not specified, logs will go
         to stdout.
   - 

      - ``SPLUNK_MEMORY_TOTAL_MIB``
      - No
      - ``512``
      - Total available memory to agent.
   - 

      - ``SPLUNK_OTEL_VERSION``
      - No
      - ``latest``
      - Version of Splunk OTel Connector to use. Defaults to latest.
   - 

      - ``SPLUNK_TRACE_URL``
      - No
      - ``https://ingest.SPLUNK_REALM.signalfx.com/v2/trace``
      - The Splunk APM base URL.

Troubleshooting
---------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



