.. caution::

   The SignalFx Tracing Library for PHP is deprecated as of February 21, 2024 and will reach End of Support (EOS) on February 21 2025. Until then, only critical security fixes and bug fixes will be provided. After the EOS date, the library will be archived and no longer maintained.

   If you want to instrument new or existing PHP applications, use :ref:`OpenTelemetry PHP instrumentation <get-started-php>`, which offers similar functionalities.

.. _instrument-php-applications:

***************************************************************************
Instrument your PHP application for Splunk Observability Cloud (deprecated)
***************************************************************************

.. meta::
   :description: The SignalFx Tracing Library for PHP automatically instruments PHP applications. Follow these steps to get started.

The SignalFx Tracing Library for PHP automatically instruments PHP applications.

To get started, use the guided setup or follow the instructions manually.

Generate customized instructions using the guided setup
====================================================================

To generate all the basic installation commands for your environment and application, use the PHP guided setup. To access the PHP guided setup, follow these steps:

#. Log in to Splunk Observability Cloud.
#. Open the :new-page:`PHP guided setup <https://login.signalfx.com/#/gdi/scripted/php-tracing/step-1?category=product-apm&gdiState=%7B"integrationId":"php-tracing"%7D>`. Optionally, you can navigate to the guided setup on your own:

   #. In the navigation menu, select :menuselection:`Data Management`.

   #. Go to the :guilabel:`Available integrations` tab, or select :guilabel:`Add Integration` in the :guilabel:`Deployed integrations` tab.

   #. In the integration filter menu, select :guilabel:`By Product`.

   #. Select the :guilabel:`APM` product.

   #. Select the :guilabel:`PHP` tile to open the PHP guided setup.

Install the SignalFx Tracing Library for PHP manually
==================================================================

If you don't use the guided setup, follow these instructions to manually install the SignalFx Tracing Library for PHP:

#. :ref:`install-php-instrumentation`
#. :ref:`deploy-php`
#. :ref:`advanced-php-configuration`

.. _install-php-instrumentation:

Instrument your PHP application
------------------------------------------

Follow these steps to automatically instrument your application:

#. Check that you meet the requirements. See :ref:`php-requirements`.

#. Download the installation script from the following location:

   .. code-block:: shell

      curl -LO  https://github.com/signalfx/signalfx-php-tracing/releases/latest/download/signalfx-setup.php

#. Install by running the installation script:

   .. code-block:: shell

      php signalfx-setup.php --php-bin=all

   .. note:: If you omit the ``--php-bin`` option, you can interactively select the PHP installation.

#. Set the following environment variables:

   .. tabs::

      .. tab:: Apache configuration

         .. code-block:: aconf

            # Add the following lines to your Apache configuration file

            SetEnv SIGNALFX_SERVICE_NAME="<my-service-name>"
            SetEnv SIGNALFX_ENDPOINT_URL='http://localhost:9411/api/v2/spans'
            SetEnv SIGNALFX_TRACE_GLOBAL_TAGS="deployment.environment:<my_environment>"

      .. tab:: Terminal

         .. code-block:: shell

            export SIGNALFX_SERVICE_NAME="<my-service-name>"
            export SIGNALFX_ENDPOINT_URL='http://localhost:9080/v1/trace'
            export SIGNALFX_TRACE_GLOBAL_TAGS="deployment.environment:<my_environment>"

         Set environment variables globally or using the start script of your PHP application.

#. Restart your server.

Next, deploy the PHP instrumentation in your environment. See :ref:`deploy-php` for more information.

.. note:: If you need to add custom attributes to spans or want to manually generate spans, instrument your PHP application or service manually. See :ref:`php-manual-instrumentation`.

.. _php-ini-config:

INI file settings
---------------------

If you don't set any environment variable, the library extracts default values from the INI file. The prefix for settings defined using environment variables that start with ``SIGNALFX_TRACE_`` is ``signalfx.trace.``. For all other environment variables that start with ``SIGNALFX_`` the prefix is ``signalfx.``.

You can use the ``signalfx-setup.php`` script to set INI file options without having to manually locate each file. For example:

.. code-block:: shell

   php signalfx-setup.php --update-config --signalfx.endpoint_url=http://172.17.0.1:9080/v1/trace

This is useful for options common to all PHP services running in the system, like endpoints.

.. _deploy-php:

Deploy the PHP instrumentation in your environment
=====================================================

You can deploy the PHP instrumentation in Docker or, Kubernetes, or you can send data directly to Splunk Observability Cloud. See the following sections for instructions for your environment:

* :ref:`docker_php`
* :ref:`kubernetes_php`
* :ref:`export-directly-to-olly-cloud-php`

.. _docker_php:

Deploy the PHP instrumentation in Docker
-----------------------------------------------

You can deploy the PHP instrumentation using Docker. Follow these steps to get started:

#. Create a startup shell script in a location Docker can access. The script can have any name, for example setup.sh.

#. Edit the startup shell script to export the environment variables described in :ref:`install-php-instrumentation`.

#. Add the following commands to the startup shell script to initialize the PHP instrumentation:

   .. code-block:: shell

      curl -LO https://github.com/signalfx/signalfx-php-tracing/releases/latest/download/signalfx-setup.php
      php signalfx-setup.php --php-bin=all
      php signalfx-setup.php --update-config --signalfx.endpoint_url=https://ingest.<realm>.signalfx.com/v2/trace/signalfxv1
      php signalfx-setup.php --update-config --signalfx.access_token=<access_token>
      php signalfx-setup.php --update-config --signalfx.service_name=<service-name>

#. Add a line to the script to start the application using the ``supervisorctl``, ``supervisord``, ``systemd``, or a similar command. The following example uses ``supervisorctl``:

   .. code-block:: shell

      supervisor start my-php-app

#. Add a command to run the newly created shell script at the end of the Dockerfile.

#. Rebuild the container using the ``docker build`` command.

Next, configure the PHP instrumentation for Splunk Observability Cloud. See :ref:`advanced-php-configuration`.

.. caution:: Make sure to deactivate the ``Xdebug`` extension, as it's not compatible with the PHP instrumentation.

.. _kubernetes_php:

Deploy the PHP instrumentation in Kubernetes
-----------------------------------------------

To deploy the PHP instrumentation in Kubernetes, configure the Kubernetes Downward API to expose environment variables to Kubernetes resources.

The following example shows how to update a deployment to expose environment variables by adding the agent configuration under the ``.spec.template.spec.containers.env`` section:

.. code-block:: yaml

   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: my-deployment
   spec:
     selector:
       matchLabels:
         app: your-application
     template:
       metadata:
         labels:
           app: your-application
       spec:
         containers:
         - name: myapp
           image: <image-name>
           env:
             - name: HOST_IP
               valueFrom:
                 fieldRef:
                   fieldPath: status.hostIP
             - name: SIGNALFX_SERVICE_NAME
               value: "<service-name>"
             - name: SIGNALFX_ENDPOINT_URL
               value: "http://$(HOST_IP):9411/api/v2/spans"
             - name: SIGNALFX_TRACE_GLOBAL_TAGS
               value: "deployment.environment:<my_environment>"

Next, configure the PHP instrumentation for Splunk Observability Cloud. See :ref:`advanced-php-configuration`.

.. _export-directly-to-olly-cloud-php:

Send data directly to Splunk Observability Cloud
-----------------------------------------------------

By default, all telemetry is sent to the local instance of the Splunk Distribution of OpenTelemetry Collector.

To bypass the OTel Collector and send data directly to Splunk Observability Cloud, set the following environment variables:

.. tabs::

   .. code-tab:: aconf Apache configuration

      SetEnv SIGNALFX_ACCESS_TOKEN=<access_token>
      SetEnv SIGNALFX_ENDPOINT_URL=https://ingest.<realm>.signalfx.com/v2/trace/signalfxv1

   .. code-tab:: shell Terminal

      export SIGNALFX_ACCESS_TOKEN=<access_token>
      export SIGNALFX_ENDPOINT_URL=https://ingest.<realm>.signalfx.com/v2/trace/signalfxv1



.. raw:: html

   <div class="include-start" id="gdi/apm-api-define-host.rst"></div>

.. include:: /_includes/gdi/apm-api-define-host.rst

.. raw:: html

   <div class="include-stop" id="gdi/apm-api-define-host.rst"></div>




To obtain an access token, see :ref:`admin-api-access-tokens`.

To find your Splunk realm, see :ref:`Note about realms <about-realms>`.

Next, configure the PHP instrumentation for Splunk Observability Cloud. See :ref:`advanced-php-configuration` for more information.

.. note:: For more information on the ingest API endpoints, see :new-page:`Send APM traces <https://dev.splunk.com/observability/docs/apm/send_traces/>`.
