---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: 2b30f95e050887130db1b2395f51e543a50e25d0
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203693"
---
## <a name="register-your-application"></a>Uw toepassing registreren

U kunt uw toepassing registreren op twee manieren, zoals beschreven in de volgende twee secties.

### <a name="option-1-express"></a>Optie 1: Express

1. Ga naar de [Microsoft-portal voor app-registratie](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2. In **toepassingsnaam**, voer een naam in voor uw toepassing.
3. Zorg ervoor dat de **begeleide installatie** selectievakje is geselecteerd en selecteer vervolgens **maken**.
4. Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code.

### <a name="option-2-advanced"></a>Optie 2: Geavanceerd

1. Ga naar de [Microsoft-portal voor app-registratie](https://apps.dev.microsoft.com/portal/register-app).
2. Typ in het vak **Toepassingsnaam** een naam voor de toepassing.
3. Zorg ervoor dat het selectievakje **Stapsgewijze instelling** is uitgeschakeld en selecteer vervolgens **Maken**.
4. Selecteer **Platform toevoegen**, selecteer **Systeemeigen toepassing** en selecteer vervolgens **Opslaan**.
5. Onder **app** > **java** > **{host}. { naamruimte}** Open `MainActivity`. 
6. Vervang *[Voer hier de toepassings-Id]* met uw toepassing / Client-ID:

    ```java
    final static String CLIENT_ID = "[Enter the application Id here]";
    ```
   <!-- Workaround for Docs conversion bug -->
7. Onder **app** > **manifesten**, open de *AndroidManifest.xml* bestand.
8. In de `manifest\application`, voeg de volgende activiteit. De `BrowserTabActivity` activiteit waarmee Microsoft om aan te roepen terug naar uw toepassing nadat deze de verificatie is voltooid:

    ```xml
    <!--Intent filter to capture System Browser calling back to our app after sign-in-->
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
9. In de `BrowserTabActivity`, Vervang `[Enter the application Id here]` met de toepassing / Client-ID.
