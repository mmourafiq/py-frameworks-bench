# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2022-09-23

[![benchmarks](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/benchmarks.yml)
[![tests](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml/badge.svg)](https://github.com/klen/py-frameworks-bench/actions/workflows/tests.yml)

----------

This is a simple benchmark for python async frameworks. Almost all of the
frameworks are ASGI-compatible (aiohttp and tornado are exceptions on the
moment).

The objective of the benchmark is not testing deployment (like uvicorn vs
hypercorn and etc) or database (ORM, drivers) but instead test the frameworks
itself. The benchmark checks request parsing (body, headers, formdata,
queries), routing, responses.

## Table of contents

* [The Methodic](#the-methodic)
* [The Results](#the-results-2022-09-23)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22sanic%22%2C%22starlette%22%2C%22fastapi%22%2C%22tornado%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B455145%2C366525%2C250800%2C126675%2C47715%5D%7D%5D%7D%7D' />

## The Methodic

The benchmark runs as a [Github Action](https://github.com/features/actions).
According to the [github
documentation](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
the hardware specification for the runs is:

* 2-core vCPU (Intel® Xeon® Platinum 8272CL (Cascade Lake), Intel® Xeon® 8171M 2.1GHz (Skylake))
* 7 GB of RAM memory
* 14 GB of SSD disk space
* OS Ubuntu 20.04

[ASGI](https://asgi.readthedocs.io/en/latest/) apps are running from docker using the gunicorn/uvicorn command:

    gunicorn -k uvicorn.workers.UvicornWorker -b 0.0.0.0:8080 app:app

Applications' source code can be found
[here](https://github.com/klen/py-frameworks-bench/tree/develop/frameworks).

Results received with WRK utility using the params:

    wrk -d15s -t4 -c64 [URL]

The benchmark has a three kind of tests:

1. "Simple" test: accept a request and return HTML response with custom dynamic
   header. The test simulates just a single HTML response.

2. "API" test: Check headers, parse path params, query string, JSON body and return a json
   response. The test simulates an JSON REST API.

3. "Upload" test: accept an uploaded file and store it on disk. The test
   simulates multipart formdata processing and work with files.


## The Results (2022-09-23)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [sanic](https://pypi.org/project/sanic/) `22.6.2` | 15018 | 4.90 | 5.23 | 4.23
| [starlette](https://pypi.org/project/starlette/) `0.20.4` | 13923 | 3.67 | 6.04 | 4.56
| [fastapi](https://pypi.org/project/fastapi/) `0.85.0` | 8870 | 5.58 | 9.71 | 7.19
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3433 | 18.59 | 18.75 | 18.64
| [django](https://pypi.org/project/django/) `4.1.1` | 1258 | 47.56 | 55.33 | 50.82


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [sanic](https://pypi.org/project/sanic/) `22.6.2` | 10436 | 7.28 | 7.68 | 6.10
| [starlette](https://pypi.org/project/starlette/) `0.20.4` | 8064 | 6.28 | 10.71 | 7.91
| [fastapi](https://pypi.org/project/fastapi/) `0.85.0` | 5751 | 8.58 | 15.20 | 11.09
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2845 | 22.50 | 22.62 | 22.49
| [django](https://pypi.org/project/django/) `4.1.1` | 1101 | 54.55 | 60.14 | 58.01

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [sanic](https://pypi.org/project/sanic/) `22.6.2` | 4889 | 15.62 | 16.63 | 13.06
| [starlette](https://pypi.org/project/starlette/) `0.20.4` | 2448 | 20.07 | 35.80 | 26.09
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2167 | 29.48 | 29.66 | 29.53
| [fastapi](https://pypi.org/project/fastapi/) `0.85.0` | 2099 | 23.96 | 41.03 | 30.44
| [django](https://pypi.org/project/django/) `4.1.1` | 822 | 74.15 | 76.09 | 77.62


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [sanic](https://pypi.org/project/sanic/) `22.6.2` | 455145 | 9.27 | 9.85 | 7.8
| [starlette](https://pypi.org/project/starlette/) `0.20.4` | 366525 | 10.01 | 17.52 | 12.85
| [fastapi](https://pypi.org/project/fastapi/) `0.85.0` | 250800 | 12.71 | 21.98 | 16.24
| [tornado](https://pypi.org/project/tornado/) `6.1` | 126675 | 23.52 | 23.68 | 23.55
| [django](https://pypi.org/project/django/) `4.1.1` | 47715 | 58.75 | 63.85 | 62.15

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)