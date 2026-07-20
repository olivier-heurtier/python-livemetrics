
OpenMetrics 1.0 Mapping
=======================

See `OpenMetrics <https://github.com/prometheus/OpenMetrics/blob/main/specification/OpenMetrics.md>`_

Status
------

See https://github.com/prometheus/OpenMetrics/blob/main/specification/OpenMetrics.md#info

/monitoring/v1/about::

    HTTP/1.1 200 OK
    Content-Type: text/plain

    This is service zzz version NN

/monitoring/v1/version::

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "version": "1.0.0",
        "build_number": "8152",
        "build_machine": "mph-p6001234",
        "build_time": "2017-10-16T05:02:17+02:00",
        "hostname": "mcp-50-50-1-20",
        "custo_version": "2.0.1",
        "custo_name": "utopia",
        "custo_build_number": "342",
        "custo_build_machine": "mph-p6001234",
        "custo_build_time": "2017-11-29T09:33:28+02:00"
    }

They both transform to::

    HTTP/1.1 200 OK
    Content-Type: application/openmetrics-text; version=1.0.0; charset=utf-8

    # TYPE about info
    about_info{name="This is services zzz version NN", version="1.0.0", build_number="8152"...} 1

See https://github.com/prometheus/OpenMetrics/blob/main/specification/OpenMetrics.md#stateset

/monitoring/v1/is_ready::

    HTTP/1.1 200 OK

It transforms to::

    HTTP/1.1 200 OK
    Content-Type: application/openmetrics-text; version=1.0.0; charset=utf-8

    # TYPE is_ready stateset
    is_ready{is_ready="is_ready"} 1


See https://github.com/prometheus/OpenMetrics/blob/main/specification/OpenMetrics.md#stateset

/monitoring/v1/is_healthy::

    HTTP/1.1 200 OK

It transforms to::

    HTTP/1.1 200 OK
    Content-Type: application/openmetrics-text; version=1.0.0; charset=utf-8

    # TYPE is_healthy stateset
    is_healty{is_healthy="is_healthy"} 1


Gauges
------

See https://github.com/prometheus/OpenMetrics/blob/main/specification/OpenMetrics.md#gauge

/monitoring/v1/metrics/gauges::

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "object": {
            "min": 8,
            "max": 8,
            "count": 8
        },
    }

where object can be, for example, nb_persons

It transforms to::

    HTTP/1.1 200 OK
    Content-Type: application/openmetrics-text; version=1.0.0; charset=utf-8

    # TYPE object gauge
    object 8.0


Meters
------

See https://github.com/prometheus/OpenMetrics/blob/main/specification/OpenMetrics.md#counter

monitoring/v1/metrics/meters::

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "event": {
            "result": {
                "count": 1234,
                "mean": 100,
                "rate1": 10.2,
                "rate5": 8.2,
                "rate15": 5.3
            }
        }
    }

It transforms to::

    HTTP/1.1 200 OK
    Content-Type: application/openmetrics-text; version=1.0.0; charset=utf-8

    # TYPE event_result_total counter
    event_result_total 1234.0


Histograms
----------

See https://github.com/prometheus/OpenMetrics/blob/main/specification/OpenMetrics.md#histogram

monitoring/v1/metrics/histograms::

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "event": {
            "count": 20,
            "min": 1.0,
            "max": 10.2,
            "mean": 8.76,
            "stddev": 4.56,
            "quantiles": {
                "0.05": 2.0,
                "0.25": 3.0,
                "0.5": 4.0,
                "0.75": 7.0,
                "0.95": 8.0
            },
            "distribution": [
                1,
                2,
                3,
                4,
                5,
                6,
                7,
                8,
                9,
                10
            ]
        }
    }

where event can be, for example, readPerson.

It transforms to::

    HTTP/1.1 200 OK
    Content-Type: application/openmetrics-text; version=1.0.0; charset=utf-8

    # TYPE event histogram
    event_bucket{le="0.05"} 2.0
    event_bucket{le="0.25"} 3.0
    event_bucket{le="0.5"} 4.0
    event_bucket{le="0.75"} 5.0
    event_bucket{le="0.95"} 8.0
    event_bucket{le="+Inf"} 9.0
    event_count 20

