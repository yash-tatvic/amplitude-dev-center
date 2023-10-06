Amplitude starts a session when the app is brought into the foreground or when an event is tracked in the background.

When the app enters the foreground, Amplitude tracks a session start, and starts a countdown based on `setMinTimeBetweenSessionsMillis()`. Amplitude extends the session and restarts the countdown any time it tracks a new event. If the countdown expires, Amplitude waits until the next event to track a session end event.

Amplitude doesn't set user properties on session events by default. To add these properties, use `identify()` and `setUserId()`. Amplitude aggregates the user property state and associates the user with events based on `device_id` or `user_id`.

If they would like to not sync user properties on these events we have an option of setting "$skip_user_properties_sync": true on the event, and this will stop us from syncing properties on that event in particular.

Due to how Amplitude manages sessions, there are certain scenarios where the SDK works expected but may appear as if events are missing or session tracking is inaccurate:

* If a user never returns to the app , there will be no corresponding session end event for the session start event
* If you disable session tracking and only track few events in the app, the tracked session length will be shorter than the time the user actually spends on the app
* If you modify user properties between the last event and the session end event, the session end event will reflect the updated user properties, potentially differing from other properties associated with events in the same session. To address this, you can utilize an enrichment plugin to set `$skip_user_properties_sync` to true on the session end event, preventing the backend from synchronizing properties for that specific event