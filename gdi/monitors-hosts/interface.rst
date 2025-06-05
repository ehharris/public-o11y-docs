.. _interface:

Interface traffic (deprecated)
===================================

.. meta::
   :description: Use this Splunk Observability Cloud integration for the interface monitor. See benefits, install, configuration, and metrics

.. note:: This integration is deprecated in favor of the ``net-io`` integration, which provides the same metrics. ``net-io`` uses the ``interface`` dimension to identify the network card instead of the ``plugin_instance`` dimension. To learn more, see :ref:`net-io`.

Configuration settings
----------------------

The following table shows the configuration options for this monitor:

.. list-table::
   :widths: 18 18 18 18
   :header-rows: 1

   - 

      - Option
      - Required
      - Type
      - Description
   - 

      - ``excludedInterfaces``
      - no
      - ``list of strings``
      - List of interface names to exclude from monitoring (**default:**
         ``[/^lo\d*$/ /^docker.*/ /^t(un|ap)\d*$/ /^veth.*$/]``)
   - 

      - ``includedInterfaces``
      - no
      - ``list of strings``
      - List of all the interfaces you want to monitor, all others will
         be ignored. If you set both ``included`` and
         ``excludedInterfaces``, only ``includedInterfaces`` will be
         honored.

Trobleshooting
--------------



.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>



