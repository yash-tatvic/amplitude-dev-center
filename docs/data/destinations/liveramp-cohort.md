---
title: Send Cohorts to Liveramp
description: Sync cohorts from Amplitude to Liveramp
---

--8<-- "includes/editions-all-paid-editions.md"

[LiveRamp](https://liveramp.com/) is a data connectivity platform that enables businesses to connect, unify, control, and activate data across different channels and devices to optimize customer experiences and drive better results.

This guide explains how to integrate Amplitude with LiveRamp, offering three methods: SFTP, Amazon S3, and Snowflake. These methods facilitate the transfer and activation of cohorts within LiveRamp, providing flexibility in your data connectivity options.

## Considerations

A paid LiveRamp license is required for this integration.

## Send a cohort through Secure File Transfer Protocol (SFTP) 

To send a cohort from Amplitude to LiveRamp through SFTP:

1. **Create the Cohort in Amplitude:** If the cohort you want to send does not exist, create it in Amplitude.
2. **Export the Cohort from Amplitude:** Export the cohort as a CSV file or with the Behavioral Cohorts API. See this [document](https://help.amplitude.com/hc/en-us/articles/360028552471-Amplitude-Audiences-overview-Drive-conversions-with-true-one-to-one-personalization-) for more details.
3. **Upload the Cohort to SFTP Server:** Upload the exported cohort file to LiveRamp's SFTP server. See LiveRamp's article [Upload a File via LiveRamp's SFTP](https://docs.liveramp.com/connect/en/upload-a-file-via-liveramp-s-sftp.html) for more information. LiveRamp may take up to 3 days to ingest your cohort data from the SFTP location.
4. **Choose Destination Account:** In LiveRamp, select a destination account based on your media plan (for example, Facebook Ads) for cohort activation.
5. **Add Cohort to Destination:** Add the cohort to the chosen destination account within LiveRamp to activate it.

### Send a cohort with Amazon S3

To send a cohort from Amplitude to LiveRamp through an Amazon S3 bucket:

1. **Create the Cohort in Amplitude:** If the cohort you want to send doesn't exist, create it in Amplitude.
2. **Configure the Amazon S3 Bucket:** Use the AWS Management Console to create an S3 bucket, if you don't have one.
3. **Send the Cohort to Amazon S3:** Use the [Amazon S3 cohort sync](https://www.docs.developers.amplitude.com/data/destinations/amazon-s3-cohort/) integration to sync cohort data from Amplitude to your Amazon S3 bucket.
4. **Grant LiveRamp access to your Amazon S3 bucket:** Allow LiveRamp to Access Your AWS S3 Bucket. For more information, see LiveRamp's article, [Allow LiveRamp to Access your AWS S3 Bucket](https://docs.liveramp.com/connect/en/allow-liveramp-to-access-your-aws-s3-bucket.html)
5. **Log Into LiveRamp:** Access your LiveRamp account.
6. **Find the Cohort:** Locate the cohort within your LiveRamp account.
7. **Choose Destination Account:** In LiveRamp, select a destination account based on your media plan (for example, Facebook Ads) for cohort activation.
8. **Add Cohort to Destination:** Add the cohort to the chosen destination account in LiveRamp to activate it.

## Send a cohort with Snowflake

To send a cohort from Amplitude to LiveRamp with Snowflake:

1. **Send Cohorts to Amazon S3:** Send cohorts from Amplitude to Amazon S3. For more information, see [Send Cohorts to Amazon S3](https://www.docs.developers.amplitude.com/data/destinations/amazon-s3-cohort/)
2. **Bulk Load from Amazon S3:** Load cohort data from your Amazon S3 bucket into your Snowflake data warehouse. For more information, see Snowflake's article, [Bulk Loading from Amazon S3](https://docs.snowflake.com/en/user-guide/data-load-s3).
3. **Install LiveRamp Native Application on Snowflake:** Install the LiveRamp Identity Resolution and Transcoding Native Application in your Snowflake environment. For more information, see LiveRamp's article, [Set Up the LiveRamp Native App in Snowflake](https://docs.liveramp.com/identity/en/set-up-the-liveramp-native-app-in-snowflake.html).
4. **Share Data to LiveRamp Account:** Perform initial Application Setup by running the Application Setup SQL. This creates the logging and metrics table which is then shared with Liveramp. For more information, see LiveRamp's article, [Set Up the LiveRamp Native App in Snowflake](https://docs.liveramp.com/identity/en/set-up-the-liveramp-native-app-in-snowflake.html).
5. **Perform Identity Resolution in Snowflake:** Activate LiveRamp's Identity Resolution in Snowflake to translate identifiers to RampIDs. For more information, see LiveRamp's article [Perform Identity Resolution in Snowflake](https://docs.liveramp.com/identity/en/perform-identity-resolution-in-snowflake.html).
6. **Choose Destination Account:** In LiveRamp, select a destination account based on your media plan (for example, Facebook Ads) for cohort activation.
7. **Add Cohort to Destination:** Add the cohort to the chosen destination account within LiveRamp to activate it.
