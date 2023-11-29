---
title: Session Replay Browser SDK Plugin
---

This article covers the installation of Session Replay using the Browser SDK plugin. If your site is already instrumented with Amplitude, use this option. If you use a provider other than Amplitude for in-product analytics, choose the [standalone implementation](/session-replay/sdks/standalone).

!!! info "Session Replay and performance"
    Amplitude built Session Replay to minimize impact on the performance of web pages on which it's installed by:

    - Asynchronously capturing and processing replay data, to avoid blocking the main user interface thread.
    - Using batching and lightweight compression to reduce the number of network connections and bandwidth.
    - Optimizing libraries for performance.

## Before you begin

Use the latest version of the Session Replay Plugin above version 0.8.1. For more information, see the [change log](https://github.com/amplitude/Amplitude-TypeScript/blob/v1.x/packages/plugin-session-replay-browser/CHANGELOG.md) on GitHub.

Session Replay requires that:

1. Your application is JavaScript-based.
2. You track sessions with a timestamp, which you can pass to the SDK. You inform the SDK whenever a session timestamp changes.
3. You can provide a device ID to the SDK.

## Quickstart

Install the plugin with yarn or npm.

=== "npm"

    ```bash
    npm install @amplitude/plugin-session-replay-browser --save
    ```

=== "yarn"

    ```bash
    yarn add @amplitude/plugin-session-replay-browser
    ```

Configure your application code.

```js
import * as amplitude from '@amplitude/analytics-browser';
import { sessionReplayPlugin } from '@amplitude/plugin-session-replay-browser';

// Your existing initialization logic with Browser SDK
amplitude.init(API_KEY);

// Create and Install Session Replay Plugin
const sessionReplayTracking = sessionReplayPlugin();
amplitude.add(sessionReplayTracking);
```

You can also add the code directly to the `<head>` of your site.

```html
<script src="https://cdn.amplitude.com/libs/analytics-browser-2.1.3-min.js.gz"></script>
<script src="https://cdn.amplitude.com/libs/plugin-session-replay-browser-0.6.13-min.js.gz"></script>
<script>
window.amplitude.init(API_KEY)
const sessionReplayTracking = window.sessionReplay.plugin();
window.amplitude.add(sessionReplayTracking);
</script>
```

## Configuration

Pass the following option when you initialize the Session Replay plugin:

| Name              | Type      | Required | Default         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ----------------- | --------- | -------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sampleRate`      | `number`  | No       | `0`             | Use this option to control how many sessions to select for replay collection. <br></br>The number should be a decimal between 0 and 1, for example `0.4`, representing the fraction of sessions to have randomly selected for replay collection. Over a large number of sessions, `0.4` would select `40%` of those sessions. |

### Track default session events

Session replay requires that you configure default session event tracking. This ensures that Session Replay captures Session Start and Session End events. If you didn't capture these events before you implement Session Replay, expect an increase in event volume. For more information about session tracking, see [Browser SDK 2.0 | Tracking Sessions](/data/sdks/browser-2/#tracking-sessions).

=== "SDK Configuration"

    Use the Browser SDK configuration to implicitly enable session tracking.

    ```js 
    amplitude.init(API_KEY, USER, {
        defaultTracking: {
            sessions: true
        }
    });
    ```

=== "Plugin Configuration"

    Disable all default tracking by the Browser SDK. In this case, the plugin enables default session tracking.

    ```js
    amplitude.init(API_KEY, USER, {
        defaultTracking: false
    });
    ```

### Mask on-screen data

The Session Replay SDK offers three ways to mask user input, text, and other HTML elements.

| Element           | Description                                                                                                                                                                                                                                               |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<input>`         | Session Replay masks all text input fields by default. When a users enters text into an input field, Session Replay captures asterisks in place of text. To *unmask* a text input, add the class `.amp-unmask`. For example: `<input class="amp-unmask">`. |
| text              | To mask text within non-input elements, add the class `.amp-mask`. For example, `<p class="amp-mask">Text</p>`. When masked, Session Replay captures masked text as a series of asterisks.                                                                 |
| non-text elements | To block a non-text element, add the class `.amp-block`. For example, `<div class="amp-block"></div>`. Session Replay replaces blocked elements with a placeholder of the same dimensions.                                                                |

### User opt-out

Session Replay provides an option for opt-out configuration. This prevents Amplitude from collecting session replays when passed as part of initialization. For example:

```js
// Pass a boolean value to indicate a users opt-out status
await sessionReplay.init(AMPLITUDE_API_KEY, {
  optOut: true,
}).promise;
```

### EU data residency

Session Replay is available to Amplitude Customers who use the EU data center. Set the `serverZone` configuration option to `EU` during initialization. For example:

```js
// For European users, set the serverZone to "EU" 
await sessionReplay.init(AMPLITUDE_API_KEY, {
  serverZone: "EU",
}).promise;
```

### Sampling rate

 By default, Session Replay captures 0% of sessions for replay. Use the `sampleRate` configuration option to set the percentage of total sessions that Session Replay captures. For example:

```js
// This configuration samples 40% of all sessions
await sessionReplay.init(AMPLITUDE_API_KEY, {
  sampleRate: 0.4
}).promise;
```

To set the `sampleRate` consider the monthly quota on your Session Replay plan. For example, if your monthly quota is 2,500,000 sessions, and you average 3,000,000 monthly visitors, your quota is 83% of your average visitors. In this case, to ensure sampling lasts through the month, set `sampleRate` to `.83` or lower.

Keep the following in mind as you consider your sample rate:

- When you reach your monthly session quote, Amplitude stops capturing sessions for replay.
- Session quotas reset on the first of every month.
- Use sample rate to distribute your session quota over the course of a month, rather than using your full quote at the beginning of the month.

### Disable replay collection

Once enabled, Session Replay runs on your site until either:

- The user leaves your site
- You call `sessionReplay.shutdown()`

Call `sessionReplay.shutdown()` before a user navigates to a restricted area of your site to disable replay collection while the user is in that area. 

Call `sessionReplay.init(API_KEY, {...options})` to re-enable replay collection when the return to an unrestricted area of your site.

You can also use a feature flag product like Amplitude Experiment to create logic that enables or disables replay collection based on criteria like location. For example, you can create a feature flag that targets a specific user group, and add that to your initialization logic:

```javascript
import { sessionReplayPlugin } from "@amplitude/plugin-session-replay-browser";

// Your existing initialization logic with Browser SDK
amplitude.init(API_KEY);

if (nonEUCountryFlagEnabled) {
  // Create and Install Session Replay Plugin
  const sessionReplayTracking = sessionReplayPlugin({
    sampleRate: 0.5,
  });
  amplitude.add(sessionReplayTracking);
}
```

## Data retention, deletion, and privacy

Session replay uses existing Amplitude tools and APIs to handle privacy and deletion requests.

### Retention period

By default, Amplitude retains raw replay data for 90 days from the date of ingestion. If your use case requires a more strict policy, Amplitude can set the value to 30 days by request.

### DSAR API

The Amplitude [DSAR API](/analytics/apis/ccpa-dsar-api/) returns metadata about session replays, but not the raw replay data. All events that are part of a session replay include a `[Amplitude] Session Replay ID` event property. This event provides information about the sessions collected for replay for the user, and includes all metadata collected with each event.

```json
{
  "amplitude_id": 123456789,
  "app": 12345,
  "event_time": "2020-02-15 01:00:00.123456",
  "event_type": "first_event",
  "server_upload_time": "2020-02-18 01:00:00.234567",
  "device_id": "your device id",
  "user_properties": { ... }
  "event_properties": {
    "[Amplitude] Session Replay ID": "cb6ade06-cbdf-4e0c-8156-32c2863379d6/1699922971244"
  }
  "session_id": 1699922971244,
}
```

### Data deletion

Session Replay uses Amplitude's [User Privacy API](/analytics/apis/user-privacy-api/) to handle deletion requests. Successful deletion requests remove all session replays for the specified user.

When you delete the Amplitude project on which you use Session Replay, Amplitude deletes that replay data.

## Session Replay storage

Session Replay doesn't set cookies on the user's browser. Instead, it relies on a browser storage option called [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API). This option enables continuous replay collection during a session in which the user navigates browser tabs or closes and reopens a tab. The plugin cleans up the data it stores in IndexedDB and shouldn't impact the user's disk space.

## Known limitations

Keep the following limitations in mind as you implement Session Replay:

- The User Sessions chart doesn't show session replays if your organization uses a custom session definition.
- Session Replay can't capture the following HTML elements:

    - Canvas
    - WebGL
    - `<object>` tags, other than `<object type="image">`
    - Lottie animations
    - `<iframe>` elements from a different origin
    - Assets that require authentication, like fonts, CSS, or images

### Multiple Amplitude instances

Session Replay supports attaching to a single instance of the Amplitude SDK. If you have more than one instance instrumented in your application, make sure to start Session Replay on the instance that most relates to your project.

```html
<script>
  const sessionReplayTracking = window.sessionReplay.plugin();
  const instance = window.amplitude.createInstance();
  instance.add(sessionReplayTracking);
  instance.init(API_KEY);
<script>

```

## Troubleshooting

### Session replays don't appear in Amplitude 

Session replays may not appear in Amplitude due to:

- Content security policy
- Blocked JavaScript
- Sampling

#### Content security policy

When you add the Session Replay script to your site, visit a page on which the Session Replay SDK is running, and open your browser's developer tools.

Check for any error messages in the JavaScript console that contain the text `Content Security Policy`. For example, `Refused to connect to 'https://api-secure.amplitude.com/sessions/track' because it violates the document's Content Security Policy`.

To resolve this error, update your site's content security policy to allow connection to Amplitude's APIs.

#### Blocked JavaScript

Browser extensions or network security policy may block the Session Replay SDK. Check your browser's developer tools to see if requests fail, and if so, add an exception for the blocked domains.

#### Sampling

As mentioned above, the default `sampleRate` for Session Replay is `0`. Update the rate to a higher number. For more information see, [Sampling rate](#sampling-rate).

#### Some sessions don't include the Session Replay ID property

Session replay doesn't require that all events in a session have the `[Amplitude] Session Replay ID` property, only that one event in the session has it. Reasons why `[Amplitude] Session Replay ID`  may not be present in an event include:

- If you instrument an event with a source different from the source you connect to Session Replay. For example, your application may send events from a backend source, rather than the Browser SDK.
- If events fire when the user isn't focused on the page. Session Replay pauses the SDK when user focus leaves the page. Amplitude events may still send through your provider, but `getSessionReplayProperties()` doesn't return the `[Amplitude] Session Replay ID` property. This is because Session Replay hasn't begun the capture, since the user hasn't interacted with the page. This should lead to a decrease in the amount of inactivity that a replay captures.

### Session Replay processing errors

In general, replays should be available within minutes of ingestion. Delays or errors may be the result of one or more of the following:

- Mismatching API keys or Device IDs. This can happen if Session Replay and standard event instrumentation use different API keys or Device IDs.
- Session Replay references the wrong project.
- Short sessions. If a users bounces within a few seconds of initialization, the SDK may not have time to upload replay data.
- Page instrumentation. If Session Replay isn't implemented on all pages a user visits, their session may not capture properly
