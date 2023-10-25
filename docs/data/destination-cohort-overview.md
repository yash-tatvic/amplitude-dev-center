---
title: Cohort Destinations Overview
description: Send Amplitude behavioral cohorts to other tools in your stack with just a few clicks, using no-code cohort integrations. 
---

Send Amplitude cohorts to other destinations in your stack with just a few clicks, using no-code cohort integrations.

Learn more about behavioral cohorts in the [Help Center](https://help.amplitude.com/hc/en-us/articles/231881448).

## Considerations

Keep these general considerations in mind when working with cohort destinations.

- You must have a paid [Amplitude plan](https://amplitude.com/pricing) to send cohorts via these integrations.
- Some destinations support data centers outside of the US. For more info, refer to the documentation for the specific destination you want to use. 
- Hourly sync cohorts have a maximum of 1 million users.
- Daily sync cohorts have a maximum of 10 million users.
- If you archive an existing cohort sync, the next sync will fail and the scheduled sync will be disabled.
- When you archive a cohort sync, the next sync fails and Amplitude disables any future scheduled syncs.
- Allow traffic from the following IP addresses, based on your region:
    - Amplitude US IP addresses
        - 52.33.3.219
        - 35.162.216.242
        - 52.27.10.221
    - Amplitude EU IP addresses
        - 3.124.22.25
        - 18.157.59.125
        - 18.192.47.195

!!! info
    Individual destinations may have their own considerations. For best results, read each destination's documentation.

## Cohort syncing destinations

--8<-- "includes/data-destinations-cohorts.md"
