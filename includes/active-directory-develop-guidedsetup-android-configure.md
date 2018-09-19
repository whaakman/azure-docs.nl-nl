---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: f0e584a4a4a54fc04b5539b56d5c901bfaa42bcc
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293758"
---
## <a name="register-your-application"></a>Uw toepassing registreren
U kunt uw toepassing registreren op twee manieren, zoals beschreven in de volgende twee secties.

### <a name="option-1-express"></a>Optie 1: Express
1. Ga naar de [Microsoft Portal voor Appregistratie](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2.  In **toepassingsnaam**, voer een naam in voor uw toepassing.

3. Zorg ervoor dat de **begeleide installatie** selectievakje is geselecteerd en selecteer vervolgens **maken**.

4. Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code.

### <a name="option-2-advanced"></a>Optie 2: geavanceerde 
1. Ga naar de [Microsoft Portal voor Appregistratie](https://apps.dev.microsoft.com/portal/register-app).
2. In de **toepassingsnaam** voert u een naam voor uw toepassing. 

3. Zorg ervoor dat de **begeleide installatie** selectievakje is uitgeschakeld, en selecteer vervolgens **maken**.

4. Selecteer **Platform toevoegen**, selecteer **systeemeigen toepassing**, en selecteer vervolgens **opslaan**.

5. Onder **app** > **java** > **{host}. { naamruimte}** Open `MainActivity`. 

6.  Vervang *[Voer hier de toepassings-Id]* met uw toepassing / Client-ID:

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
