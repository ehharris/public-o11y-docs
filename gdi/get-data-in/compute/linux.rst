.. _get-started-linux:

***********************
Collect Linux data
***********************

.. meta::
   :description: Send metrics and logs from Linux hosts to Splunk Observability Cloud.

The Splunk Distribution of the OpenTelemetry Collector is a package that provides integrated collection and forwarding for all telemetry types of the Linux platform to Splunk Observability Cloud.

Supported versions
=====================



.. raw:: html

   <div class="include-start" id="requirements/collector-linux.rst"></div>

.. include:: /_includes/requirements/collector-linux.rst

.. raw:: html

   <div class="include-stop" id="requirements/collector-linux.rst"></div>




Install the Collector for Linux
======================================

Splunk Observability Cloud offers a guided setup to install the Collector:

#. Log in to Splunk Observability Cloud.

#. Open the :new-page:`Linux guided setup <https://login.signalfx.com/#/gdi/scripted/otel-connector-linux/step-1?category=all&gdiState=%7B"integrationId":"otel-connector-linux"%7D>`. Optionally, you can navigate to the guided setup on your own:

   #. In the navigation menu, select :menuselection:`Data Management`. 
   
   #. Go to the :guilabel:`Available integrations` tab, or select :guilabel:`Add Integration` in the :guilabel:`Deployed integrations` tab.

   #. In the integration filter menu, select :guilabel:`All`.

   #. Select :guilabel:`Linux`.

   #. Select :guilabel:`Add Connection`. The integration guided setup appears.

#. Follow the steps in the guided setup.

For advanced installation instructions, see :ref:`otel-install-linux`.

Learn more
=================

- :ref:`collector-linux-intro`.
- Troubleshoot Collector issues. See :ref:`otel-troubleshooting`.
- For a list of host and application monitors see :ref:`monitor-data-sources`.