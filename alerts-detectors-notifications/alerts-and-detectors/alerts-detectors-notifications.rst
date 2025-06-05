.. _get-started-detectoralert:

**************************************************************************
Introduction to alerts and detectors in Splunk Observability Cloud
**************************************************************************

.. toctree::
    :maxdepth: 3
    :hidden:

    Best practices for creating detectors <detectors-best-practices>
    Alerts and detectors scenario library TOGGLE <scenarios-detectors-alerts/scenarios-intro.rst>
    Use and customize AutoDetect alerts and detectors TOGGLE <autodetect/autodetect>
    create-detectors-for-alerts
    detector-manage-permissions
    link-detectors-to-charts
    manage-notifications
    preview-detector-alerts
    View alerts <view-alerts>
    View detectors <view-detectors>
    detector-options
    mute-notifications
    auto-clearing-alerts
    Troubleshoot detectors <troubleshoot-detectors>
    Built-in alert conditions TOGGLE <alert-condition-reference/index>
    alert-message-variables-reference

.. meta::
   :description: Splunk Observability Cloud uses detectors, events, alerts, and notifications to keep you informed when certain criteria are met. When a detector condition is met, the detector generates an event, triggers an alert, and can send one or more notifications.

Splunk Observability Cloud uses detectors, events, alerts, and notifications to keep you informed when certain criteria are met. Active alerts and existing detectors can be found in tabs on the :strong:`Alerts` page, and events can be found in the :strong:`Events` sidebar, available from within any dashboard.

.. raw:: html
  
    <embed>
      <h2>Example scenarios of alerts and detectors<a name="alert-detect-scenarios" class="headerlink" href="#alert-detect-scenarios" title="Permalink to this headline">¶</a></h2>
    </embed>

- You want a message sent to a Slack channel or to an email address for the Ops team when CPU Utilization has reached the 95th percentile.
- You want to be notified when the number of concurrent users is approaching a limit that might require you to spin up an additional AWS instance.

For more example scenarios, see :ref:`scenarios-alerts-detectors`.

.. raw:: html
  
    <embed>
      <h2>Detectors<a name="detectors-definition" class="headerlink" href="#detectors-definition" title="Permalink to this headline">¶</a></h2>
    </embed>

A :term:`detector` monitors signals on a plot line, as on a chart, and triggers alert events and clear events based on conditions you define in rules. Conceptually, you can think of a detector as a chart that can trigger alerts when a signal value crosses specified thresholds defined in alert rules.

Rules trigger an alert when the conditions in those rules are met. Individual rules in a detector are labeled according to severity: Info, Warning, Minor, Major, and Critical. For example, a detector that monitors the latency of an API call might go into a critical state when the latency is significantly higher than normal, as defined in the detector rules.

Detectors also evaluate streams against a specific condition over a period of time. When you apply analytics to a metric time series (MTS), it produces a stream, an object of SignalFlow query language. The MTS can contain raw data or the output of an analytics function.

.. raw:: html
  
    <embed>
      <h3>Metadata in detectors<a name="detector-metadata" class="headerlink" href="#detector-metadata" title="Permalink to this headline">¶</a></h3>
    </embed>

The metadata associated with MTS can be used to make detector definition simpler, more compact, and more resilient.

For example, if you have a group of 30 virtual machines that are used to provide a clustered service like Kafka, you normally include the dimension :code:`service:kafka` with all of the metrics coming from those virtual machines.

If you want to track whether the CPU utilization remains below 80 for each of those virtual machines, you can create a single detector that queries for the CPU utilization metrics that include the :code:`service:kafka` dimension and evaluates those metrics against the threshold of 80. This single detector triggers individual alerts for each virtual machine whose CPU utilization exceeds the threshold, as if you had 30 separate detectors. You do not need to create 30 individual detectors to monitor each of your 30 virtual machines.

If the population changes because the cluster has grown to 40 virtual machines, you can make a cluster- or service-level detector. If you include the :code:`service:kafka` dimension for the newly-added virtual machines, the existing detector's query includes all new virtual machines in the cluster in the threshold evaluation.

.. raw:: html
  
    <embed>
      <h3>Dynamic threshold conditions<a name="threshold-conditions" class="headerlink" href="#threshold-conditions" title="Permalink to this headline">¶</a></h3>
    </embed>

Setting static values for detector conditions can lead to noisy alerting because the appropriate value for one service or for a particular time of day might not be suitable for another service or a different time of day. For example, if your applications or services contain an elastic infrastructure, like Docker containers or EC2 autoscaling, the values for your alerts might vary by time of day.

You can define dynamic thresholds to account for changes in streaming data. For example, if your metric exhibits cyclical behavior, you can define a threshold that is a one-week timeshifted version of the same metric. Suppose the relevant basis of comparison for your data is the behavior of a population, such as a clustered service. In that case, you can define your threshold as a value that reflects that behavior. For example, the 90th percentile for the metric across the entire cluster over a moving 15-minute window.

To learn more, see :ref:`condition-reference`.

.. raw:: html
  
    <embed>
      <h2>Alerts<a name="alerts" class="headerlink" href="#alerts" title="Permalink to this headline">¶</a></h2>
    </embed>

When data in an input MTS matches a condition, the detector generates a trigger event and an alert that has a specific severity level. You can configure an alert to send a notification using Splunk On-Call. For more information, see the :ref:`about-spoc` documentation.

Alert rules use settings you specify for built-in alert conditions to define thresholds that trigger alerts. When a detector determines that the conditions for a rule are met, it triggers an alert, creates an event, and sends notifications (if specified). Detectors can send notifications via email, as well as via other systems, such as Slack, or via a webhook.

.. raw:: html
  
    <embed>
      <h2>Interaction between detectors, events, alerts, and notifications<a name="detector-dashboard" class="headerlink" href="#detector-dashboard" title="Permalink to this headline">¶</a></h2>
    </embed>

The interaction between detectors, events, alerts, and notifications is as follows:

-  :term:`Detectors<detector>` contain :term:`rules<rule>` that specify:

   -  When the detector is triggered, based on conditions related to the detector's :term:`signal`.
   -  The severity of the :term:`alert` to be generated by the detector.
   -  Where :term:`notifications<notification>` should be sent.

-  When a detector is triggered, it does the following:

   -  Generates an :term:`event`, which can be viewed on charts and in the Events sidebar.
   -  Triggers an alert, which can be viewed in a number of places throughout Splunk Observability Cloud.
   -  Sends one or more notifications, so people are informed about the alert even if they are not currently monitoring dashboards.

-  When the condition clears, the detector generates a second event and sends a second set of notifications.

The following diagram illustrates the relationship between detectors and alerts. 
The boxes represent objects relating to the detector, and the diamonds represent processes relating to the detector.

.. mermaid:: 
  
  flowchart LR

    accTitle: Alert and detector diagram
    accDescr: The detector encompasses a signal, an alert rule, and an alert condition. Based on the signal and alert rule, the detector checks whether its alert condition is met. If the alert condition is met, the detector is triggered, and the detector sends an alert, an event, and (optionally) a notification. If the alert condition isn't met, then the detector isn't triggered.

      subgraph Detector
      Signal --> A{Alert condition met?}
      B[Alert rule] --> A
      end
      A -- yes --> D{Detector triggered}
      A -- no --> E{Detector not triggered}
      D --> Alert
      D --> Event
      D -.-> F["Notifications (optional)"]
    

.. raw:: html
  
    <embed>
      <h2>What you can do with alerts and detectors<a name="detector-do" class="headerlink" href="#detector-do" title="Permalink to this headline">¶</a></h2>
    </embed>

The following table shows you what you can do with detectors, events, alerts, and notifications:

.. list-table::
   :header-rows: 1
   :widths: 50 50

   * - :strong:`Do this`
     - :strong:`Link to documentation`

   * - View alerts based on configured detectors for your organization.
     - :ref:`View alerts<view-alerts>`

   * - Limit who can make changes to your detectors.
     - :ref:`detector-manage-permissions`

   * - Specify where to send alert notifications.
     - :ref:`manage-notifications`

   * - Temporarily mute (stop sending) notifications.
     - :ref:`mute-notifications`

   * - Create and view events to supplement alert information.
     - :ref:`events-intro`

   * - Create detectors to generate events, alerts, and notifications that meet your monitoring requirements.
     - :ref:`create-detectors`

   * - Work with built-in alert conditions.
     - :ref:`condition-reference`

   * - See default setting that automatically clears alerts generated by a metric that stops reporting.
     - :ref:`auto-clearing-alerts`

   * - Determine why a detector doesn't trigger an alert, or triggers an alert unexpectedly.
     - :ref:`troubleshoot-detectors`

   * - Link a detector to a chart.
     - :ref:`linking-detectors`


