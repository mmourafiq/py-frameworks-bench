# Async Python Web Frameworks comparison

https://klen.github.io/py-frameworks-bench/
----------
#### Updated: 2022-09-25

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
* [The Results](#the-results-2022-09-25)
    * [Accept a request and return HTML response with a custom dynamic header](#html)
    * [Parse path params, query string, JSON body and return a json response](#api)
    * [Parse uploaded file, store it on disk and return a text response](#upload)
    * [Composite stats ](#composite)



<img src='https://quickchart.io/chart?width=800&height=400&c=%7Btype%3A%22bar%22%2Cdata%3A%7Blabels%3A%5B%22sanic%22%2C%22starlette%22%2C%22fastapi%22%2C%22tornado%22%2C%22django%22%5D%2Cdatasets%3A%5B%7Blabel%3A%22num%20of%20req%22%2Cdata%3A%5B454695%2C367830%2C252675%2C127485%2C47895%5D%7D%5D%7D%7D' />

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


## The Results (2022-09-25)

<h3 id="html"> Accept a request and return HTML response with a custom dynamic header</h3>
<details open>
<summary> The test simulates just a single HTML response. </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [sanic](https://pypi.org/project/sanic/) `22.6.2` | 15058 | 4.90 | 5.21 | 4.23
| [starlette](https://pypi.org/project/starlette/) `0.20.4` | 13857 | 3.75 | 6.04 | 4.59
| [fastapi](https://pypi.org/project/fastapi/) `0.85.0` | 8891 | 5.54 | 9.69 | 7.17
| [tornado](https://pypi.org/project/tornado/) `6.1` | 3416 | 18.69 | 18.85 | 18.74
| [django](https://pypi.org/project/django/) `4.1.1` | 1237 | 48.30 | 54.43 | 51.65


</details>

<h3 id="api"> Parse path params, query string, JSON body and return a json response</h3>
<details open>
<summary> The test simulates a simple JSON REST API endpoint.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [sanic](https://pypi.org/project/sanic/) `22.6.2` | 10307 | 7.32 | 7.76 | 6.17
| [starlette](https://pypi.org/project/starlette/) `0.20.4` | 8199 | 5.98 | 10.46 | 7.77
| [fastapi](https://pypi.org/project/fastapi/) `0.85.0` | 5843 | 8.42 | 15.03 | 10.92
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2901 | 22.00 | 22.13 | 22.06
| [django](https://pypi.org/project/django/) `4.1.1` | 1123 | 53.83 | 56.75 | 56.95

</details>

<h3 id="upload"> Parse uploaded file, store it on disk and return a text response</h3>
<details open>
<summary> The test simulates multipart formdata processing and work with files.  </summary>

Sorted by max req/s

| Framework | Requests/sec | Latency 50% (ms) | Latency 75% (ms) | Latency Avg (ms) |
| --------- | -----------: | ---------------: | ---------------: | ---------------: |
| [sanic](https://pypi.org/project/sanic/) `22.6.2` | 4948 | 15.55 | 16.23 | 12.90
| [starlette](https://pypi.org/project/starlette/) `0.20.4` | 2466 | 20.26 | 34.71 | 25.90
| [tornado](https://pypi.org/project/tornado/) `6.1` | 2182 | 29.30 | 29.40 | 29.32
| [fastapi](https://pypi.org/project/fastapi/) `0.85.0` | 2111 | 23.49 | 40.70 | 30.25
| [django](https://pypi.org/project/django/) `4.1.1` | 833 | 73.02 | 75.51 | 76.62


</details>

<h3 id="composite"> Composite stats </h3>
<details open>
<summary> Combined benchmarks results</summary>

Sorted by completed requests

| Framework | Requests completed | Avg Latency 50% (ms) | Avg Latency 75% (ms) | Avg Latency (ms) |
| --------- | -----------------: | -------------------: | -------------------: | ---------------: |
| [sanic](https://pypi.org/project/sanic/) `22.6.2` | 454695 | 9.26 | 9.73 | 7.77
| [starlette](https://pypi.org/project/starlette/) `0.20.4` | 367830 | 10.0 | 17.07 | 12.75
| [fastapi](https://pypi.org/project/fastapi/) `0.85.0` | 252675 | 12.48 | 21.81 | 16.11
| [tornado](https://pypi.org/project/tornado/) `6.1` | 127485 | 23.33 | 23.46 | 23.37
| [django](https://pypi.org/project/django/) `4.1.1` | 47895 | 58.38 | 62.23 | 61.74

</details>

## Conclusion

Nothing here, just some measures for you.

## License

Licensed under a MIT license (See LICENSE file)