+++
title = 'Security Innovations - Shred - Command and Control'
date = '2023-03-09'
description = 'Pwning the Shred room by Security Innovations'
categories = [
    'Walkthroughs',
    'Cmd+Ctrl',
    ]
tags = [
    'ctf',
    ]
+++
Ooooooh, yeah. I did it, because the range hasn't changed for over a fucking year. That's lazy on their part. The security industry evolves, your ranges should too. Here, I will be using [https://caido.io/](Caido) instead of Burp or ZAProxy for this... why? No particular reason. Just to be fair and showcase that such a thing exists. There's also Charles, and many others.

`"><plaintext>` - Breaks the search feature, there's cross site scripting here.

`https://52-53-226-7-shred.vulnerablesites.net/Shred/accountSettings?id=235963` - IDOR - Insecure Direct Object Reference (changing your ID after login gives access to other's profiles, also gives access to tamper with their CC#s and Gift Cards)

`https://52-53-226-7-shred.vulnerablesites.net/Shred/credits` - View the credits page

`test:test` - account left over from development