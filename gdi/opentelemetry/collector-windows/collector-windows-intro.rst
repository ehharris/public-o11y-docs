.. _collector-windows-intro:

***********************************************************
Get started with the Collector for Windows
***********************************************************

.. meta::
   :description: Introduction to the Splunk Distribution of the OpenTelemetry Collector for Windows.

.. toctree::
   :maxdepth: 5
   :hidden:

   Install the Collector for Windows (script) <install-windows.rst>
   Install the Collector for Windows (MSI) <install-windows-msi.rst>
   Install the Collector for Windows (tools) <install-windows-tools.rst>
   Install the Collector for Windows (manually) <install-windows-manual.rst>       
   windows-config-ootb.rst
   windows-config.rst
   windows-config-logs.rst
   metrics-ootb-windows.rst   
   windows-upgrade.rst   
   windows-uninstall.rst

To install the Splunk Distribution of the OpenTelemetry Collector for Windows, follow these docs:

* :ref:`otel-install-windows`
* :ref:`otel-install-windows-msi` 
* :ref:`otel-install-windows-tools`
* :ref:`otel-install-windows-manual`
* You can also deploy the Collector using the Technical Add-on, which provides out-of-the box Collector content and configuration. Learn more at :ref:`collector-addon-intro`.

See the default settings and configuration options at:

* :ref:`windows-config-ootb`
* By default, you'll obtain these :ref:`metrics <ootb-metrics-windows>` 
* :ref:`otel-windows-config`
* :ref:`windows-config-logs`



.. raw:: html

   <div class="include-start" id="gdi/collector-common-options.rst"></div>

.. include:: /_includes/gdi/collector-common-options.rst

.. raw:: html

   <div class="include-stop" id="gdi/collector-common-options.rst"></div>




To automatically find services and applications running in your Windows environment and send data from them to Splunk Observability Cloud refer to :ref:`discovery-windows`.        

To upgrade or uninstall, see:

* :ref:`otel-windows-upgrade`
* :ref:`otel-windows-uninstall`

.. note:: If you have any installation or configuration issues, refer to :ref:`otel-troubleshooting`.

