.. _instrument-python-frameworks:

***************************************************************
Instrument Python frameworks for Splunk Observability Cloud
***************************************************************

.. meta::
   :description: If you're instrumenting a Python app that uses Django or uWSGI, perform these additional steps after you've followed the common procedure for zero-code instrumentation.

If you're instrumenting a Python application or service that uses Django or uWSGI, follow these additional steps after you've followed all the steps in :ref:`instrument-python-applications`.

.. _django-instrumentation:

Instrument your Django application
========================================

To automatically instrument Django applications, set the ``DJANGO_SETTINGS_MODULE`` environment variable to the same value in manage.py or wsgi.py.

For example, if your manage.py file sets the environment variable to mydjangoproject.settings and you start your project using ``./manage.py runserver``, you can run the following commands:

.. tabs::

   .. code-tab:: bash Linux

         export DJANGO_SETTINGS_MODULE=mydjangoproject.settings
         opentelemetry-instrument python3 ./manage.py runserver --noreload

   .. code-tab:: shell Windows PowerShell

         $env:DJANGO_SETTINGS_MODULE=mydjangoproject.settings
         opentelemetry-instrument python3 ./manage.py runserver --noreload

.. _uwsgi-instrumentation:

Instrument your uWSGI application
========================================

When using uWSGI, you must configure tracing as a response to the ``post_fork`` signal:

.. code-block:: python

   import uwsgidecorators
   from splunk_otel import init_splunk_otel

   @uwsgidecorators.postfork
   def start_otel():
      init_splunk_otel()

Customize and use the following snippet to run the application:

.. code-block:: bash

   uwsgi --http :9090 --wsgi-file <your_app.py> --callable <your_wsgi_callable> --master --enable-threads

Place the snippet in the main Python script that uWSGI imports and loads.

.. caution:: Do not run your uWSGI application using ``splunk-py-trace``, as it could have unintended consequences.

uWSGI and Flask
-------------------------------------------

When using both uSWGI and Flask, calling ``start_tracing()`` only autoinstruments new Flask instances. To instrument an existing Flask app, import and call the Flask instrumentor:

.. code-block:: python

   # app.py
   import uwsgidecorators
   from splunk_otel import init_splunk_otel
   from opentelemetry.instrumentation.flask import FlaskInstrumentor
   from flask import Flask

   app = Flask(__name__)

   @uwsgidecorators.postfork
   def setup_otel():
      init_splunk_otel()
      # Instrument the Flask app instance explicitly
      FlaskInstrumentor().instrument_app(app)

   @app.route('/')
   def hello_world():
      return 'Hello, World!'

Customize and use the following snippet to run the application:

.. code-block:: bash

   uwsgi --http :9090 --wsgi-file <your_app.py> --callable <your_wsgi_callable> --master --enable-threads
