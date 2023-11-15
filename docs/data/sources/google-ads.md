---
title: Import Google Ads Data
description: Amplitude Data's Google Ads integration lets you import your Google Ad spend, click, and impression data in just a few steps.
---

Amplitude Data's Google Ads integration lets you import your Google Ad spend, click, and impression data in just a few steps.

!!!note "Other Amplitude + Google Ads Integrations"

    This integration imports Google Ads data into Amplitude. Amplitude offers other integrations with Google Ads:

    - [Send Cohorts to Google Ads](/data/destinations/google-ads-cohort)

## Setup

### Prerequisites

To set up, you need the following:

- [Google Ads Customer ID](https://support.google.com/google-ads/answer/1704344?hl=en) of the ad account you want to connect to.
- If you don't have direct access to the account, `Google Ads Manager ID` that you authorized access on which can view this ad account.

### Considerations

- This source only imports **metrics data** from Google Ads. Hence, you can not import other types of data (e.g., experiments).
- Amplitude's Google Ads integration imports data from Google Ads **once a day**, so you will always see your Daily Ad Metrics populated at a certain time. You can check the time of the next sync in the settings.
- The advertising data you import will be associated with the new Amplitude event called `Daily Ad Metrics` with several event properties, such as `Ad Impressions`, `Ad Clicks`, `Ad Group ID`, etc. For more information, see [Ad-Network Integrations in Amplitude](https://amplitude.com/blog/ad-network-integration)
- Google Ads import pulls in data and clicks on a **per-ad** level. Hence, the advertising metrics and properties are not tied to users and can only be displayed along with the `Display Ad Metrics` event. Therefore, `Daily Ads Metric` events might be the only event showing up in your users' event stream.

### Amplitude setup

In Amplitude, navigate to **Data Sources**, then find **Google Ad** in the **I want to import data into Amplitude** tab.

![Google Add Source](../../assets/images/marketing-analytics/add-sources.png)

1. Log into Google and grant Amplitude permission in the consent form.
![Google Login Image](../../assets/images/marketing-analytics/google-login.png)
2. Enter the Google Ads Customer ID for the ad account you want to import data from.
![Google Enter Account ID](../../assets/images/marketing-analytics/google-enter-info.png)
3. If you don't have direct access to the account, enter the `Manager ID` that you authorized access on which can view this ad account. Otherwise, just leave the field as blank.
4. [Optional] Import past data for a given period.
![Google Historical Backfill](../../assets/images/marketing-analytics/google-past-data.png)

For more information on how you can use the data from this integration in Amplitude, see [this blog post](https://amplitude.com/blog/ad-network-integration).

## Common issues

### Insufficient permissions

Amplitude's Google Ads Import integration requires that your Google Ads Manager account has administrator privileges. This level of permission allows Amplitude to add and remove users from specific user lists in Google Ads. 
For more information, see [About access levels in your Google Ads Account](https://support.google.com/google-ads/answer/9978556) in Google's documentation. 

### Import job ingests no data 

1. Check if you reject unplanned events in your [Schema settings](https://help.amplitude.com/hc/en-us/articles/360055495852-Configure-the-Schema-settings-to-handle-unexpected-data). If you reject unplanned data - Amplitude will not store the event or its properties.
You can learn more about handling unplanned event types in this article.
2. Do your users have corresponding accounts in Amplitude and Google Ads? Google Ads import will try to match users between platforms based on the key-value pairs you selected, so your import job will fail if it does not find any corresponding values.

### Daily Ad Metric discrepancies 

Your metrics may occasionally update days after a click occurs. This can happen for a number of reasons, including but not limited to:
- When a conversion occurs days after the initial click
- When the source detects and removes invalid traffic
Hence, if Google updates the data afterward - the import job will have to run again to get the updated numbers.
For more information, see [About data freshness](https://support.google.com/google-ads/answer/2544985?hl=en) in Google's documentation. 
