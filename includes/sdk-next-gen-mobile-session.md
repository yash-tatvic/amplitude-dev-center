Amplitude starts a session when the app is brought into the foreground or when an event is tracked in background.

Amplitude tracks a session start event upon entering the foreground and starts a countdown set by `setMinTimeBetweenSessionsMillis()`. If a new event is tracked, Amplitude extends the session and restart the countdown with the same duration. If the countdown expires, Amplitude doesn't immediately track a session end event but  waits until the next event.

Amplitude SDKs don't set user properties to session events directly. Instead `identify()` and `setUserId()` update user properties from the SDK. The user property state is then aggregated on the backend and associated with events during ingestion based on `device_id` or `user_id`.

If they would like to not sync user properties on these events we have an option of setting "$skip_user_properties_sync": true on the event, and this will stop us from syncing properties on that event in particular.

Due to how Amplitude manages sessions, there are certain scenarios where the SDK works expected but may appear as if events are missing or session tracking is inaccurate:

* If a user never returns to the app , there will be no corresponding session end event for the session start event
* If you disable session tracking and only track few events in the app, the tracked session length will be shorter than the time the user actually spends on the app
* If you modify user properties between the last event and the session end event, the session end event will reflect the updated user properties, potentially differing from other properties associated with events in the same session. To address this, you can utilize an enrichment plugin to set `$skip_user_properties_sync` to true on the session end event, preventing the backend from synchronizing properties for that specific event