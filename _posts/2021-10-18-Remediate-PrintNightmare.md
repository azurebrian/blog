---
layout: post
description: Use this script to remediate Print Nightmare 
categories: [security, 'blue team']
title: Find Machines Vulnerable to Print Nightmare and Remediate
date: 2021-10-18
comments: true
published: true
---

It's been a while, and boy was it a Spring/Summer to remember regarding Microsoft vulnerabilities.  Print Nightmare ([CVE-2021-34527](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-34527)) scared me enough to want to ensure I went "blue team" enough to prevent any infection. So, I crafted a somewhat crude, but effective, script to detect and remediate on the machines I had influence over.  You can find the script in my [Github repo](https://github.com/azurebrian/scripts/blob/6b5306c543027f162307070cbf101263c4a3fd29/Remediate-PrintNightmare.ps1). 

## A couple of things to point out

You'll notice right at the top that my intention was to disable the print spooler service altogether where I could get away with it:

```Powershell
#For more info:  https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-34527

#For machines where printing is not needed and we can just disable printing altogether!!!
Get-Service -Name Spooler
Stop-Service -Name Spooler -Force
Set-Service -Name Spooler -StartupType Disabled
```

Of course, on user machines that's a little more difficult and hence the trouble to detect and remediate those machines.  The most fool-proof way to detect if the appropriate patch has been installed is to search for it:

```Powershell
$Session = New-Object -ComObject "Microsoft.Update.Session"
$Searcher = $Session.CreateUpdateSearcher()
$historyCount = $Searcher.GetTotalHistoryCount()
$hotfix = $Searcher.QueryHistory(0, $historyCount) | Where-Object {$_.Title -like "*KB5004945*"}
if ($null -ne $hotfix)
{
    $patchApplied = $True
}
else 
{
    Write-Host "Patch KB5004945 has not been applied and you are at risk.  Please install Windows Updates and run this script again."    
}

```

You might be tempted to use the `Get-Hotfix` Poweshell command to detect for the presence of the installed update.  Sometimes that works, but not always according to this [post](https://docs.microsoft.com/en-us/answers/questions/191945/get-hotfix-not-returning-all-installed-kbs.html).

The rest of the script is pretty much taken from the recommendations in the [Microsoft Security Update Guide](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-34527) for this vulnerability.  Please have a look and feel free to drop a comment regarding any similar solutions you might have come up with.  And feel free to download, mod, and even open a PR if you like.  

Thanks for reading!
