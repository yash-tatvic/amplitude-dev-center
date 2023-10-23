---
title: Experiment PHP SDK
description: Official documentation for Amplitude Experiment's server-side PHP SDK implementation.
icon: simple/php
---

Official documentation for Amplitude Experiment's server-side PHP SDK implementation.

![Packagist](https://img.shields.io/packagist/v/amplitude/experiment-php-server.svg)

!!!info "SDK Resources"
     [:material-github: GitHub](https://github.com/amplitude/experiment-php-server) · [:material-code-tags-check: Releases](https://github.com/amplitude/experiment-php-server/releases)

!!!note
    Currently, only [remote evaluation](../general/evaluation/remote-evaluation.md) is supported. Support for [local evaluation](../general/evaluation/local-evaluation.md) will be available for future releases.

## Remote evaluation

Implements fetching variants for a user via [remote evaluation](../general/evaluation/remote-evaluation.md).

### Install

!!!info "PHP version compatibility"

    The PHP Server SDK works with PHP 7.4+.

Install the PHP Server SDK with composer.

=== "php"

    ```bash
    composer require amplitude/experiment-php-server
    ```

!!!tip "Quick Start"

    1. [Initialize the experiment client](#initialize)
    2. [Fetch variants for the user](#fetch)
    3. [Access a flag's variant](#fetch)

    ```php
    <?php
    // (1) Initialize the experiment client
    $experiment = new \AmplitudeExperiment\Experiment();
    $client = $experiment->initializeRemote('<DEPLOYMENT_KEY>');

    // (2) Fetch variants for a user
    $user = (new \AmplitudeExperiment\UserBuilder())
        ->deviceId('abcdefg')
        ->userId('user@company.com')
        ->userProperties(['premium' => true]) 
        ->build();
    $variants = $client->fetch($user)>wait();

    // (3) Access a flag's variant
    $variant = $variants['FLAG_KEY']
    if ($variant) {
        if ($variant->value == 'on') {
            // Flag is on
        } else {
            // Flag is off
        }
    }
    ```

    **Not getting the expected variant result for your flag?** Make sure your flag [is activated](../guides/getting-started/create-a-flag.md#activate-the-flag), has a [deployment set](../guides/getting-started/create-a-flag.md#add-a-deployment), and has [users allocated](../guides/getting-started/create-a-flag.md#configure-targeting-rules).

### Initialize

The SDK client should be initialized in your server on startup. The [deployment key](../general/data-model.md#deployments) argument passed into the `apiKey` parameter must live within the same project that you are sending analytics events to.

```php
<?php
initializeRemote(string $apiKey, ?RemoteEvaluationConfig $config): RemoteEvaluationClient
```

| Parameter | Requirement | Description |
| --- | --- | --- |
| `apiKey` | required | The [deployment key](../general/data-model.md#deployments) which authorizes fetch requests and determines which flags should be evaluated for the user. |
| `config` | optional | The client [configuration](#configuration) used to customize SDK client behavior. |

!!!info "Timeout & Retry Configuration"
    Configure the timeout and retry options to best fit your performance requirements.
    ```php
    <?php
    $experiment = new \AmplitudeExperiment\Experiment();
    $config = (new \AmplitudeExperiment\Remote\RemoteEvaluationConfigBuilder())
                ->fetchTimeoutMillis(500)
                ->fetchRetries(1)
                ->fetchRetryBackoffMinMillis(0)
                ->fetchRetryTimeoutMillis(500)
                ->build();
    $client = $experiment->initializeRemote('<DEPLOYMENT_KEY>', $config);
    ```

#### Configuration

You can configure the SDK client on initialization.

???config "Configuration Options"
    | <div class="big-column">Name</div>  | Description | Default Value |
    | --- | --- | --- |
    | `debug` | Enable additional debug logging. | `false` |
    | `serverUrl` | The host to fetch variants from. | `https://api.lab.amplitude.com` |
    | `fetchTimeoutMillis` | The timeout for fetching variants in milliseconds. This timeout only applies to the initial request, not subsequent retries | `10000` |
    | `fetchRetries` | The number of retries to attempt if a request to fetch variants fails. | `8` |
    | `fetchRetryBackoffMinMillis` | The minimum (initial) backoff after a request to fetch variants fails. This delay is scaled by the `fetchRetryBackoffScalar` | `500` |
    | `fetchRetryBackoffMaxMillis` | The maximum backoff between retries. If the scaled backoff becomes greater than the max, the max is used for all subsequent requests | `10000` |
    | `fetchRetryBackoffScalar` | Scales the minimum backoff exponentially. | `1.5` |
    | `fetchRetryTimeoutMillis` | The request timeout for retrying variant fetches. | `10000` |

!!!info "EU Data Center"
    If you're using Amplitude's EU data center, configure the `serverUrl` option on initialization to `https://api.lab.eu.amplitude.com`

### Fetch

Fetches variants for a [user](../general/data-model.md#users) and returns the results. This function [remote evaluates](../general/evaluation/remote-evaluation.md) the user for flags associated with the deployment used to initialize the SDK client.

```php
<?php
fetch(User $user): PromiseInterface
// Upon resolution of the promise, an array of variants is returned on success, an empty array is returned on failure
```

| Parameter  | Requirement | Description |
| --- | --- | --- |
| `user` | required | The [user](../general/data-model.md#users) to remote fetch variants for. |

```php
<?php
$user = (new \AmplitudeExperiment\UserBuilder())
    ->deviceId('abcdefg')
    ->userId('user@company.com')
    ->userProperties(['premium' => true])
    ->build();
$variants = $client.fetch(user)->wait();
```

After fetching variants for a user, you may to access the variant for a specific flag.

```php
<?php
$variant = $variants['FLAG-KEY']
if ($variant) {
    if ($variant->value == 'on') {
        // Flag is on
    } else {
        // Flag is off
    }
}
```
