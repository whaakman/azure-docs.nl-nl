---
title: Azure AD v2 Android aan de slag - configureren | Microsoft Docs
description: Hoe een Android-app kunt ophalen van een toegangstoken en Microsoft Graph API of API's waarvoor toegangstokens van Azure Active Directory-v2-eindpunt aanroepen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 945b09ccdb7537987da33d32d94a3ccacd829ffd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
## <a name="create-an-application-express"></a>Maken van een toepassing (snelle)
Nu gaat u naar het registreren van uw toepassing in de *Portal voor registratie van Microsoft-toepassing*:
1. Registreren van uw toepassingen via de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)
2.  Voer een naam voor uw toepassing en uw e-mailadres
3.  Zorg ervoor dat de optie voor begeleide Setup is ingeschakeld
4.  Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>De registratiegegevens van uw toepassing toevoegen aan uw oplossing (Geavanceerd)
Nu gaat u naar het registreren van uw toepassing in de *Portal voor registratie van Microsoft-toepassing*:
1. Ga naar de [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/portal/register-app) een toepassing registreren
2. Voer een naam voor uw toepassing en uw e-mailadres 
3. Zorg ervoor dat de optie voor begeleide Setup is uitgeschakeld
4. Klik op `Add Platform`, selecteer daarna `Native Application` en klik op Opslaan
5.  Open `MainActivity` (onder `app`  >  `java`  >   *`{host}.{namespace}`* )
6.  Vervang de *[Voer de toepassing Id hier]* in de regel die begint met `final static String CLIENT_ID` met de toepassings-ID die u zojuist hebt geregistreerd:

```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Open `AndroidManifest.xml` (onder `app`  >  `manifests`) toevoegen van de volgende activiteiten `manifest\application` knooppunt. Hiermee registreert een `BrowserTabActivity` waarmee het besturingssysteem op uw toepassing na het voltooien van de verificatie hervatten:
</li>
</ol>

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        
        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, the scheme should be similar to 'msal[appId]' -->
        <data android:scheme="msal[Enter the application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```
<!-- Workaround for Docs conversion bug -->
<ol start="8">
<li>
In de `BrowserTabActivity`, Vervang `[Enter the application Id here]` met de toepassings-ID.
</li>
</ol>
