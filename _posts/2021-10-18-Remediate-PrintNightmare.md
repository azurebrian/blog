---
layout: post
description: Use this script to remediate Print Nightmare 
categories: [security, 'blue team']
title: Find Machines Vulnerable to Print Nightmare and Remediate
comments: true
published: true
---

It's been a while, and boy was it a Spring/Summer to remember regarding Microsoft vulnerabilities.  Print Nightmare ([CVE-2021-34527](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-34527)) scared me enough to want to ensure I went "blue team" enough to prevent any infection. So, I crafted a somewhat crude, but effective, script to detect and remediate on the machines I had influence over.  You can find the script in my [Github repo](https://github.com/azurebrian/scripts/blob/6b5306c543027f162307070cbf101263c4a3fd29/Remediate-PrintNightmare.ps1). You'll notice right at the top that my intention was to disable the print spooler service altogether where I could get away with it.  Of course, on user machines that's a little more difficult and hence the trouble to detect and remediate those machines.  Anyway, this is pretty short and to the point I guess, so I won't belabor any unnecessary details.  Please have a look and feel free to drop a comment regarding any similar solutions you might have come up with.  And feel free to download, mod, and even open a PR if you like.  

Thanks for reading!
