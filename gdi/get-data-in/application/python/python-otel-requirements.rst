.. _python-otel-requirements:

*************************************************************
Python agent compatibility and requirements
*************************************************************

.. meta::
    :description: This is what you need to instrument any Python application using the Splunk OTel Python agent.

Meet these requirements to instrument Python applications for Splunk Observability Cloud.

.. _supported-python-libraries:

Supported libraries and frameworks
=================================================

The Splunk Distribution of OpenTelemetry Python instruments numerous libraries and packages. For a complete list, see :new-page:`Supported libraries <https://github.com/open-telemetry/opentelemetry-python-contrib/blob/main/instrumentation/README.md>` in the OpenTelemetry documentation.

.. _python-requirements:

Ensure you have supported Python and library versions
==============================================================



.. raw:: html

   <div class="include-start" id="requirements/python.rst"></div>

.. include:: /_includes/requirements/python.rst

.. raw:: html

   <div class="include-stop" id="requirements/python.rst"></div>



   
.. _python-otel-connector-requirement:

Install and configure the Splunk Distribution of OpenTelemetry Collector
======================================================================================================

The Splunk Distribution of OpenTelemetry Python exports application traces and spans to the Splunk Distribution of OpenTelemetry Collector, which also collects system metric data and logs.

To send application traces and spans to Splunk Observability Cloud, install the Splunk Distribution of OpenTelemetry Collector for your platform. The following distributions are available:

- Splunk OTel Collector for Linux. See :ref:`otel-install-linux`.
- Splunk OTel Collector for Windows. See :ref:`otel-install-windows`.
- Splunk OTel Collector for Kubernetes. See :ref:`otel-install-k8s`.

