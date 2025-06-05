.. _onboarding-milestones:

************************************************************************
Onboarding milestones
************************************************************************

.. meta::
   :description: Planning your Splunk On-Call implementation.



Splunk On-Call is a powerful tool that allows teams to maintain a culture of high availability without slowing down the innovation process. Implementation of a new tool can be difficult; here are some key milestones to hit before going live with Splunk On-Call that will ensure your company is set up for success.

Planning milestones
=========================

#. Create a rough draft of your desired Splunk On-Call workflow in a spreadsheet. For example, who are the administrators, what tools are most important, who are action takers, what will your on-call schedules look like, and so on.
#. Set an Onboarding Timeline. For example:
 
.. image:: /_images/spoc/onboarding-milestones.png
      :width: 99%
      :alt: Create a week-by-week implementation timeline.


User milestones
============================

The user milestones include the following:

#. Invite users.
#. Determine user roles and permissions:
    - Determine your Global, Alert, and Team admins.
    - Share the admin training guides:
       - :ref:`global-admin`
       - :ref:`team-admin`
       - :ref:`alert-admin`
#. Set Primary Paging policies:
    - :ref:`primary-paging`
#. Implement user training with the :ref:`user-role`.

Team milestones
====================

Team milestones include the following:

#. Create Teams and assign Team Admins.
#. Create On-Call schedules, including:
   - :ref:`rotation-setup`
   - :ref:`schedule-examples`
#. Implementation of Team Workflows, including:
   - :ref:`Create escalation policies <team-escalation-policy>`.
   - :ref:`Tips and tricks for multiple escalation policies <multi-escalation-policies>`.


Integration milestones
===============================

Integration milestones include the following:

#. Determine necessary integrations.
#. Create Splunk On-Call routing keys. :ref:`Routing Key Best Practice <spoc-routing-keys>`
#. Configure integrations.  :ref:`Splunk On-Call Integration Guides <integrations-main-spoc>`
#. Test integrations and trigger test incidents. :ref:`Maintenance Mode <maintenance-mode>`


Go Live milestones
===============================

Go Live milestones include the following:

#. Ensure all Splunk On-Call users have completed their User Trainings.
#. Internal & External Resources made available, including:
   - Create internal documentation for Splunk On-Call 
   - Familiarize all Splunk On-Call users with the Splunk On-Call resources
#. Go Live!