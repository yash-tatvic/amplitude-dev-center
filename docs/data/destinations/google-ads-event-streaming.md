---
title: Google Ads Event Streaming
description: Amplitude Data's Google Ads Event streaming integration enables you to stream your Amplitude event data straight to Google Ads with just a few clicks.
status: new
---

Amplitude Data's Google Ads integration enables you to stream your Amplitude event data straight to Google Ads with just a few clicks.

## Considerations

- You must enable this integration in each Amplitude project you want to use it in.
- Amplitude sends custom events using Amplitude event_type as event name.

## Setup

### Prerequisites

To set up event streaming to Google Ads, you need the following:

- A `Google Ads Customer ID`
- A `Google Ads Conversion Action ID`
- A `Google Cloud Service Account`

### Amplitude setup

1. In Amplitude Data, click **Catalog** and select the **Destinations** tab.
2. In the Event Streaming section, click **Google Ads**.
3. Enter a sync name, then click **Create Sync**.

After you create the destination, you must configure the settings.

#### Configure settings

1. On the **Settings** tab, click **Edit**.
2. Enter your [**Google Ads Customer ID**](https://support.google.com/google-ads/answer/1704344?hl=en).
3. Enter your [**Google Ads Conversion Action ID**](https://support.google.com/google-ads/thread/105330243?hl=en&sjid=5504033552721490234-EU).
4. Enter your [**Base64 encoded Google Cloud Service Account**](https://developers.google.com/google-ads/api/docs/oauth/service-accounts).
5. Configure **Send Events** to send events ingested by Amplitude to Google Ads.
      1. To send events, toggle **Send Events** to **Enabled**.
      2. Expand the **Select and filter events** panel, and select which events to send. Amplitude recommends that you send only the events you need in Google Ads, rather than selecting **All Events**.
6. Save when finished.

#### Enable integration

The final step is enabling the destination. You must enable the destination to start streaming events.

1. Navigate back to the **Settings** tab and click **Edit**.
2. Toggle **Status** from **Disabled** to **Enabled**.
3. Save when finished.

## Common Issues

### Stuck at the authentication step

You might see the `ERR_BLOCKED_BY_CLIENT` error in your console, which is a common case when AdBlock is installed.
Please remove any browser extensions, clear your cache and cookies, and try to re-add the connection.

### "Error: Invalid Customer ID"

A **Customer ID** is required to set up Google Ads. 
You can learn more about your **Google Ads Customer ID** in [this article](https://support.google.com/google-ads/answer/1704344?hl=en)
- **Manager Account Customer ID** is different from the **Customer ID**.

### Insufficient permissions

Check if you have a Google Ads Manager account and you are an Admin.
In the case of Google Ads, we add and remove users from a specific user list. As such, we need appropriate permissions to create and delete Google Ads account data.
You can learn more about it [here](https://support.google.com/google-ads/answer/9978556)  
