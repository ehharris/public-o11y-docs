.. _collector-kubernetes-intro:

***********************************************************
Get started with the Collector for Kubernetes
***********************************************************

.. meta::
   :description: Introduction to the Splunk Distribution of OpenTelemetry Collector for Kubernetes.

.. toctree::
   :maxdepth: 5
   :hidden:

   Install with Helm <install-k8s.rst>
   Helm components <kubernetes-helm-architecture.rst>
   Helm releases and images <kubernetes-helm-releases.rst>
   Helm commands reference <otel-commands.rst>
   Install with YAML manifests <install-k8s-manifests.rst>
   Kubernetes (EKS Add-on) <install-k8s-addon-eks.rst>
   Configure with Helm <kubernetes-config.rst>
   Add components and data sources <kubernetes-config-add.rst>
   Configure logs and events <kubernetes-config-logs.rst>
   Advanced configuration <kubernetes-config-advanced.rst>
   Default Kubernetes metrics <metrics-ootb-k8s.rst>
   Upgrade <kubernetes-upgrade.rst>
   Uninstall <kubernetes-uninstall.rst>
   Troubleshoot <k8s-troubleshooting/troubleshoot-k8s-landing.rst>
   Support <kubernetes-support.rst>
   Tutorial: Monitor your Kubernetes environment <k8s-infrastructure-tutorial/about-k8s-tutorial.rst>
   Tutorial: Configure the Collector for Kubernetes <collector-configuration-tutorial-k8s/about-collector-config-tutorial.rst>

To install, configure and use the Splunk Distribution of the Collector for Kubernetes follow this documentation.

.. raw:: html

  <embed>
    <h2>Install the Collector for Kubernetes<a name="k8s-install" class="headerlink" href="#k8s-install" title="Permalink to this headline">¶</a></h2>
  </embed>

To install the Splunk Distribution of the OpenTelemetry Collector for Kubernetes using the Helm chart read and follow these docs:

* :ref:`otel-install-k8s`
* :ref:`kubernetes-helm-architecture`
* :ref:`kubernetes-helm-releases`

Optionally, you can also:

* :ref:`otel-install-k8s-manifests`
* :ref:`Deploy the Collector as an AWS EKS Add-on <install-k8s-addon-eks>`
* Deploy the Collector using the Technical Add-on, which provides out-of-the box Collector content and configuration. Learn more at :ref:`collector-addon-intro`

.. raw:: html

  <embed>
    <h2>Configure the Collector for Kubernetes<a name="k8s-configure" class="headerlink" href="#k8s-configure" title="Permalink to this headline">¶</a></h2>
  </embed>

.. note:: To deploy the Collector for Kubernetes and automatically find services and applications running in your environment refer to :ref:`discovery-kubernetes`.   

To configure the Collector see:

* :ref:`otel-kubernetes-config`
* :ref:`kubernetes-config-add` 
* :ref:`kubernetes-config-logs`
* :ref:`otel-kubernetes-config-advanced`

.. raw:: html

   <div class="include-start" id="gdi/collector-common-options.rst"></div>

.. include:: /_includes/gdi/collector-common-options.rst

.. raw:: html

   <div class="include-stop" id="gdi/collector-common-options.rst"></div>

.. raw:: html

  <embed>
    <h2>Metrics and other telemetry<a name="k8s-metrics" class="headerlink" href="#k8s-metrics" title="Permalink to this headline">¶</a></h2>
  </embed>

If you use the default configuration for the Splunk Distribution of the Collector you'll obtain these :ref:`Kubernetes metrics <ootb-metrics-k8s>`. 

To obtain additional telemetry from other sources see:

* :ref:`kubernetes-config-add`
* :ref:`receiver-creator-receiver` to create new receivers at runtime  

.. raw:: html

  <embed>
    <h2>Upgrade, uninstall and troubleshoot<a name="k8s-ts" class="headerlink" href="#k8s-ts" title="Permalink to this headline">¶</a></h2>
  </embed>

To upgrade or uninstall the Collector see:

* :ref:`otel-kubernetes-upgrade`
* :ref:`otel-kubernetes-uninstall`

If you have any installation or configuration issues, refer to: 

* :ref:`troubleshoot-k8s-landing` 
* :ref:`kubernetes-support`

.. raw:: html

  <embed>
    <h2>Tutorials<a name="k8s-tutorials" class="headerlink" href="#k8s-tutorials" title="Permalink to this headline">¶</a></h2>
  </embed>

For a walkthrough of common tasks related to the OpenTelemetry Collector for Kubernetes, see:

* :ref:`about-k8s-tutorial`
* :ref:`about-collector-configuration-tutorial-k8s`





