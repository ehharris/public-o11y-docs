.. _set-up-transactional-browser-test:

*********************************************************
Add synthetic transactions to your Browser Test 
*********************************************************

.. meta::
    :description: Customize your browser tests by adding a synthetic transaction that monitors a multi-step user flow in Splunk Synthetic Monitoring. For example, a checkout flow that requires various interactions on a webpage. 

Use synthetic transactions in your Browser Tests to test multi-step user flows and transactions. 

What are synthetic transactions?
================================
A synthetic transaction is a group of one or more logically related interactions that represent a business-critical user flow, such as the login process or a checkout flow. Synthetic transactions are made up of steps. 

Splunk Synthetic Monitoring generates the following additional metrics for each synthetic transaction: 

* :strong:`Duration:` Total duration for the synthetic transaction
* :strong:`Requests:` Total number of requests made during the synthetic transaction
* :strong:`Size:` Total size of the content loaded during the synthetic transaction

You can create multiple synthetic transactions in a given test to measure size, duration, and requests for multiple workflows across multiple pages. 

.. _bt-steps:

What are steps?
=================
Steps are interactions with a webpage that require user input. By adding steps to your Browser Test, you can simulate choices or interactions that a user might make and test how your application behaves in response. 

Example steps include:

* Clicking a link
* Entering information in a field
* Selecting a value from a drop-down menus
* Accepting or dismissing an alert
* Running and saving values from JavaScript
* Saving text from an element

You can identify specific page elements involved in these steps by their id, name, XPATH, CSS Path, link, or JS path. Use developer tools to interact with the DOM (inspector) and JavaScript (console) to identify and interact with the elements of a site.

To learn more about all available types of steps, see :ref:`step-types` below.

A step doesn't generate its own dedicated metrics, but it counts toward the metrics for the synthetic transaction it belongs to. Additionally, when a step triggers a new page load, the page load triggers the set of page load metrics. 

.. _step-types:

Types of steps you can include in your Browser Tests
-----------------------------------------------------------
Assertions are checks to see what state an object exists in, for example if text is present or an element is visible.  
The following table describes the types of steps you can include for actions: 

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - :strong:`Type of step`
     - :strong:`Description`

   * - Accept alert
     - Accept an alert that appears on the page.

   * - Clear
     - Clear an element you identify under :guilabel:`Selector`. Optionally, wait for navigation. 

   * - Click
     - Click on an element you identify under :guilabel:`Selector`. Optionally, wait for navigation. 

   * - Dismiss alert
     - Dismiss an alert that appears on the page.

   * - Fill in field
     - Fill in the field you identify under :guilabel:`Selector` with a value you provide in the :guilabel:`Value` field. For security and reusability, you may want to use a :ref:`built-in  <built-in-variables>`, :ref:`custom <custom-variables>`, or :ref:`global <global-variables>` variable in the :guilabel:`Value` field. Reference a custom variable as ``{{custom.your-variable-name}}``. Optionally, wait for navigation.

   * - Go to URL
     - Navigate to a URL you provide in the :guilabel:`URL` field. 

   * - Execute JavaScript
     - Execute a piece of JavaScript you provide under :guilabel:`Value`. Optionally, wait for navigation. 

   * - Save return value from Javascript 
     - Execute a piece of JavaScript you specify in the :guilabel:`JavaScript` field. If the script returns a value you want to save, specify a name for the saved value in the :guilabel:`Variable` field. This creates a custom variable which you can reference in subsequent steps as ``{{custom.your-variable-name}}``. Optionally, wait for navigation.

   * - Save text from element 
     - Save the text an element you identify under :guilabel:`Selector`, as the variable you provide under :guilabel:`Variable`. 

   * - Select 
     - Select an element you identify under :guilabel:`Selector`. Choose the value you identify under :guilabel:`Value`. Optionally, wait for navigation.

   * - Switch to iframe
     - Switch focus to an embedded document in an inline frame, identified under :guilabel:`Selector`. 

   * - Switch to main
     - Switch focus back to the main frame of the webpage.

   * - Wait
     - Wait a certain number of minutes. See, :ref:`configurable-wait-times`.




.. _add-transactions:

Add synthetic transactions to your Browser Test 
===================================================
Follow these steps to create a Browser Test with synthetic transactions:

#. From the Splunk Synthetic Monitoring landing page, click :guilabel:`Add new test > Browser test` to start creating a Browser Test. See :ref:`set-up-browser-test` for more details.
#. While creating your Browser Test, select :guilabel:`Edit steps or synthetic transactions`. Your current configuration and detector selections are preserved and the :guilabel:`Add synthetic transactions` view opens.  
#. Enter a name for your synthetic transaction, such as "Log in" or "Begin search."
#. Enter a name for the first step in your synthetic transaction.
#. Use the picker to choose the type of step from the dropdown. See :ref:`step-types` to learn more about the options.
#. If your step type requires you identify an element by :guilabel:`Selector`, enter the following. 

      * Selector type: Choose the selector type, from among id, name, XPATH, CSS Path, link, or JS path
      * Selector path: Enter the path used to identify the selector you're using, conforming to the selector type you chose. 

#. If your step type requires that you enter a :guilabel:`Value`, you can either type a raw value, or use a built-in, custom, or Global Variable here. You can select a variable name from the :guilabel:`Variables` tab to copy it and paste it in the field where you'd like it to be entered.
#. If your step type has the option to :guilabel:`Wait for navigation`, check the checkbox if you'd like the test to wait for a 2 second delay to allow the specified action to be executed. 
#. (Optional) Create additional steps and synthetic transactions using the :guilabel:`+ Step` and :guilabel:`+ Synthetic transaction` buttons. Click and drag steps and synthetic transactions to rearrange their order. 
#. (Optional) Use the :guilabel:`Test settings` tab to adjust your test configuration settings. See :ref:`test-config` to learn more.
#. (Optional) Use the :guilabel:`Detectors` tab to add detectors to your test. See :ref:`synth-alerts` to learn more.
#. When you're satisfied with your transactional Browser Test, select :guilabel:`Save`.
