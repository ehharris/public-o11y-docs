.. _apm-create-data-links-terraform:

*********************************************************************
Manage global data links to Splunk AppDynamics tiers with Terraform
*********************************************************************

.. meta::
   :description: An overview of how to use Terraform to create and delete global data links to Splunk AppDynamics tiers.

.. toctree::
      :hidden:
      :maxdepth:  3

      apm-create-data-links-terraform-batch
      apm-create-data-links-terraform-file
      apm-delete-data-links-terraform

.. note::
    You can only create a global data link from a Splunk APM inferred service to a Splunk AppDynamics tier if the tier is monitored by a Splunk AppDynamics SaaS environment.

When a transaction goes through a service monitored by Splunk APM as well as a tier monitored by Splunk AppDynamics, the service monitored by Splunk APM is considered an inferred service.

Create a global data link to link a Splunk APM inferred service to a Splunk AppDynamics tier. When you view the inferred service in Splunk APM, you can select the data link to navigate to the tier in the Splunk AppDynamics user interface.

Using Terraform, you can programmatically:

* (Recommended) :ref:`apm-create-data-links-terraform-batch`
* :ref:`apm-create-data-links-terraform-file`
* :ref:`apm-delete-data-links-terraform`

To create a data link using the user interface, see :ref:`apm-create-gdl-to-appd`. The user interface only supports creating one data link at a time.