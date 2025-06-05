.. _view-slo:


******************************************************************************************
View and manage service level objectives (SLOs)
******************************************************************************************

.. meta::
   :description: View a summary of all SLOs and manage SLOs in your organization.

You can view a list of existing SLOs and manage your SLOs after creating them.

View a list of SLOs
================================

To view existing SLOs, from the landing page of Splunk Observability Cloud, go to :strong:`Detectors & SLOs` and select the :strong:`SLOs` tab. The table view gives you a summary of SLO status, budget remaining, and burn rate alerting.

* By default, SLOs are sorted from most to least recently updated. To sort SLOs by name, select the :strong:`Name` column header.
* By default, the table shows all existing SLOs. To filter SLOs by status, select the :guilabel:`Status: All` dropdown menu and select the status you want to see: :guilabel:`Breached` or :guilabel:`Normal`.

Edit an SLO
================================

#. On the :strong:`SLOs` tab, select the more icon (|more|) next to the SLO you want to edit.
#. In the actions menu, select :menuselection:`Edit`.
#. Make changes to the SLO on the editing screen.
#. Select :guilabel:`Save` to save your changes.

Delete an SLO
================================

#. On the :strong:`SLOs` tab, select the more icon (|more|) next to the SLO you want to edit.
#. In the actions menu, select :menuselection:`Delete`.
#. Select :guilabel:`Delete` on the dialog box to confirm.

.. note:: You can't recover a deleted SLO.

Add an SLI chart to a dashboard
================================

You can add SLI visualization to a dashboard as a column chart or a single value chart. Splunk Observability Cloud chooses a chart type for your SLO depending on the size of the chart. SLI charts are read-only.

To add an SLI chart to a dashboard, follow these steps:

#. On the :strong:`Service Level Objectives (SLOs)` page, select the more icon (|more|) next to the SLO you want to add to a dashboard.
#. In the actions menu, select :menuselection:`Add to dashboard...`.
#. Search for an existing dashboard or create a new dashboard.
#. Select :strong:`OK` to add the SLI chart to the dashboard.

After adding an SLI chart to a dashboard, you can also save the chart as a CSV file or an image. For more information, see :ref:`export-and-share-charts`.

.. note:: You must have write permission for a dashboard to add an SLI chart to it.

Troubleshooting with an SLI chart
--------------------------------------

With an SLI chart, not only can you get an overview of your SLO at a glance, but you can also gain more specific insights into your SLI performance using different filter and customized time ranges. For more information, see :ref:`time-range-selector`.
