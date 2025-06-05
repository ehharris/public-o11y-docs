.. _Pingdom-spoc:

Pingdom integration for Splunk On-Call
***************************************************

.. meta::
    :description: Configure the Pingdom integration for Splunk On-Call.

Pingdom is a service that tracks the uptime, downtime, and performance of websites.

Requirements
==================

This integration is compatible with the following versions of Splunk On-Call:

- Starter
- Growth
- Enterprise

Splunk On-Call configuration
=============================

In Splunk On-Call, select :guilabel:`Integrations` *>>* :guilabel:`Pingdom (Webhook)`

If the integration has not yet been enabled, click the *Enable
Integration* button to generate your endpoint URL as seen below. Be
sure to replace the "$routing_key" section with the actual routing key
you intend to use. It is essential that you replace what you see here
with the actual routing key you intend to use. Everything after the
final forward slash must be replaced with your key. For example,
assuming a routing_key value of "database":

………36437/**$routing_key** ==>  ……..36437/**database**

.. image:: /_images/spoc/Screen_Shot_2019-10-09_at_11_47_13_AM.png
   :alt: Pingdom endpoint URL example

Routing keys in Splunk On-Call can be set up and associated by clicking
on *Settings >> Route Keys.*

For more information on routing keys and best practices, see :ref:`spoc-routing-keys`.

Pingdom configuration
======================

Select :guilabel:`Integrations` from the menu bar, click the "Integrations" option, then click :guilabel:`Add integratiopn`.

In the :guilabel:`Add Integration` window, use the drop-down menu for :guilabel:`Type` to
select :guilabel:`Webhook`. Give the webhook a name, and paste in the webhook URL
provided by Splunk On-Call. Be sure to replace the "$routing_key" section
with your actual routing key.

Click :guilabel:`Save Integration`.

.. image:: /_images/spoc/Screen-Shot-2019-10-09-at-11.48.22-AM.png
   :alt: Add Integration window


When creating or editing checks, scroll to the bottom of the settings to
select the new integration you have just added. You don't need to
include any alerting actions for the webhook to function.

.. image:: /_images/spoc/Screen-Shot-2019-10-09-at-11.52.47-AM.png
   :alt: Edit Check window


Email endpoint integration
============================

Email endpoint integration is accomplished though tasks in Splunk On-Call and Pingdom.


Splunk On-Call (Email) configuration
----------------------------------------

Navigate to :guilabel:`Integrations >> Pingdom (Email)`.

.. image:: /_images/spoc/Screen-Shot-2019-10-09-at-12.56.21-PM.png
   :alt: Pingdom integrations email option


If the integration has not already been enabled, enable the integration
and copy the email endpoint.

.. image:: /_images/spoc/3rd_Party_Integrations-EMStester-3.png
   :alt: Pingdom email endpoint example


:guilabel:`Routing Key` (+$routing_key) can be used to route an email endpoint-
initiated incident to a specific team or teams within Splunk. Replace $routing_key in the email endpoint
address with a valid routing_key from your Splunk On-Call instance.

Pingdom (Email) configuration
----------------------------------------

Navigate to :guilabel:`Users and Teams` in Pingdom and select :guilabel:`Users`.

Select :guilabel:`Add User`. When creating the new
Splunk On-Call user make sure to select, next to :guilabel:`Alert recipients`,
:guilabel:`Contact; can only receive alerts`.

.. image:: /_images/spoc/Screen-Shot-2019-10-09-at-12.28.04-PM-1.png
   :alt: Add user or contact menu

In contact details, give your contact a name that makes sense for the alert destination (such as Splunk). Then paste the Splunk On-Call Pingdom Email endpoint
into the contact method. Save the user by clicking :guilabel:`Add User`.

.. image:: /_images/spoc/Screen_Shot_2019-10-09_at_12_31_46_PM.png
   :alt: Adding a contact name

Under Experience Monitoring, add your new contact to your desired checks. 

You add the user to your desired checks by editing a
check and selecting that user for :guilabel:`Add Who to alert?`. Once selected and
saved, this check alerts your Splunk On-Call email endpoint.

.. image:: /_images/spoc/Screen-Shot-2019-10-09-at-12.38.25-PM.png
   :alt: Associating alert checks with a new contact
