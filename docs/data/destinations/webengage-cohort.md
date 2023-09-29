---
title: Send Cohorts to WebEngage
description: Sync cohorts from Amplitude to WebEngage
---

!!!beta

    This integration is currently in beta and is in active development. If you have any feedback to improve the WebEngage destination or have suggestions for their documentation, please contact [WebEngage support team](https://webengage.com/). 

[WebEngage](https://webengage.com/) enables Product and marketing teams to configure in-app nudges to improve feature adoption and drive conversions. Fully no-code.

WebEngage is a user engagement and retention platform that enables you to send personalized marketing communications across 12+ channels, including email, push notification, WhatsApp, and more.

Powered by a robust CDP, build a holistic user profile, and track custom events and user attributes. Take data-driven decisions based on cohorts, funnel analysis, and more. The platform also lets you leverage marketing automation to orchestrate campaigns, at scale, across multiple touchpoints in a customer lifecycle.


!!!tip

    This integration is maintained by WebEngage. Contact the WebEngage support team for support with this integration.

## Considerations

- This integration is only available for customers who have paid plans with Amplitude.
- You must enable this integration in each Amplitude project you want to use it in.
- You need a paid WebEngage plan to enable this integration.
- Amplitude matches the user_id to the CUID within WebEngage to associated cohorts. If a user with that id doesn't exist within WebEngage, a user is not created. Make sure that the Amplitude user_id field matches the WebEngage id field i.e. CUID to avoid user duplication.


## Setup

### WebEngage setup

1. In WebEngage, navigate to Settings > Integrations > Amplitude.
2. Copy the API key to your clipboard.

### Amplitude setup

1. In Amplitude Data, click **Catalog** and select the **Destinations** tab.
2. In the Cohort section, click **WebEngage**.
3. Enter **Name** and paste in the **API** key you copied from **WebEngage**.
4. Select the Amplitude properties that map to WebEngage's User ID.
5. Save when finished.

## Send a cohort

To sync your first cohort, follow these steps:

1. In Amplitude, open the cohort you want to sync, then click **Sync**.
2. Select **WebEngage**, then click **Next**.
3. Choose the account you want to sync to.
4. Choose the sync cadence.
5. When finished, save your work.

### Use cases
Exporting user or behavioral-based Amplitude cohorts to WebEngage enables you to:
1. Drive retention-led growth by activating dormant customers, promoting repeat purchases, and driving platform engagement & content consumption. 2. Engage cohort users with highly targeted & personalized messages through their preferred channel. Be it Push, In-app, SMS, Web Overlays, Web Push, Email, or WhatsApp.

