---
layout: post
title: How to Troubleshoot issues with Microsoft Access and SharePoint
---


Problem: Users in a SharePoint Farm are trying to import a spreadsheet with Microsoft Access to SharePoint 2013 and receive the following error: "Cannot update. Database or object is read-only."

Solution: There is a known issue with SharePoint and Access that also results in the same error but we already <a href="https://docs.microsoft.com/en-us/office/troubleshoot/access/access-linked-table-return-deleted">setup the database for Never Cache</a>.  We reproduced the issue on multiple Production 2013 farms but could not repro on SP 2013 UAT and SP 2019 Production environments.  This ruled out any potential issues related to the Access client.  

The issue was reproduced while running Fiddler and analyzed the "_viti_bin/lists.asmx" call.  We noticed that the X-SharePointHealthScore was 0 which indicated the problem most because of server load.  If server load was the problem there most likely would have been other performance problems on the farms.  The SPRequestGuid was used to pull the ULS logs and we identified a custom Event Receiver running on the list in question and it shouldn't have been.  There was a maintenance window over the weekend and this solution was deployed to a larger scope than expected.  Correcting the scope of the feature resolved the issue.