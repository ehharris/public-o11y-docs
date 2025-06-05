.. _logs-intro-logconnect:

*****************************************************************
Introduction to Splunk Log Observer Connect
*****************************************************************



.. meta::
   :description: Log Observer integration with Splunk Cloud Platform or Splunk Enterprise. The introduction is an overview describing all Log Observer Connect functionality.


Splunk Log Observer Connect is an integration that allows you to query your Splunk Enterprise or Splunk Cloud Platform logs using the capabilities of Splunk Log Observer and :ref:`Related Content <get-started-relatedcontent>` in Splunk Observability Cloud. With Log Observer Connect, you can troubleshoot your application and infrastructure behavior using high-context logs. Perform codeless queries on your Splunk Enterprise or Splunk Cloud Platform logs to detect the source of problems in your systems, then jump to Related Content throughout Splunk Observability Cloud in one click. Seeing your logs data correlated with metrics and traces in Splunk Observability Cloud helps your team to locate and resolve problems exponentially faster.

Region and version availability
==============================================================



.. raw:: html

   <div class="include-start" id="logs/loc-availability.rst"></div>

.. include:: /_includes/logs/loc-availability.rst

.. raw:: html

   <div class="include-stop" id="logs/loc-availability.rst"></div>




.. _LogObserverFeatures:

What can I do with Log Observer Connect?
==============================================================
The following table lists features available to customers who have integrated Splunk Enterprise or Splunk Cloud Platform with Splunk Observability Cloud, allowing them to use Log Observer Connect.

.. list-table::
   :header-rows: 1
   :widths: 40, 30, 30

   * - :strong:`Do this`
     - :strong:`With this tool`
     - :strong:`Link to documentation`

   * - View your incoming logs and zoom in or out to the time period of your choice.
     - Timeline
     - :ref:`logs-timeline`

   * - Scan logs.
     - Logs table
     - :ref:`logs-raw-logs-display`

   * - Find out which path in your API has the slowest response time.
     - Log aggregations
     - :ref:`logs-aggregations`

   * - Search logs by keyword or field.
     - Content control bar
     - :ref:`logs-keyword`

   * - Filter your logs to see only logs that contain a field of your choice with the value :guilabel:`error`.
     - Logs table
     - :ref:`logs-keyword`

   * - View the JSON schema of an individual log.
     - Log details
     - :ref:`logs-individual-log-connect`

   * - See the metrics, traces, and infrastructure related to a specific log.
     - Related Content
     - :ref:`get-started-scenario`

   * - Save and share Log Observer queries.
     - Saved Queries
     - :ref:`logs-save-share`


Get started with Log Observer Connect
==============================================================
If you manage Splunk Enterprise in a data center or public cloud and want to begin using Log Observer Connect, see :ref:`logs-set-up-logconnect`. If you use Splunk Cloud Platform and want to integrate Log Observer Connect, see :ref:`logs-scp`.

.. note:: You can collect data using both the Splunk Distribution of OpenTelemetry Collector and the Universal Forwarder without submitting any duplicate telemetry data. See :ref:`collector-with-the-uf` to learn how.