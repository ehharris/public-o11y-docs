.. _span-metrics-connector:

***********************************
Span Metrics connector
***********************************

.. meta::
      :description: Connects logs to traces.

The Splunk Distribution of the OpenTelemetry Collector uses the Span Metrics connector to aggregate requests, errors and duration (R.E.D) OpenTelemetry metrics from span data.

As an exporter, the supported pipeline type is ``traces``. As a receiver, the supported pipeline type is ``metrics``. See :ref:`otel-data-processing` for more information.

Overview 
======================

The connector pulls span data and aggregates them into Request, Error and Duration (R.E.D) OpenTelemetry metrics.

Requests
--------------------------------------------

Request counts are computed as the number of spans seen per unique set of dimensions, including Errors. Multiple metrics can be aggregated if, for instance, you wish to view call counts just on ``service.name`` and ``span.name``.

.. code-block:: 

  calls{service.name="shipping",span.name="get_shipping/{shippingId}",span.kind="SERVER",status.code="Ok"}

Errors
--------------------------------------------

Error counts are computed from the Request counts which have an Error Status Code metric dimension.

.. code-block:: 

  calls{service.name="shipping",span.name="get_shipping/{shippingId},span.kind="SERVER",status.code="Error"}

Duration
--------------------------------------------

Duration is computed from the difference between the span start and end times and inserted into the relevant duration histogram time bucket for each unique set dimensions.

.. code-block:: 

  duration{service.name="shipping",span.name="get_shipping/{shippingId}",span.kind="SERVER",status.code="Ok"}

Common dimensions for metrics
--------------------------------------------

Each metric has at least the following dimensions because they are common across all spans:

* ``service.name``
* ``span.name``
* ``span.kind``
* ``status.code``

Get started
======================

Follow these steps to configure and activate the component:

1. Deploy the Splunk Distribution of the OpenTelemetry Collector to your host or container platform:

  - :ref:`otel-install-linux`
  - :ref:`otel-install-windows`
  - :ref:`otel-install-k8s`

2. Configure the connector as described in the next section.
3. Restart the Collector.

Sample configuration
--------------------------------------------

To activate the connector, add ``spanmetrics`` to the ``connectors`` section of your configuration file. 

For example:

.. code-block:: yaml

  receivers:
    nop:

  exporters:
    nop:

  connectors:
    spanmetrics:
      histogram:
        explicit:
          buckets: [100us, 1ms, 2ms, 6ms, 10ms, 100ms, 250ms]
      dimensions:
        - name: http.method
          default: GET
        - name: http.status_code
      exemplars:
        enabled: true
      exclude_dimensions: ['status.code']
      dimensions_cache_size: 1000
      aggregation_temporality: "AGGREGATION_TEMPORALITY_CUMULATIVE"    
      metrics_flush_interval: 15s
      metrics_expiration: 5m
      events:
        enabled: true
        dimensions:
          - name: exception.type
          - name: exception.message
      resource_metrics_key_attributes:
        - service.name
        - telemetry.sdk.language
        - telemetry.sdk.name

To complete the configuration, in the ``service`` section of your configuration file, include the connector as an exporter in the ``traces`` pipeline and as a receiver in the ``metrics`` pipeline. 

For example:

.. code-block:: yaml

  service:
    pipelines:
      traces:
        receivers: [nop]
        exporters: [spanmetrics]
      metrics:
        receivers: [spanmetrics]
        exporters: [nop]

Configuration options
----------------------------------

The following settings can be optionally configured:

* ``histogram``. ``explicit`` by default. Use it to configure the type of histogram to record calculated from spans duration measurements. Must be either ``explicit`` or ``exponential``.

  * ``disable``. ``false`` by default. Disables all histogram metrics.

  * ``unit``. ``ms`` by default. The time unit for recording duration measurements, calculated from spans duration measurements. Possible values ar ``ms`` or ``s``.

  * ``explicit``:

    * ``buckets``. The list of durations defining the duration histogram time buckets. Default buckets: ``[2ms, 4ms, 6ms, 8ms, 10ms, 50ms, 100ms, 200ms, 400ms, 800ms, 1s, 1400ms, 2s, 5s, 10s, 15s]``.

  * ``exponential``:

    * ``max_size``. ``160`` by default. The maximum number of buckets per positive or negative number range.

* ``dimensions``. The list of dimensions to add together with the default dimensions defined above.

  * Each additional dimension is defined with a name which is looked up in the span's collection of attributes or resource attributes (process tags) such as ``ip``, ``host.name`` or ``region``.

  * If the ``named`` attribute is missing in the span, the optional provided default is used.

  * If no default is provided, this dimension will be omitted from the metric.

* ``exclude_dimensions``. The list of dimensions to be excluded from the default set of dimensions. Use to exclude unneeded data from metrics.

* ``dimensions_cache_size``. ``1000`` by default. The size of cache for storing Dimensions to improve Collectors' memory usage. Must be a positive number.

* ``resource_metrics_cache_size``. ``1000`` by default. The size of the cache holding metrics for a service. This is mostly relevant for cumulative temporality to avoid memory leaks and correct metric timestamp resets.

* ``aggregation_temporality``. ``AGGREGATION_TEMPORALITY_CUMULATIVE`` by default. Defines the aggregation temporality of the generated metrics. Possible values are ``AGGREGATION_TEMPORALITY_CUMULATIVE`` or ``AGGREGATION_TEMPORALITY_DELTA``.

* ``namespace``. Defines the namespace of the generated metrics. If a namespace's provided, the prefix ``namespace.`` is added to the generated metric name.

* ``metrics_flush_interval``. ``60`` by default. Defines the flush interval of the generated metrics.

* ``metrics_expiration``. ``0`` by default. Defines the expiration time as ``time.Duration``, after which, if no new spans are received, metrics will no longer be exported. If you set it to ``0`` metrics will never expire.

* ``exemplars``. Use them to configure how to attach exemplars to metrics.

  * ``enabled``. ``faulse`` by default. Enabling it will add spans as Exemplars to all metrics. Exemplars are only kept for one flush interval.

* ``events``. Use it to configure the events metric.

  * ``enabled``. ``false`` by default. Enabling it adds the events metric.

  * ``dimensions``: Mandatory if ``events`` is enabled. The list of the span's event attributes to add as dimensions to the events metric, which will be included on top of the common and configured dimensions for span and resource attributes.

* ``resource_metrics_key_attributes``. Filter the resource attributes used to produce the resource metrics key map hash. Use this in case changing resource attributes (e.g. process id) are breaking counter metrics.

.. _span-metrics-connector-migration:

Migrate from the Span Metrics processor to the Span Metrics connector
================================================================================================

The ``spanmetrics`` connector is a port of the :ref:`span-processor`, but with multiple improvements and breaking changes. It was done to bring the spanmetrics connector closer to the OpenTelemetry specification and make the component agnostic to exporters logic. 

The connector comes with the following changes:

* The ``operation`` metric attribute is renamed to ``span.name``.
* The ``latency`` histogram metric name is changed to ``duration``.
* The ``_total`` metric prefix was dropped from generated metrics names.
* The Prometheus-specific metrics labels sanitization was dropped.

Other improvements include:

* Added support for OTel exponential histograms for recording span duration measurements.
* Added support for the milliseconds and seconds histogram units.
* Added support for generating metrics resource scope attributes. The ``spanmetrics`` connector generates the number of metrics resource scopes that corresponds to the number of the spans resource scopes, so it generates more metrics. Previously, ``spanmetrics`` generated a single metrics resource scope.

.. _span-metrics-connector-prometheus:

Use the Span Metrics connector with Prometheus components
====================================================================

The ``spanmetrics`` connector can be used with Prometheus exporter components such as :ref:`prometheus-receiver`.

For some functionality of the exporters, for example, to generate the ``target_info`` metric, the incoming spans resource scope attributes must contain the ``service.name`` and ``service.instance.id`` attributes.

Let's look at the example of using the ``spanmetrics`` connector with the ``prometheusremotewrite`` exporter:

.. code-block:: yaml

  receivers:
    otlp:
      protocols:
        http:
        grpc:

  exporters:
    prometheusremotewrite:
      endpoint: http://localhost:9090/api/v1/write
      target_info:
        enabled: true

  connectors:
    spanmetrics:
      namespace: span.metrics

  service:
    pipelines:
      traces:
        receivers: [otlp]
        exporters: [spanmetrics]
      metrics:
        receivers: [spanmetrics]
        exporters: [prometheusremotewrite]

This configures the ``spanmetrics`` connector to generate metrics from received spans and export those metrics to the Prometheus Remote Write exporter. The ``target_info`` metric is generated for each resource scope, while OpenTelemetry metric names and attributes are normalized to be compliant with Prometheus naming rules. 

For example, the generated calls OTel sum metric can result in multiple Prometheus ``calls_total`` (counter type) time series and the ``target_info`` time series. See below:

.. code-block:: 

  target_info{job="shippingservice", instance="...", ...} 1
  calls_total{span_name="/Address", service_name="shippingservice", span_kind="SPAN_KIND_SERVER", status_code="STATUS_CODE_UNSET", ...} 142

.. _span-metrics-connector-troubleshooting:

Troubleshooting
======================

.. raw:: html

   <div class="include-start" id="troubleshooting-components.rst"></div>

.. include:: /_includes/troubleshooting-components.rst

.. raw:: html

   <div class="include-stop" id="troubleshooting-components.rst"></div>





