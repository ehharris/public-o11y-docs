.. _sso-google-cloud-identity:

*********************************************************************
Configure a Google Cloud Identity SSO integration
*********************************************************************

.. meta::
   :description: Splunk Observability Cloud provides the capability for your users to log in using various SSO providers. The Google Cloud Identity (GCI) SSO integration lets users log in to Splunk Observability Cloud using their Google Cloud credentials.

The Google Cloud Identity (GCI) SSO integration lets users log in to Splunk Observability Cloud
using their Google Cloud credentials.

Before you begin to configure the Google Cloud Identity integration, ensure you have completed the steps in :new-page-ref:`sso-label`, including the section :ref:`Name an SSO integration<naming-note-sso>` to learn about naming your integrations.

To configure GCI as an IdP using a Splunk Observability Cloud SSO integration,
you must be an administrator for your organization and a super-administrator of your Google domain.
To learn more, see :new-page-ref:`admin-manage-users`.

The :new-page:`G Suite Administrator Help document <https://support.google.com/a/answer/7623225?hl=en>`
topic, developed by Google, describes how to configure the integration.

After you complete these steps, the GCI SSO integration is available to
users in your GCI organization. When users sign in to Splunk Observability Cloud
from GCI for the first time, they receive an email containing a link that
they must open in order to authenticate. This only occurs the first time the user
signs in. Subsequent login attempts don't require validation.

If you want to turn off the email authentication feature, contact :ref:`support`.

Once you have a custom URL configured, your users can continue to log in using their existing username/password pair, or they can use their GCI credentials instead. GCI SSO authentication and Splunk Observability Cloud username/password authentication are independent.



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



