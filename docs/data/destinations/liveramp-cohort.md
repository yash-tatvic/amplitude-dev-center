---
title: Send Cohorts to Liveramp
description: Sync cohorts from Amplitude to Liveramp
---

--8<-- "includes/editions-all-paid-editions.md"

[LiveRamp](https://liveramp.com/) is a data connectivity platform that enables businesses to connect, unify, control, and activate data across different channels and devices to optimize customer experiences and drive better results.

This guide is your resource for sending cohorts from Amplitude to LiveRamp, a powerful data connectivity platform. By following the instructions outlined here, you can seamlessly synchronize and activate your cohorts within LiveRamp. This process enables you to leverage LiveRamp's extensive network of integrations with top measurement and media partners, spanning over 550+ destinations. Whether you opt for sending cohorts via Secure File Transfer Protocol (SFTP), Amazon S3, or Snowflake, this guide provides a step-by-step walkthrough to enhance your customer experiences and achieve superior results while reaching a vast array of platforms and services.

## Considerations

- Amplitude lacks a built-in LiveRamp destination.
- A paid LiveRamp license is required for this integration.


## Send a cohort through Secure File Transfer Protocol (SFTP) 

To send a cohort from Amplitude to LiveRamp through SFTP:

1. **Create the Cohort in Amplitude:** If the cohort you want to send does not exist, create it in Amplitude.
2. **Export the Cohort from Amplitude:** Export the cohort as a CSV file or with the Behavioral Cohorts API. See this [document](https://help.amplitude.com/hc/en-us/articles/360028552471-Amplitude-Audiences-overview-Drive-conversions-with-true-one-to-one-personalization-) for more details.
3. **Upload the Cohort to SFTP Server:** Upload the exported cohort file to LiveRamp's SFTP server. See LiveRamp's article [Upload a File via LiveRamp's SFTP](https://docs.liveramp.com/connect/en/upload-a-file-via-liveramp-s-sftp.html) for more information. LiveRamp may take up to 3 days to ingest your cohort data from the SFTP location.
4. **Choose Destination Account:** In LiveRamp, select a destination account based on your media plan (e.g., Facebook Ads) for cohort activation.
5. **Add Cohort to Destination:** Add the uploaded cohort to the chosen destination account in LiveRamp for activation.

### Send a cohort with Amazon S3

To send a cohort from Amplitude to LiveRamp through an Amazon S3 bucket:

1. **Create Cohort in Amplitude:** Create the desired cohort within Amplitude.
2. **Configure the Amazon S3 Bucket:** Use the AWS Management Console to create an S3 bucket, if you don't have one.
3. **Send the Cohort to Amazon S3:** Use the [Amazon S3 cohort sync](https://www.docs.developers.amplitude.com/data/destinations/amazon-s3-cohort/) integration to sync cohort data from Amplitude to your Amazon S3 bucket.
4. **Log Into LiveRamp:** Access your LiveRamp account.
5. **Find the Cohort:** Locate the cohort within your LiveRamp account.
6. **Choose Destination Account:** Select a destination account aligned with your media plan (e.g., Facebook Ads) for cohort activation.
7. **Add Cohort to Destination:** Add the cohort to the chosen destination account within LiveRamp to activate it.

## Send cohort via Snowflake

1. **Send Cohorts to Amazon S3:** Send cohorts from Amplitude to Amazon S3. See this document for more details.
2. **Bulk Load from Amazon S3:** Load cohort data from your Amazon S3 bucket into your Snowflake data warehouse. See this document for more details.
3. **Install LiveRamp Native Application on Snowflake:** Install the LiveRamp native application in your Snowflake environment.
4. **Enable LiveRamp Embedded Identity in Snowflake:** Activate LiveRamp's Identity Resolution in Snowflake to translate identifiers to RampIDs.
5. **Share Data to LiveRamp Account:** Share the processed data with your LiveRamp account.
6. **Choose Destination Account:** Finally, select a destination account based on your media plan (e.g., Facebook Ads) and add the cohort for activation.
