---
title: Adjust Event Streaming
description: Stream Amplitude events to Adjust
---

Amplitude CDP's Adjust streaming integration enables you to forward your Amplitude events straight to [Adjust](https://www.adjust.com/) with just a few clicks.

!!!beta "This feature is in Beta"

    This feature is in Beta and is in active development. Contact the Amplitude support team if you would like to get access or support for this integration via integrations@amplitude.com.

## About Adjust

[Adjust](https://www.adjust.com/) is a business intelligence platform for mobile app marketers, combining attribution for advertising sources with advanced analytics and store statistics.

## Setup

### Prerequisites

To configure an Event Streaming integration from Amplitude to Adjust, you need the following information from Adjust:

- **Adjust App Token:** The App token is located in the Adjust.com dashboard. See the [Adjust documentation](https://help.adjust.com/en/article/server-to-server-events) for more help.
- **Adjust S2S Security Token:** To start sending data into Adjust, you have to get your Adjust S2S Security Token. See the [Adjust documentation](https://help.adjust.com/en/article/server-to-server-events) for more help.

### Create a new sync

1. In Amplitude, navigate to **Data Destinations**, then find **Adjust - Event Stream**.
2. In the Event Streaming section, click **Adjust**.
3. Enter a sync name, then click **Create Sync**.

### Enter credentials

1. Select your **Adjust App Token**.
2. Enter your **Adjust S2S Security Token**.

### Configure event forwarding

Under **Send Events**, make sure the toggle is enabled ("Events are sent to Adjust") if you want to stream events to Adjust. When enabled, events are automatically forwarded to Adjust when they're ingested in Amplitude. Events aren't sent on a schedule or on-demand using this integration.

1. In **Select and filter events** choose which events you want to send. Choose only the events you need in Adjust. Transformed events aren't supported.

    - For **Event Mapping**, select an event (one event per row) and then enter the corresponding Adjust event token.
    ![screenshot of an Event Mapping](../../assets/images/ADJUST-event-mapping.png)

2. In **Map properties to destination**: *Transformed user properties aren't supported*.

    - Select an Amplitude user property that corresponds to your Adjust ID, from the left dropdown.
    - (Recommended) Map an Amplitude user property to Adjust **User ID**.
    - It's recommended that you map Amplitude properties to as many of the Adjust IDs (iOS IDFA, Google Advertising ID, Amazon Fire Advertising ID, Huawei Open Advertising ID, Adjust Device ID (ADID), IDFA, Android ID).
    - To track Revenue events, map the following parameters:

        - **revenue**: Revenue event value in full currency units. Minimum 0.001.
        - **currency**: Revenue event [currency code](https://help.adjust.com/resources/lists/supported-currencies).
        - **environment**: Environment to post the data to (**sandbox** or **production**). If this parameter is not included, it will post to **production**.

3. (optional) In **Select additional properties**, select any more event and user properties you want to send to Adjust. If you don't select any properties here, Amplitude doesn't send any. Transformed event properties and transformed user properties aren't supported.

### Enable sync

When satisfied with your configuration, at the top of the page toggle the Status to "Enabled" and click Save.

## Example configuration setup

![screenshot of Adjust streaming config UI](../../assets/images/ADJUST-event-streaming-config.png)
