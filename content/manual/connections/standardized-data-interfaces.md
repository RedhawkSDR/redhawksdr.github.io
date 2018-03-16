---
title: "Standardized Data Interfaces"
weight: 50
---

Data flow between REDHAWK resources (components and devices) is managed through two sets of interfaces: BulkIO and BurstIO. The BulkIO module is designed for streaming data and maximizes the efficiency for bulk data transfers between resources, whereas, BurstIO is designed for applications that require small and possibly non-contiguous chunks of data transfers. Both interfaces also allow for the association of metadata, Signal Related Information (SRI), and a Precision Time Stamp, which describe the content being transferred in support of content processing. The following 3 sections detail the capabilities for both BulkIO and BurstIO implementations and the interfaces they provide.
