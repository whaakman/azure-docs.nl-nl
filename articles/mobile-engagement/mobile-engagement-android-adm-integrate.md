---
title: Azure Mobile Engagement Android SDK-integratie
description: Meest recente updates en -procedures voor Android-SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a7d719ec-67b3-4be3-9d7f-0b61a57fe978
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 43987962ea2b7b825b88643d18b4db65f1f1670e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-integrate-adm-with-engagement"></a>Het integreren van ADM met Engagement
> [!IMPORTANT]
> U moet de integratie procedure beschreven in de manier waarop integreren engagement voor Android-document voordat u deze handleiding volgen.
> 
> Dit document is alleen nuttig als u al is geïntegreerd de Reach-module en plannen voor de push-Amazon-apparaten. Als u wilt integreren in uw toepassing Reach-campagnes, lees eerst integreren Engagement bereiken op Android.
> 
> 

## <a name="introduction"></a>Inleiding
Integratie van ADM kan uw toepassing worden geactiveerd wanneer de doelcomputer Amazon Android-apparaten.

ADM-nettoladingen altijd naar de SDK gepusht bevatten de `azme` sleutel in het gegevensobject. Dus als u in uw toepassing ADM voor een ander doel gebruikt, kunt u filteren op basis van die sleutel pushes.

> [!IMPORTANT]
> Alleen Amazon Kindle apparaten met Android 4.0.3 of hoger worden ondersteund door de Amazon Device Messaging; u kunt echter deze code veilig op andere apparaten integreren.
> 
> 

## <a name="sign-up-to-adm"></a>Meld u aan ADM
Als u niet hebt gedaan, moet u de ADM inschakelen op de Amazon-account.

De procedure wordt beschreven op: [ <https://developer.amazon.com/sdk/adm/credentials.html>].

U krijgt bij voltooiing van de procedure:

* OAuth-referenties (een Client-ID en een Clientgeheim) voor Engagement kunnen uw apparaten te pushen.
* Een API-sleutel die moet worden geïntegreerd in uw toepassing.

## <a name="sdk-integration"></a>SDK-integratie
### <a name="managing-device-registrations"></a>Het beheren van apparaat-registraties
Elk apparaat een opdracht registratie moet verzenden naar de ADM-servers, anders ze kunnen niet worden bereikt.

Als u al gebruikt de [ADM-clientbibliotheek], en al [ADM geïntegreerd] rechtstreeks gaat u naar android-sdk-adm-ontvangen.

Als u hebt geen ADM geïntegreerd, heeft Engagement een eenvoudigere manier om in te schakelen in uw toepassing:

Bewerk uw `AndroidManifest.xml` bestand:

* Voeg de Amazon-naamruimte moet beginnen met het bestand als volgt:
  
      <?xml version="1.0" encoding="utf-8"?>
      <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:amazon="http://schemas.amazon.com/apk/res/android"
* In de `<application/>` tag, het toevoegen van deze sectie:
  
      <amazon:enable-feature
         android:name="com.amazon.device.messaging"
         android:required="false"/>
  
      <meta-data android:name="engagement:adm:register" android:value="true" />
* Na het toevoegen van de tag amazon mogelijk hebt u een opbouwfout als uw doel van de Projectbuild lager dan Android 2.1 is. U moet gebruiken een **Android 2.1 +** doel bouwen (Maak je geen zorgen, kunt u nog steeds een `minSdkVersion` ingesteld op 4).
* De ADM-API-sleutel integreren als een actief door [deze procedure].

Volg de instructies van de volgende secties.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Registratie-id voor de Engagement Push service communiceren en meldingen ontvangen
Om te communiceren de registratie-id van het apparaat naar de Engagement Push-service en de meldingen ontvangen, het volgende toevoegen aan uw `AndroidManifest.xml` bestand, binnen de `<application/>` tag (zelfs als u de ADM zonder Engagement gebruiken):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Zorg ervoor dat u hebt de volgende machtigingen uw `AndroidManifest.xml` (voordat de `</application>` label).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

## <a name="grant-engagement-oauth-credentials"></a>Verleen Engagement OAuth-referenties
Dien uw OAuth-referenties (Client-ID en Clientgeheim) in de Engagement-Portal.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM-clientbibliotheek]:https://developer.amazon.com/sdk/adm/setup.html
[ADM geïntegreerd]:https://developer.amazon.com/sdk/adm/integrating-app.html
[deze procedure]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
