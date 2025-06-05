.. _browser-rum-install:

*******************************************************************************
Install the Browser RUM agent for Splunk RUM
*******************************************************************************

.. meta::
   :description: The Browser RUM agent from the Splunk Distribution of OpenTelemetry JavaScript for Web provides a Real User Monitoring (RUM) instrumentation framework for your browser-based web applications. Use it to send RUM data from your front end to Splunk RUM.

You can instrument the front end of your web applications for Splunk RUM using the Browser RUM agent from the Splunk Distribution of OpenTelemetry JavaScript for Web.

To instrument your browser application and get data into Splunk RUM, follow the instructions on this page.

.. _rum-browser-requirements:

Check compatibility and requirements
==============================================



.. raw:: html

   <div class="include-start" id="requirements/browser.rst"></div>

.. include:: /_includes/requirements/browser.rst

.. raw:: html

   <div class="include-stop" id="requirements/browser.rst"></div>







.. raw:: html

   <div class="include-start" id="requirements/realm.rst"></div>

.. include:: /_includes/requirements/realm.rst

.. raw:: html

   <div class="include-stop" id="requirements/realm.rst"></div>





.. _rum-browser-install:

Instrument your web application for Splunk RUM
====================================================================

Before you instrument and configure Splunk RUM for your web application, understand which data RUM collects about your application and determine the scope of what you want to monitor. See :ref:`rum-data-collected`.

Select one of the following methods to instrument your web application.

.. list-table::
   :header-rows: 1
   :width: 100%

   * - Installation method
     - Use cases
     - Considerations

   * - :ref:`rum-browser-install-cdn`
     - Multi-page websites
     - Quick integration with your site or applciation. Ad-blockers might interfere with loading.

   * - :ref:`rum-browser-install-self-hosted`
     - Multi-page websites
     - Provides greater control over the installation. Updates are entirely manual.

   * - :ref:`rum-browser-install-npm`
     - Single-page applications
     - Bundles into your web application. Must be loaded as soon as possible. See :ref:`loading-initializing_browser-rum` for more information.


.. _rum-browser-install-cdn:

Splunk CDN
----------------------------------------------------------------------

You can use the Splunk Content Delivery Network (CDN) to load the Browser RUM agent synchronously. The CDN link ensures that your application always uses the latest version.

Decide which version to run in your environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The versioning of the Browser RUM agent follows semantic versioning rules. To have more control over the version you load, see the following versioning policy:

* Use the latest version to use the latest version of the Browser RUM agent. In preproduction, use ``latest`` to try out the most recent version of Splunk RUM. Don't use in production environments without prior testing. This version might not be suitable for manual instrumentation, as breaking API changes might occur between major version changes.
* Use major versions, for example ``v1``, if you want to receive new features automatically while keeping backward compatibility with the API. This is the default for all production deployments, as well as for npm installations.
* Use minor versions, for example ``v1.1``, to receive bug fixes while not receiving new features automatically.
* Use patch versions, for example, ``v1.2.1``, to pin a specific version of the agent for your application.

The versions of the agent are included in URLs as a designated token. For example:

``https://cdn.signalfx.com/o11y-gdi-rum/v<MAJOR.MINOR.PATCH>/splunk-otel-web.js``

In production environments, use the pinned version which was previously tested in preproduction and update the production version on a monthly cycle.

Add the CDN script to your application
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Follow these steps to instrument your application with the CDN:

#. Customize the following snippet:

   .. code-block:: html

      <!--//

      IMPORTANT: Replace the <version> placeholder in the src URL with a
      version from https://github.com/signalfx/splunk-otel-js-web/releases

      //-->
      <script src="https://cdn.signalfx.com/o11y-gdi-rum/<version>/splunk-otel-web.js" crossorigin="anonymous"></script>
      <script>
         SplunkRum.init({
            realm: '<realm>',
            rumAccessToken: '<your_rum_token>',
            applicationName: '<your_app_name>',
            version: '<your_app_version>',
            deploymentEnvironment: '<your_environment_name>'
         });
      </script>

   * In the URL of the script, replace ``<version>`` with a version from the :new-page:`Releases page in GitHub <https://github.com/signalfx/splunk-otel-js-web/releases>`.

   * ``realm`` is the Splunk Observability Cloud realm, for example, ``us0``. To find your Splunk realm, see :ref:`Note about realms <about-realms>`.

   * To generate a RUM access token, see :ref:`rum-access-token`.

#. Add the snippet to the head section of every page you want to monitor in your application.

#. Deploy the changes to your application. Make sure to test the instrumentation in a pre-production environment before deploying to production.

.. caution:: Don't use the ``latest`` version in production without prior testing.


.. _rum-browser-install-self-hosted:

Self-hosted script
------------------------------------------------------

To use your own CDN or comply with your own deployment requirements, instrument your application using a self-hosted script. When you host the script, you need to update to newer versions of the agent manually.

Follow these steps to instrument your application using a self-hosted script:

#. Go to :new-page:`splunk-otel-js-web <https://github.com/signalfx/splunk-otel-js-web/releases>` in GitHub and download the ``splunk-otel-web.js`` file for the release you want to use.

#. Deploy the files in a location accessible by the users of your application.

#. Customize the following snippet:

   .. code-block:: html

      <script src="http://example.domain/path/splunk-otel-web.js"></script>
      <script>
         SplunkRum.init({
            realm: '<realm>',
            rumAccessToken: '<your_rum_token>',
            applicationName: '<your_app_name>',
            version: '<your_app_version>',
            deploymentEnvironment: '<your_environment_name>'
         });
      </script>

   * ``realm`` is the Splunk Observability Cloud realm, for example, ``us0``. See :new-page:`Realms in endpoints <https://dev.splunk.com/observability/docs/realms_in_endpoints>`.
   * To generate a RUM access token, see :ref:`rum-access-token`.

#. Add the snippet to the head section of every page you want to monitor in your application.

#. Deploy the changes to your application. Make sure to test the instrumentation in a pre-production environment before deploying to production.


.. _rum-browser-install-npm:

npm package
------------------------------------------------

To bundle the Browser RUM agent directly with your application, use the ``@splunk/otel-web`` npm package.

Follow these steps to instrument and configure Splunk RUM using npm:

#. Enter the following command to install the Browser RUM agent and add it to your package.json file:

   .. code-block:: shell

      npm install @splunk/otel-web --save

#. Create the ``splunk-instrumentation.js`` initialization file next to your bundle root file. The following snippet contains sample content for the initialization file:

   .. code-block:: javascript

      import SplunkOtelWeb from '@splunk/otel-web';
      SplunkOtelWeb.init({
         realm: '<realm>',
         rumAccessToken: '<your_rum_token>',
         applicationName: '<your_application_name>',
         version: '<your_app_version>',
         deploymentEnvironment: '<your_environment_name>'
      });

   * ``realm`` is the Splunk Observability Cloud realm, for example, ``us0``. To find the realm name of your account, follow these steps:

         1. Open the navigation menu in Splunk Observability Cloud.
         2. Select :menuselection:`Settings`.
         3. Select your username.

      The realm name appears in the :guilabel:`Organizations` section.

   * To generate a RUM access token, see :ref:`rum-access-token`.

#. Import or require the ``splunk-instrumentation.js`` file before other files to ensure that the instrumentation runs before the application code.

#. Deploy the changes to your application. Make sure to test the instrumentation in a pre-production environment before deploying to production.

.. note:: Make sure the Splunk RUM agent doesn't run in Node.js. To instrument Node.js services for Splunk APM, see :ref:`get-started-nodejs`.


.. _loading-initializing_browser-rum:

Load and initialize the Browser RUM agent
========================================================

To avoid gaps in your data, load and initialize the Browser RUM agent synchronously and as early as possible. Delayed loading might result in missing data, as the instrumentation cannot collect data before it's initialized.

Use one the following methods to load and initialize the Browser RUM agent, in order of effectiveness:

* Synchronously load the Browser RUM agent as the first resource, or at least the first JS resource, in the head section. The Browser RUM agent JavaScript file must be loaded before any other JS file. This ensures that the instrumentation collects all user interactions, resources, and errors.
* Bundle the Browser RUM agent with other application scripts. Place the Browser RUM agent at the top of the bundle and make sure the bundle loads synchronously.

If you defer the loading of the Browser RUM agent, make sure other scripts are also deferred to preserve the initialization order. Note that asynchronously loaded scripts are not supported.

Set up Session Replay
================================================

With Session Replay you can replay a session to take a look at exactly what the user experienced and make informed decisions about what to do next.

See :ref:`rum-session-replay` for instructions on how to set up and configure Session Replay.

.. _modify-spans:

Customize your RUM data intake
=================================================

You can customize the data intake for the Browser RUM agent to reduce noise and redact information.

Opt out of error.message collection
------------------------------------------------

To avoid collecting ``error.message`` responses, deactivate the errors instrumentation as in the following example:

.. code-block:: html
   :emphasize-lines: 8

   <script src="https://cdn.signalfx.com/o11y-gdi-rum/latest/splunk-otel-web.js" crossorigin="anonymous"></script>
   <script>
      SplunkRum.init({
         realm: '<realm>',
         rumAccessToken: '<your_rum_token>',
         applicationName: '<your_app_name>',
         version: '<your_app_version>',
         instrumentations: { errors: false }
      });
   </script>


Change attributes before they're collected
----------------------------------------------------------------

To remove or change attributes in your spans, see :ref:`rum-browser-redact-pii`.


Collect errors with single-page application frameworks
------------------------------------------------------------------

To collect errors when using single-page application frameworks, see :ref:`rum-browser-spa-errors`.


.. _rum-apm-connection:

Link RUM with Splunk APM
==================================

Splunk RUM uses server timing to calculate the response time between the front end and back end of your application, and to join the front-end and back-end traces for end-to-end visibility.

By default, the Splunk Distributions of OpenTelemetry already send the ``Server-Timing`` header. The header links spans from the browser with back-end spans and traces.

.. note::  When linking sessions from Splunk RUM to Splunk APM while using the Safari browser, note that Safari supports linking XHR and fetch requests to Splunk APM, but doesn't support linking page loads or resource loads to Splunk APM.


Instrument WebViews in Mobile applications
=============================================

You can instrument WebViews in your iOS and Android applications by sharing the `splunk.rumSessionId` between the mobile instrumentation and the web instrumentation. This lets you see data from both your native app and your web app in a single stream.

To instrument WebViews, follow the instructions for the app's operating system:

* :ref:`Android WebViews <android-webview-instrumentation>`
* :ref:`iOS WebViews <ios-webview-instrumentation>`


Considerations for content security policy
=================================================

If your application uses Content Security Policy (CSP) to mitigate potential impact from cross-site scripting (XSS) and other attacks, make sure the policy allows Splunk RUM to run:

- When using the CDN version of the agent, allow the ``script-src cdn.signalfx.com`` URL.
- When self-hosting or using the npm package, configure your site accordingly.
- Add the host from the ``beaconEndpoint`` property to the ``connect-src`` property. For example: ``connect-src rum-ingest.us1.signalfx.com``.


How to contribute
=========================================================

The Splunk Distribution of OpenTelemetry JavaScript for Web is open-source software. You can contribute to its improvement by creating pull requests in GitHub. To learn more, see the :new-page:`contributing guidelines <https://github.com/signalfx/splunk-otel-js-web/blob/main/CONTRIBUTING.md>` in GitHub.
