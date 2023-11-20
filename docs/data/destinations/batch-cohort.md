---
title: Send Cohorts to Batch
description: Sync cohorts from Amplitude to Batch
---

!!!Tip

    This integration is maintained by Batch. Contact the Batch support team for support with this integration. 

[Batch](https://batch.com/) allows advertisers to access a wide range of digital advertising inventory, including display ads, video ads, mobile ads, native ads, and more. Advertisers can use the platform to target specific audiences based on various parameters such as demographics, interests, browsing behavior, and location. The platform also offers real-time bidding capabilities, allowing advertisers to bid on ad placements in real-time auctions. This integration lets you sync cohorts from Amplitude to Batch.  

## Considerations

- This integration is only available for customers who have paid plans with Amplitude.
- You must enable this integration in each Amplitude project you want to use it in.
- You will also need to have a paid Batch plan to enable this integration.
- You will need to have custom audiences enabled as well as third-party connections on your Batch account.
- Batch expects an identifier that matches the Batch Custom User ID field. This means the User_ID or user property you select in Amplitude must contain the same identifier as the one tracked in Batch.

## Setup

### Batch setup

1. In Batch, navigate to Settings > General.
2. Copy the API Key ("Live API Key" for mobile or "SDK API Key" for web) of the application you want to use Amplitude’s Cohorts in as well as your account's REST API Key.

### Amplitude setup

1. In Amplitude Data, click **Catalog** and select the **Destinations** tab.
2. In the Cohort section, click **Batch**.
3. Click **Add another destination**.
4. Enter **Name** and paste in the **API key** you copied from **Batch**.
5. Map the Amplitude User ID field to the Batch User ID field
6. Save when finished.

## Send a cohort

To sync your first cohort, follow these steps:

1. In Amplitude, open the cohort you want to sync, then click **Sync**.
2. Select **Batch**, then click **Next**.
3. Choose the account you want to sync to.
4. Choose the sync cadence.
5. When finished, save your work.

## Locating your cohort in Batch

Once the Cohort is synched to Batch, you should see it appear in the “Audiences” tab of the dashboard. You can then use it in a Push or In-App campaign by selecting it in the targeting section of the campaign editor. This is useful if you want to include or exclude a segment of users from your campaign.

### Use cases

1. **Targeted Campaigns:** By syncing specific user cohorts from Amplitude, which segments users based on their behavior or attributes, to Batch, companies can create highly targeted push  or In-App campaigns. For example, a cohort of users who have abandoned their shopping carts could be targeted with a notification offering a discount to encourage completion of the purchase.
2. **User Retention:** Cohorts of users who have not engaged with the app or website for a set period can be identified in Amplitude and synced to Batch. These users can then be re-engaged with personalized content or updates relevant to their previous interactions, aiming to bring them back to the app or website.
3. **Product Feedback and Updates:** For users who have engaged with certain features of an app or service (a cohort identified in Amplitude), syncing this information to Batch would allow for sending specific notifications about updates, improvements, or requests for feedback on those features, ensuring that the most relevant users are being reached.
