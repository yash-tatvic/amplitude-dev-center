---
title: Session Replay Standalone SDK
---
 
This article covers the installation of Session Replay using the standalone SDK. If you use a provider other than Amplitude for in-product analytics, choose this option. If your site is already instrumented with Amplitude, use the [Session Replay Browser SDK Plugin](/session-replay/sdks/plugin).

!!! info "Session Replay and performance"
    Amplitude built Session Replay to minimize impact on the performance of web pages on which it's installed by:

    - Asynchronously capturing and processing replay data, to avoid blocking the main user interface thread.
    - Using batching and lightweight compression to reduce the number of network connections and bandwidth.
    - Optimizing DOM processing.

Session Replay captures changes to a page's Document Object Model (DOM), then replays these changes to build a video-like replay. For example, at the start of a session, Session Replay captures a full snapshot of the page's DOM. As the user interacts with the page, Session Replay captures each change to the DOM as a diff. When you watch the replay of a session, Session Replay applies each diff back to the original DOM in sequential order, to construct the replay. Session replays have no maximum length.

## Before you begin

Use the latest version of the Session Replay standalone SDK above version 0.5.0. For more information, see the [change log](https://github.com/amplitude/Amplitude-TypeScript/blob/v1.x/packages/session-replay-browser/CHANGELOG.md) on GitHub.

Session Replay requires that:

1. Your application is web-based.
2. You track sessions with a timestamp, which you can pass to the SDK. You inform the SDK whenever a session timestamp changes.
3. You can provide a device ID to the SDK.

### Supported browsers

Session replay supports the same set of browsers as Amplitude's SDKs. For more information, see [Browser Compatibility](https://help.amplitude.com/hc/en-us/articles/360024709711).

## Quickstart

Install the standalone SDK with yarn or npm.

=== "npm"

    ```bash
    npm install @amplitude/session-replay-browser --save
    ```

=== "yarn"

    ```bash
    yarn add @amplitude/session-replay-browser
    ```

Configure your application code.

1. Call `sessionReplay.init` to begin collecting replays. Pass the API key, session identifier, and device identifier.
2. When the session identifier changes, pass the new value to Amplitude with `sessionReplay.setSessionId`.
3. Collect Session Replay properties to send with other event properties with `sessionReplay.getSessionReplayProperties`

```javascript
import * as sessionReplay from "@amplitude/session-replay-browser";
import 3rdPartyAnalytics from 'example'

const AMPLITUDE_API_KEY = "key"

// Configure the SDK and begin collecting replays
await sessionReplay.init(AMPLITUDE_API_KEY, {
  deviceId: "<string>",
  sessionId: "<number>",
  optOut: "<boolean>",
  sampleRate: "<number>"
}).promise;

// Call whenever the session id changes
sessionReplay.setSessionId(sessionId);

// When you send events to Amplitude, call this event to get
// the most up to date session replay properties for the event
const sessionReplayProperties = sessionReplay.getSessionReplayProperties();
3rdPartyAnalytics.track('event', {...eventProperties, ...sessionReplayProperties})
```

## Configuration

Pass the following configuration options when you initialize the Session Replay SDK.

| Name              | Type      | Required | Default         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ----------------- | --------- | -------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `deviceId`        | `string`  | Yes      | `undefined`     | Sets an identifier for the device running your application.                                                                                                                                                                                                                                                                                                                                                                                 |
| `sessionId`       | `number`  | Yes      | `undefined`     | Sets an identifier for the users current session. The value must be in milliseconds since epoch (Unix Timestamp).                                                                                                                                                                                                                                                                                                                           |
| `sampleRate`      | `number`  | No       | `0`             | Use this option to control how many sessions to select for replay collection. <br></br>The number should be a decimal between 0 and 1, for example `0.4`, representing the fraction of sessions to have randomly selected for replay collection. Over a large number of sessions, `0.4` would select `40%` of those sessions. |
| `optOut`          | `boolean` | No       | `false`         | Sets permission to collect replays for sessions. Setting a value of true prevents Amplitude from collecting session replays.                                                                                                                                                                                                                                                                                                                |
| `flushMaxRetries` | `number`  | No       | `5`             | Sets the maximum number of retries for failed upload attempts. This is only applicable to errors that Amplitude can retry.                                                                                                                                                                                                                                                                                                                                 |
| `logLevel`        | `number`  | No       | `LogLevel.Warn` | `LogLevel.None` or `LogLevel.Error` or `LogLevel.Warn` or `LogLevel.Verbose` or `LogLevel.Debug`. Sets the log level.                                                                                                                                                                                                                                                                                                                       |
| `loggerProvider`  | `Logger`  | No       | `Logger`        | Sets a custom `loggerProvider` class from the Logger to emit log messages to desired destination.                                                                                                                                                                                                                                                                                                                                             |
| `serverZone`      | `string`  | No       | `US`            | EU or US. Sets the Amplitude server zone. Set this to EU for Amplitude projects created in EU data center.                                                                                                                                                                                                                                                                                                                                  |

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

To set the `sampleRate` consider the monthly quota on your Session Replay plan. For example, if your monthly quota is 2,500,000 sessions, and you average 3,000,000 monthly sessions, your quota is 83% of your average sessions. In this case, to ensure sampling lasts through the month, set `sampleRate` to `.83` or lower.

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
import * as sessionReplay from "@amplitude/session-replay-browser";
import 3rdPartyAnalytics from 'example'

const AMPLITUDE_API_KEY = <...>
sessionReplay.init(AMPLITUDE_API_KEY, {
  deviceId: <string>,
  sessionId: <number>,
  optOut: <boolean>,
  sampleRate: <number>
})

if (nonEUCountryFlagEnabled) {
  const sessionReplayProperties = sessionReplay.getSessionReplayProperties();
  3rdPartyAnalytics.track('event', {...eventProperties, ...sessionReplayProperties})
}
```

## Data retention, deletion, and privacy

Session replay uses existing Amplitude tools and APIs to handle privacy and deletion requests.

### Retention period

By default, Amplitude retains raw replay data for 90 days from the date of ingestion. If your use case requires a more strict policy, Amplitude can set the value to 30 days by request.

Changes to the retention period impact replays ingested after the change. Sessions captured and ingested before a retention period change retain the previous retention period.

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

### Bot filter

Session Replay uses the same [block filter](https://help.amplitude.com/hc/en-us/articles/14884769332507-Block-bot-web-traffic) available in the Amplitude app. Session Replay doesn't block traffic based on event or user properties.

## Session Replay storage

Session Replay doesn't set cookies on the user's browser. Instead, it relies on a browser storage option called [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API). This option enables continuous replay collection during a session in which the user navigates browser tabs or closes and reopens a tab. The SDK cleans up the data it stores in IndexedDB and shouldn't impact the user's disk space.

## Known limitations

Keep the following limitations in mind as you implement Session Replay:

- Session Replay doesn't stitch together replays from a single user across multiple projects. For example:
  
    - You instrument your marketing site and web application as separate Amplitude projects with Session Replay enabled in each.
    - A known user begins on the marketing site, and logs in to the web application.
    - Amplitude captures both sessions.
    - The replay for each session is available for view in the host project.

- The User Sessions chart doesn't show session replays if your organization uses a custom session definition.
- Session Replay can't capture the following HTML elements:

    - Canvas
    - WebGL
    - `<object>` tags including plugins like Flash, Silverlight, or Java. Session replay supports `<object type="image">`
    - Lottie animations
    - `<iframe>` elements from a different origin
    - Assets that require authentication, like fonts, CSS, or images

<!-- 
## Go-live checklist

TBD 
-->

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
- Page instrumentation. If Session Replay isn't implemented on all pages a user visits, their session may not capture properly.

## Use Session Replay with Segment analytics

Session Replay supports other analytics providers. Follow the information below to add Session Replay to an existing Segment-instrumented site.

- [Amplitude (Actions)](#amplitude-actions-destination)
- [Amplitude Classic (Cloud-mode)](#amplitude-classic-destination-cloud-mode)
- [Amplitude Classic (Device-mode)](#amplitude-classic-destination-device-mode)

### Amplitude (Actions) destination

Amplitude (Actions) tracks sessions automatically. When a user starts a new session, Amplitude sets an `analytics_session_id` cookie on the users browser. Configure your implementation to listen for changes in value to `analytics_session_id`, which you can do with Segment's [Source Middleware](https://segment.com/docs/connections/sources/catalog/libraries/website/javascript/middleware/).

This code snippet shows how to configure Session Replay with Segment's Amplitude (Actions) integration, and update the session ID when `analytics_session_id` changes.

```javascript
import * as sessionReplay from "@amplitude/session-replay-browser";
import { AnalyticsBrowser } from "@segment/analytics-next";

const segmentAnalytics = AnalyticsBrowser.load({
  writeKey: "segment-key",
});

const AMPLITUDE_API_KEY = 'api-key' // must match that saved with Segment

const getStoredSessionId = () => {
 return cookie.get("amp_session_id") || 0;
}

const user = await segmentAnalytics.user();
const storedSessionId = getStoredSessionId();
await sessionReplay.init(AMPLITUDE_API_KEY, { 
  sessionId: storedSessionId,
  deviceId: user.anonymousId()
}).promise;

// Add middleware to check if the session id has changed, 
// and update the session replay instance
segmentAnalytics.addSourceMiddleware(({ payload, next, integrations }) => {
  const storedSessionId = getStoredSessionId();
  const nextSessionId = payload.obj.integrations['Actions Amplitude'].session_id || 0

  if (storedSessionId < nextSessionId) {
    cookie.set("amp_session_id", nextSessionId);
    sessionReplay.setSessionId(nextSessionId);
  }
  next(payload);
});

// Add middleware to always add session replay properties to track calls
SegmentAnalytics.addSourceMiddleware(({ payload, next, integrations }) => {
  const sessionReplayProperties = sessionReplay.getSessionReplayProperties();
  if (payload.type() === "track") {
    payload.obj.properties = {
      ...payload.obj.properties,
      ...sessionReplayProperties,
    };
  }
  
  next(payload);
});
```

### Amplitude Classic destination (Device-mode)

This version of the Amplitude destination installs the Amplitude JavaScript SDK (5.2.2) on the client, and sends events directly to `api.amplitude.com`.

The Device-mode integration tracks sessions by default, since it includes the amplitude-js SDK. The included SDK version (5.2.2) doesn't include an event for session changes. As a result, use Segment's middleware to update Session Replay when the session ID changes.

```javascript
import * as sessionReplay from "@amplitude/session-replay-browser";
import { AnalyticsBrowser } from "@segment/analytics-next";

const segmentAnalytics = AnalyticsBrowser.load({
  writeKey: "segment-key",
});

const AMPLITUDE_API_KEY = 'api-key' // must match that saved with Segment

const getAmpSessionId = () => {
  const sessionId = window.amplitude.getInstance().getSessionId();
  cookie.set("amp_session_id", sessionId);
  return sessionId;
};

// Wait for the amplitude-js SDK to initialize,
// then initialize session replay with the correct device id and session id
window.amplitude.getInstance().onInit(() => {
  const sessionId = getAmpSessionId();
  sessionReplay.init(AMPLITUDE_API_KEY, {
    deviceId: window.amplitude.getInstance().options.deviceId,
    sessionId: getAmpSessionId(),
  });
});


// Add middleware to check if the session id has changed, 
// and update the session replay instance
SegmentAnalytics.addSourceMiddleware(({ payload, next, integrations }) => {
  const nextSessionId = window.amplitude.getInstance().getSessionId();
  const storedSessionId = cookie.get("amp_session_id") || 0;

  if (storedSessionId < nextSessionId) {
    cookie.set("amp_session_id", nextSessionId);
    sessionReplay.setSessionId(nextSessionId);
  }
  next(payload);
});

// Add middleware to always add session replay properties to track calls
SegmentAnalytics.addSourceMiddleware(({ payload, next, integrations }) => {
  const sessionReplayProperties = sessionReplay.getSessionReplayProperties();
  if (payload.type() === "track") {
    payload.obj.properties = {
      ...payload.obj.properties,
      ...sessionReplayProperties,
    };
  }
  
  next(payload);
});
```

### Amplitude Classic destination (Cloud-mode)

This version of the Amplitude destination sends events to Segment's backend, which forwards them to Amplitude. The Cloud-mode destination doesn't track sessions by default. To overcome this, use the Browser SDK as a shell to manage sessions, and use Session Replay as a plugin, as shown below.

```javascript
import { sessionReplayPlugin } from "@amplitude/plugin-session-replay-browser";
import { AnalyticsBrowser } from "@segment/analytics-next";

const segmentAnalytics = AnalyticsBrowser.load({
  writeKey: "segment-key",
});

// A plugin must be added so that events sent through Segment will have
// session replay properties and the correct session id
const segmentPlugin: () => Plugin = () => {
  return {
    name: 'segment',
    type: 'enrichment',
    execute: async (event) => {
      const properties = event.event_properties || {};

      segmentAnalytics.track(event.event_type, properties, {
        integrations: {
          Amplitude: {
            session_id: amplitude.getSessionId(),
          },
        },
      });
      return Promise.resolve(event);
    }
  }
}

const AMPLITUDE_API_KEY = 'api-key' // must match that saved with Segment

// Add the session replay plugin first, then the segment plugin
await amplitude.add(sessionReplayPlugin()).promise;
await amplitude.add(segmentPlugin()).promise;

const user = await segmentAnalytics.user();
await amplitude.init(AMPLITUDE_API_KEY, { 
  instanceName: 'session-replay',
  sessionTimeout: Number.MAX_SAFE_INTEGER,
  defaultTracking: false,
  deviceId: user.anonymousId()
}).promise;
amplitude.remove('amplitude');

// Events must be tracked through the shell Browser SDK to properly attach
// session replay properties
amplitude.track('event name')
```
