---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: brandwe
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: brandwe
ms.custom: include file
ms.openlocfilehash: 1604b7c9ee9888375e65aa679803c6e996e13b14
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988239"
---
## <a name="register-your-application"></a>Uw toepassing registreren

U kunt uw toepassing registreren op twee manieren, zoals beschreven in de volgende twee secties.

### <a name="option-1-express-mode"></a>Optie 1: Snelle modus

Nu moet u uw toepassing registreren in de *Portal voor Appregistratie Microsoft*:

1. Registreren van uw toepassing via de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure).
2. Voer een naam voor uw toepassing en uw e-mailadres.
3. Zorg ervoor dat de optie voor begeleide instelling is ingeschakeld.
4. Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code.

### <a name="option-2-advanced-mode"></a>Optie 2: Geavanceerde modus

1. Ga naar [Microsoft Portal voor Appregistratie](https://apps.dev.microsoft.com/portal/register-app).
2. Voer een naam in voor de toepassing.
3. Zorg ervoor dat de optie voor begeleide instelling is uitgeschakeld.
4. Selecteer `Add Platform` en selecteer vervolgens `Native Application`.
5. Selecteer `Save`.
6. Ga terug naar Xcode. In `ViewController.swift`, vervang de regel die begint met '`let kClientID`' met de toepassings-ID die u zojuist hebt geregistreerd:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Beheren en klikt u op <code>Info.plist</code> het contextmenu, en klik vervolgens op: <code>Open As</code> > <code>Source Code</code>
</li>
<li>
Onder de <code>dict</code> hoofd-knooppunt, voeg de volgende:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
Vervang <i> <code>[Your_Application_Id_Here]</code> </i> met de toepassings-Id die u zojuist hebt geregistreerd
</li>
</ol>
