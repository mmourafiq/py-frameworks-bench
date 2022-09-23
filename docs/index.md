---
layout: home
list_title: Archive
description: Python frameworks benchmarks
---

<script src="https://cdn.jsdelivr.net/npm/chart.js@3.2.1/dist/chart.min.js"></script>

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp and tornado are exceptions on the
moment).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

* Read about the benchmark: [The Methodic](methodic.md)
* Check complete results for the latest benchmark here: [Results (2022-09-23)](_posts/2022-09-23-results.md)

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

## Combined results

<canvas id="chart" style="margin-bottom: 2em"></canvas>
<script>
    var ctx = document.getElementById('chart').getContext('2d');
    var myChart = new Chart(ctx, {
        type: 'bar',
        data: {
            labels: ['sanic','starlette','fastapi','tornado','django',],
            datasets: [
                {
                    label: '# of requests',
                    data: ['455145','366525','250800','126675','47715',],
                    backgroundColor: [
                        '#4E79A7', '#A0CBE8', '#F28E2B', '#FFBE7D', '#59A14F', '#8CD17D', '#B6992D', '#F1CE63', '#499894', '#86BCB6', '#E15759', '#FF9D9A', '#79706E', '#BAB0AC', '#D37295', '#FABFD2', '#B07AA1', '#D4A6C8', '#9D7660', '#D7B5A6',
                    ]
                },
            ]
        }
    });
</script>

Sorted by sum of completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [sanic](https://pypi.org/project/sanic/) `22.6.2` | 455145 | 9.27 | 9.85 | 7.8
| [starlette](https://pypi.org/project/starlette/) `0.20.4` | 366525 | 10.01 | 17.52 | 12.85
| [fastapi](https://pypi.org/project/fastapi/) `0.85.0` | 250800 | 12.71 | 21.98 | 16.24
| [tornado](https://pypi.org/project/tornado/) `6.1` | 126675 | 23.52 | 23.68 | 23.55
| [django](https://pypi.org/project/django/) `4.1.1` | 47715 | 58.75 | 63.85 | 62.15


More details: [Results (2022-09-23)](_posts/2022-09-23-results.md)