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
