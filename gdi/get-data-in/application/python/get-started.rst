.. _get-started-python:

**************************************************************
Instrument Python applications for Splunk Observability Cloud
**************************************************************


.. meta::
   :description: Instrument Python applications automatically to export spans and metrics to Splunk Observability Cloud.

.. toctree::
   :hidden:

   Requirements <python-otel-requirements>
   Instrument your Python application <instrumentation/instrument-python-application>
   Connect trace data with logs <instrumentation/connect-traces-logs>
   Configure the Python agent <configuration/advanced-python-otel-configuration>
   Metrics and attributes <configuration/python-otel-metrics>
   Manual instrumentation <instrumentation/python-manual-instrumentation>
   Additional steps for frameworks <instrumentation/instrument-python-frameworks>
   Troubleshoot the Python instrumentation <troubleshooting/common-python-troubleshooting>
   About Splunk OTel Python <splunk-python-otel-distribution>
   Migrate from SignalFx Python agent <troubleshooting/migrate-signalfx-python-agent-to-otel>
   Migrate to Splunk Python 2.X <migration-guide>
   Version 1.X (deprecated) <version1x/get-started-1x>

The Splunk Distribution of OpenTelemetry Python provides a Python agent that automatically adds APM instrumentation to your Python application. The instrumentation captures distributed traces and metrics and sends them to Splunk Observability Cloud. For more information, see :ref:`splunk-python-otel-dist`.

To instrument your Python application, follow these steps:

#. Check compatibility and requirements. See :ref:`python-otel-requirements`.
#. Instrument your Python application. See :ref:`instrument-python-applications`.
#. Configure your instrumentation. See :ref:`configure-python-instrumentation`.

.. note:: The SignalFx Python Agent is deprecated and has reached End of Support. See :ref:`migrate-signalfx-python-agent-to-otel` to migrate to the Splunk Distribution of OpenTelemetry Python.
