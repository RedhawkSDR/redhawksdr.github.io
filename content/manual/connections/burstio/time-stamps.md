---
title: "Time Stamps"
weight: 40
---

The following code segment provides an example of how to construct a `BULKIO::PrecisionUTCTime` time stamp to be sent in the burst SRI.

```c++
/**
 * To create a time stamp from the current time of day
 */

BULKIO::PrecisionUTCTime tstamp = burstio::utils::now();
```
