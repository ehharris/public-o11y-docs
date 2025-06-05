.. _instrument-php-otel-applications:

*******************************************************************************
Instrument your PHP application for Splunk Observability Cloud
*******************************************************************************

.. meta::
   :description: The OpenTelemetry PHP extensions automatically instruments PHP applications using a PHP extension and available instrumentation libraries. Follow these steps to get started.



.. raw:: html

   <div class="include-start" id="zero-code-info.rst"></div>

.. include:: /_includes/zero-code-info.rst

.. raw:: html

   <div class="include-stop" id="zero-code-info.rst"></div>



   
The OpenTelemetry PHP extension automatically instruments PHP applications using a PHP extension and available instrumentation libraries. You can send telemetry to the Splunk Distribution of OpenTelemetry Collector or directly to the Splunk Observability Cloud ingest endpoint.

To get started, use the guided setup or follow the instructions to install manually.

Generate customized instructions using the guided setup
====================================================================

To generate all the basic installation commands for your environment and application, use the PHP OpenTelemetry guided setup. To access the PHP OpenTelemetry guided setup, follow these steps:

#. Log in to Splunk Observability Cloud.
#. Open the :new-page:`PHP OpenTelemetry guided setup <https://login.signalfx.com/#/gdi/scripted/php-dotnet-tracing/>`. Optionally, you can navigate to the guided setup on your own:

   #. In the navigation menu, select :menuselection:`Data Management`.
   #. Go to the :guilabel:`Available integrations` tab, or select :guilabel:`Add Integration` in the :guilabel:`Deployed integrations` tab.
   #. In the integration filter menu, select :guilabel:`By Product`.
   #. Select the :guilabel:`APM` product.
   #. Select the :guilabel:`PHP (OpenTelemetry)` tile to open the PHP OpenTelemetry guided setup.

.. _install-php-otel-instrumentation:

Install the Splunk Distribution of OpenTelemetry PHP manually
==================================================================

If you don't use the guided setup, follow these steps to manually install and automatically instrument your PHP application:

1. Check that you meet the requirements. See :ref:`php-otel-requirements`.

2. Install the OpenTelemetry PHP extension using PECL in the command line:

   .. tabs::

      .. code-tab:: shell Linux

         sudo apt-get install gcc make autoconf
         pecl install opentelemetry

      .. tab:: Windows

         Download the precompiled DLL file from the :new-page:`releases page <https://github.com/open-telemetry/opentelemetry-php-instrumentation/releases/latest>` on GitHub.

         Make sure to place the DLL in your extensions's directory, as defined by the value of ``extension_dir`` in your php.ini file.

3. Add the extension to your php.ini file:

   .. tabs::

      .. code-tab:: ini Linux

         [opentelemetry]
         extension=opentelemetry.so

      .. code-tab:: ini Windows

         [opentelemetry]
         extension=php_opentelemetry.dll

4. Install the required instrumentations you need using Composer:

   .. code-block:: bash

      php composer.phar install open-telemetry/exporter-otlp:^1.0.3
      php composer.phar install php-http/guzzle7-adapter:^1.0

   You can also install additional instrumentations. See :ref:`supported-php-otel-libraries`.

5. Configure the basic settings in your php.ini file or using environment variables:

   .. code-block:: bash

      OTEL_PHP_AUTOLOAD_ENABLED=true \
      OTEL_SERVICE_NAME="<your-service-name>" \
      OTEL_RESOURCE_ATTRIBUTES="deployment.environment=<your_env>" \

6. Run your application.

   See the :new-page:`OpenTelemetry PHP examples <https://github.com/signalfx/tracing-examples/tree/main/opentelemetry-tracing/opentelemetry-php>` in GitHub for sample instrumentation scenarios.

To configure the instrumentation, see :ref:`php-settings-otel`.

.. _activate_rum_apm_php:

Connect RUM to APM through server trace data
===================================================================

To connect Real User Monitoring (RUM) requests from mobile and web applications with server trace data, add the OpenTelemetry server timing propagator as a dependency:

.. code-block:: shell

   php composer.phar install open-telemetry/opentelemetry-propagation-server-timing:^0.0.2


.. _export-directly-to-olly-cloud-php-otel:

Send data directly to Splunk Observability Cloud
====================================================================

By default, all telemetry is sent to the local instance of the Splunk Distribution of OpenTelemetry Collector.

To bypass the Collector and send data directly to Splunk Observability Cloud, set the following environment variables:

.. code-block:: shell

   OTEL_EXPORTER_OTLP_TRACES_HEADERS=x-sf-token=<access_token>
   OTEL_EXPORTER_OTLP_ENDPOINT=https://ingest.<realm>.signalfx.com/trace/otlp

To obtain an access token, see :ref:`admin-api-access-tokens`.

To find your Splunk realm, see :ref:`Note about realms <about-realms>`.

.. _php-settings-otel:

Instrumentation settings
=========================================================================

You can configure the OpenTelemetry PHP extension using the following settings.

General settings
------------------------

.. raw:: html

    <div class="instrumentation" section="settings" group="category" filter="general" url="https://raw.githubusercontent.com/splunk/o11y-gdi-metadata/main/apm/opentelemetry-php-metadata.yaml" data-renaming='{"keys": "Identifier", "description": "Description", "instrumented_components": "Components", "signals": "Signals", "env": "Environment variable", "default": "Default", "type": "Type"}'></div>

Exporter settings
------------------------

.. raw:: html

    <div class="instrumentation" section="settings" group="category" filter="exporter" url="https://raw.githubusercontent.com/splunk/o11y-gdi-metadata/main/apm/opentelemetry-php-metadata.yaml" data-renaming='{"keys": "Identifier", "description": "Description", "instrumented_components": "Components", "signals": "Signals", "env": "Environment variable", "default": "Default", "type": "Type"}'></div>

Instrumentation settings
------------------------

.. raw:: html

    <div class="instrumentation" section="settings" group="category" filter="instrumentation" url="https://raw.githubusercontent.com/splunk/o11y-gdi-metadata/main/apm/opentelemetry-php-metadata.yaml" data-renaming='{"keys": "Identifier", "description": "Description", "instrumented_components": "Components", "signals": "Signals", "env": "Environment variable", "default": "Default", "type": "Type"}'></div>
