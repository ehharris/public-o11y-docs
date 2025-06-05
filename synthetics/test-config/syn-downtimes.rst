.. _syn-downtimes:

************************************************************
Downtime
************************************************************


When you are working on your site, consider using a downtime configuration to account for maintenance and other planned irregularities in your monitoring. Here is some guidance on how to choose the downtime configuration rule you want to use for your for your situation. 

.. list-table::
  :header-rows: 1
  :widths: 20 80 

  * - :strong:`Downtime configuration`
    - :strong:`Description`
  * - Pause tests 
    - Suppose you are doing some maintenance on your site which impacts the login workflow. Choose to pause tests that are impacted by the maintenance so that you don't need to monitor the recurring failure messages around the known issue of being unable to login because of site maintenance. 
  * - Augment data 
    - If you are obligated to keep monitoring your site for SLAs even while you're doing maintenance, choose to augment the data with a dimension ``under_maintenance``.Then, you can filter out the augmented data so they don't impact your average metrics. All tests from a downtime window have the dimension ``under_maintenance=true``.


Schedule a downtime configuration 
====================================

It's a best practice to schedule maintenance windows with a 15 to 30 minute time buffer before and after you start and stop your maintenance work. This gives the system an opportunity to catch up with the maintenance state and reduces the chances of Splunk Synthetic Monitoring generating false positives during maintenance operations.

Schedule requirements: 

* at least fifteen minutes long
* up to one year in advance and one year in duration 

How to schedule a downtime configuration: 

1. In Splunk Synthetic Monitoring, go to settings, then :strong:`Downtime configurations`.
2. Select :strong:`Create downtime configuration`. 
3. Enter a name, choose a rule, and select the test you want to include. 
4. Set up the schedule and select :strong:`Create`. 

To mute any alerts associated with a test included in a downtime configuration window, see :ref:`mute-notifications`.

When a downtime configuration is active, you can't edit, or delete it. You can extend the duration, or cancel while it is active. 

Records 
======================

The downtime configuration record shows when the window started and finished. The records are kept for thirteen months. 

During a downtime configuration window, there are gaps in synthetics metrics if you chose to the rule to pause tests. Any metrics with active tests for the rule to augment data have the dimension ``under_maintenance: true``.


