---
title: Azure Mobile Engagement Android SDK-integratie
description: Meest recente updates en -procedures voor Android-SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: d72b5014-a22b-4a7f-a470-d2b8145b5b86
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/10/2016
ms.author: piyushjo
ms.openlocfilehash: 0282abbf44406cac89c13520bc2a4e375817ed1f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-integrate-gcm-with-mobile-engagement"></a>Het integreren van GCM met Mobile Engagement
> [!IMPORTANT]
> U moet de integratie procedure beschreven in de manier waarop integreren engagement voor Android-document voordat u deze handleiding volgen.
> 
> Dit document is alleen nuttig als u al is geïntegreerd de Reach-module en plannen voor de push-Google Play-apparaten. Als u wilt integreren in uw toepassing Reach-campagnes, lees eerst integreren Engagement bereiken op Android.
> 
> 

## <a name="introduction"></a>Inleiding
Integratie van GCM kan de toepassing moet worden gepusht.

GCM-nettoladingen altijd naar de SDK gepusht bevatten de `azme` sleutel in het gegevensobject. Dus als u GCM voor een ander doel in uw toepassing gebruiken, kunt u filteren op basis van die sleutel pushes.

> [!IMPORTANT]
> Alleen apparaten met Android 2.2 of hoger, met Google Play geïnstalleerd en met Google achtergrond verbinding ingeschakeld kan worden geactiveerd via GCM; u kunt echter integreren deze code veilig op niet-ondersteunde apparaten (alleen intents wordt gebruikt).
> 
> 

## <a name="create-a-google-cloud-messaging-project-with-api-key"></a>Met API-sleutel een Google Cloud Messaging-project maken
[!INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

## <a name="sdk-integration"></a>SDK-integratie
### <a name="managing-device-registrations"></a>Het beheren van apparaat-registraties
Elk apparaat een registratie-opdracht naar de Google-servers moet worden verzonden, anders ze kunnen niet worden bereikt.

Een apparaat kan ook Hef de registratie van GCM-meldingen (het apparaat is automatisch geregistreerd als de toepassing wordt verwijderd).

Als u geen [Google Play-SDK] of u niet al verzend de registratie wordt intentie, kunt u het apparaat automatisch voor u geregistreerd Engagement.

Hiervoor het volgende toevoegen aan uw `AndroidManifest.xml` bestand, binnen de `<application/>` tag:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Registratie-id voor de Engagement Push service communiceren en meldingen ontvangen
Om te communiceren de registratie-id van het apparaat naar de Engagement Push-service en de meldingen ontvangen, het volgende toevoegen aan uw `AndroidManifest.xml` bestand, binnen de `<application/>` tag (zelfs als u apparaten registraties zelf beheren):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Zorg ervoor dat u hebt de volgende machtigingen uw `AndroidManifest.xml` (nadat de `</application>` label).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

## <a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Mobile Engagement toegang verlenen tot uw GCM-API-sleutel
Ga als volgt [in deze handleiding](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) Mobile Engagement toegang verlenen tot uw GCM-API-sleutel.

[Google Play-SDK]:https://developers.google.com/cloud-messaging/android/start
