# gaxios

[![npm version](https://img.shields.io/npm/v/gaxios.svg)](https://www.npmjs.org/package/gaxios)
[![codecov](https://codecov.io/gh/googleapis/gaxios/branch/master/graph/badge.svg)](https://codecov.io/gh/googleapis/gaxios)
[![Code Style: Google](https://img.shields.io/badge/code%20style-google-blueviolet.svg)](https://github.com/google/gts)

> An HTTP request client that provides an `axios` like interface over top of `node-fetch`.

## Install
```sh
$ npm install gaxios
```

## Example

```js
const {request} = require('gaxios');
const res = await request({
  url: 'https://www.googleapis.com/discovery/v1/apis/'
});
```

## Setting Defaults
Gaxios supports setting default properties both on the default instance, and on additional instances. This is often useful when making many requests to the same domain with the same base settings.  For example:

```js
const gaxios = require('gaxios');
gaxios.instance.defaults = {
  baseURL: 'https://example.com'
  headers: {
    Authorization: 'SOME_TOKEN'
  }
}
gaxios.request({url: '/data'}).then(...);
```

## Request Options

```js
{
  // The url to which the request should be sent.  Required.
  url: string,

  // The HTTP method to use for the request.  Defaults to `GET`.
  method: 'GET',

  // The base Url to use for the request. Prepended to the `url` property above.
  baseURL: 'https://example.com';

  // The HTTP methods to be sent with the request.
  headers: { 'some': 'header' },

  // The data to base64 encode and send in the body of the request.
  data: {
    some: 'data'
  },

  // The max size of the http response content in bytes allowed.
  // Defaults to `0`, which is the same as unset.
  maxContentLength: 2000,

  // The max number of HTTP redirects to follow.
  // Defaults to 100.
  maxRedirects: 100,

  // The querystring parameters that will be encoded using `qs` and
  // appended to the url
  params: {
    querystring: 'parameters'
  },

  // By default, we use the `querystring` package in node core to serialize
  // querystring parameters.  You can override that and provide your
  // own implementation.
  paramsSerializer: (params) => {
    return qs.stringify(params);
  },

  // The timeout for the HTTP request. Defaults to 0.
  timeout: 1000,

  // Optional method to override making the actual HTTP request. Useful
  // for writing tests.
  adapter?: (options) => {
    return {
      data: 'your data'
    }
  };

  // The expected return type of the request.  Options are:
  // json | stream | blob | arraybuffer | text
  // Defaults to `json`.
  responseType: 'json',

  // The node.js http agent to use for the request.
  agent: someHttpsAgent,

  // Custom function to determine if the response is valid based on the
  // status code.  Defaults to (>= 200 && < 300)
  validateStatus: (status: number) => true,

  // Configuration for retrying of requests.
  retryConfig: {
    // The number of times to retry the request.  Defaults to 3.
    retry?: number;

    // The number of retries already attempted.
    currentRetryAttempt?: number;

    // The amount of time to initially delay the retry.  Defaults to 100.
    retryDelay?: number;

    // The HTTP Methods that will be automatically retried.
    // Defaults to ['GET','PUT','HEAD','OPTIONS','DELETE']
    httpMethodsToRetry?: string[];

    // The HTTP response status codes that will automatically be retried.
    // Defaults to: [[100, 199], [429, 429], [500, 599]]
    statusCodesToRetry?: number[][];

    // Function to invoke when a retry attempt is made.
    onRetryAttempt?: (err: GaxiosError) => void;

    // Function to invoke which determines if you should retry
    shouldRetry?: (err: GaxiosError) => boolean;

    // When there is no response, the number of retries to attempt. Defaults to 2.
    noResponseRetries?: number;
  },

  // Enables default configuration for retries.
  retry: boolean,

  // Cancelling a request requires the `abort-controller` library.
  // See https://github.com/bitinn/node-fetch#request-cancellation-with-abortsignal
  signal?: AbortSignal
}
```

## License
[Apache-2.0](https://github.com/googleapis/gaxios/blob/master/LICENSE)
