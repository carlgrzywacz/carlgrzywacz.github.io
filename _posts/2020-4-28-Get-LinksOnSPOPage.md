---
layout: post
title: Read a SharePoint Online page with PowerShell
---
Invoke-WebRequest is useful tool to parse websites.  Unfortanately, Invoke-WebRequest isn't compatible with SharePoint Online Sites using multi-factor authentication (MFA).  PnP PowerShell (https://docs.microsoft.com/en-us/powershell/sharepoint/sharepoint-pnp/sharepoint-pnp-cmdlets?view=sharepoint-ps) to the rescue!  

The example below gets links that don't open in a new tab and displays the Link Information. First, connect to a site using the web login functionality, which is compatible with MFA.  Next we get a page and create a new HTML Object to mimic the result from Invoke-WebRequest.  Then we are able to interact with page data like hyperlinks.

<script src="https://gist.github.com/carlgrzywacz/4c514868b48888dbd1636d2a31253b48.js"></script>