---
title: "BurstIO"
weight: 70
---

For those applications that require small, and possibly non-contiguous, chunks (or bursts) of data with frequently-varying metadata, BurstIO provides the data transfer containers and interfaces to meet those requirements. This interface only supports the transfer of data vectors: float, double, octet (int8/uint8), short (int16), ushort (uint16), long (int32), ulong (uint32), longlong (int64), and ulonglong(uint64). Similar to BulkIO, BurstIO provides Burst Signal Related Information (SRI), and a Precision Time stamp but provides this information in-band with each data burst. With the increased overhead requirement for metadata, BurstIO can achieve its highest throughput by grouping multiple bursts into a single transfer, either programmatically or through configurable policy settings, to try and maximize efficiency and limit latency.

{{% children depth="999" %}}
