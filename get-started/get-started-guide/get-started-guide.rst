.. _get-started-guide:

Get started guide for Splunk Observability Cloud admins 
*********************************************************

.. toctree::
   :hidden:
   :maxdepth: 3

   Phase 1: Onboarding readiness <onboarding-readiness>
   Phase 2: Initial rollout <initial-rollout>
   Phase 3: Scaled rollout <scaled-rollout>

The journey for getting started with Splunk Observability Cloud has 3 phases: onboarding readiness, initial rollout, and scaled rollout. In the onboarding readiness phase, you set up users, teams, and access controls using roles and token management and lay the groundwork for connectivity. Next, in the initial rollout phase, you get your data into Splunk Observability Cloud and set up the Splunk Observability Cloud products for your initial project team use cases. In the final scaled rollout phase, you establish repeatable observability practices using automation, data management, detectors, and dashboards.  

.. raw:: html
  
    <embed>
      <h2>How to use this guide<a name="use-guide" class="headerlink" href="#use-guide" title="Permalink to this headline">¶</a></h2>
    </embed>


* Use the following table to get a high-level overview of the primary setup steps involved in each phase. 
* Use the links for each step to go directly to the detailed instructions or go to the phase topic to view all phase steps in detail. 
* In the table, you can also reference optional and advanced configurations that you can make to your setup as part of each phase of your journey. 
* Use the links to education resources for each phase to ensure you have the foundational knowledge and skills to successfully set up Splunk Observability Cloud.

.. note:: This guide is for Splunk Observability Cloud users with the admin role. 
  
.. image:: /_images/get-started/o11y_onboardingGuideFlow_full-flow.svg
   :width: 100%
   :alt: Flow showing the 3 phases of the get started journey: onboarding, initial rollout, and scaled rollout.

.. list-table:: 
   :header-rows: 1
   :widths: 10 30 30 30
   :width: 100%

   * - :strong:`Information type`
     - :strong:`Phase 1: Onboarding readiness`
     - :strong:`Phase 2: Initial rollout`
     - :strong:`Phase 3: Scaled rollout`

   * - :strong:`Phase description`
     - Set up users, teams, and access controls through roles and token management and lay the groundwork for connectivity
     - Bring data in and set up the Splunk Observability Cloud products for your initial project team use cases 
     - Increase usage across all user teams and establish repeatable observability practices through automation, data management, detectors, and dashboards

   * - :strong:`Primary setup steps`
     - #. :ref:`phase1-create-trial`
       #. :ref:`phase1-network`
       #. :ref:`phase1-user-access`
       #. :ref:`phase1-teams-tokens`

       See :ref:`get-started-guide-onboarding-readiness` for detailed steps.

     - #. :ref:`phase2-initial-environment`
       #. :ref:`phase2-infra-mon`
       #. :ref:`phase2-apm`
       #. :ref:`phase2-rum`
       #. :ref:`phase2-synthetics`

       See :ref:`get-started-guide-initial-rollout` for detailed steps.

     - #. :ref:`phase3-pipeline`
       #. :ref:`phase3-rotate-token`
       #. :ref:`phase3-mpm`
       #. :ref:`phase3-names-data`
       #. :ref:`phase3-dash-detect`
       #. :ref:`phase3-onboard-all`

       See :ref:`get-started-guide-scaled-rollout` for detailed steps.

   * - :strong:`Optional and advanced configurations`
     - * :ref:`advanced-config-custom-URL`
       * :ref:`advanced-config-parent-child`
       * :ref:`advanced-config-logs`

       See :ref:`Phase 1 optional and advanced configurations <phase1-advanced-config>`.

     - * :ref:`advanced-config-3rd-party`
       * :ref:`phase2-network-exp`
       * :ref:`phase2-profiling`
       * :ref:`phase2-related-content`

       See :ref:`Phase 2 optional and advanced configurations <phase2-advanced-config>`.

     - * :ref:`phase3-data-links`
       * :ref:`phase3-usage-limits`

       See :ref:`Phase 3 optional and advanced configurations <phase3-advanced-config>`.

   * - :strong:`Education resources`
     - * :new-page:`Free Splunk Observability Cloud courses<https://www.splunk.com/en_us/training/free-courses/overview.html#observability>`
       * :new-page:`Full course catalog for Splunk Observability Cloud <https://www.splunk.com/en_us/training/course-catalog.html?filters=filterGroup4SplunkObservabilityCloud>`
           * See the :new-page:`Curated started track for Splunk Observability Cloud <https://drive.google.com/file/d/1LHZL1jaP8irQvfI3HG71XcgGavgEn5cD/view>` to determine what courses to prioritize.
       * :new-page:`Splunk Observability Cloud metrics user certification <https://www.splunk.com/en_us/training/course-catalog.html?filters=filterGroup2SplunkO11yCloudCertifiedMetricsUser>`
     - * :new-page:`Get familiar with OpenTelemetry concepts <https://opentelemetry.io/docs/concepts/>`
       * To learn more about the data model for Splunk Observability Cloud, see :ref:`data-model`
     - * See :ref:`otel-sizing` to learn about OpenTelemetry sizing requirements.
       * :new-page:`Splunk Observability Cloud Workshops<https://splunk.github.io/observability-workshop/latest/en/index.html>`
       * :new-page:`Curated training curriculum for Splunk Observability Cloud end users<https://drive.google.com/file/d/1LHZL1jaP8irQvfI3HG71XcgGavgEn5cD/view>`