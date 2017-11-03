---
title: Azure Mobile Engagement Web SDK upgradeprocedures | Microsoft Docs
description: De nieuwste updates en procedures voor de webservice-SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a20529b4-ec8d-4503-8ae9-09b5f0846d5b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: afa8037dcb7a53042fa606e2c4014b442d4be326
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure Mobile Engagement Web SDK upgradeprocedures
Als u hebt al een eerdere versie van Azure Mobile Engagement Web SDK geïntegreerd in uw webtoepassing, moet u rekening houden met de volgende punten wanneer u een upgrade uitvoert van de SDK.

Als u meerdere versies van de Mobile Engagement Web SDK overgeslagen, moet u mogelijk verschillende procedures tijdens de upgrade te voltooien. Bijvoorbeeld, als u van 1.4.0 naar 1.6.0 migreert, eerst de procedures volgen upgrade vanaf 1.4.0 naar 1.5.0. Volg de procedures voor het upgraden van 1.5.0 naar 1.6.0.

Welke versie u een upgrade van uitvoert een vorige versie van het bestand azure-engagement.js vervangen door de meest recente versie van het bestand.

## <a name="upgrade-from-121-to-200"></a>Upgrade uitvoeren van 1.2.1 naar 2.0.0
Deze sectie beschrijft het migreren van een Mobile Engagement Web SDK-integratie van de service Capptain, die worden aangeboden door Capptain SAS, naar een Azure Mobile Engagement-app. Als u vanaf een eerdere versie migreert, neem contact op met de website Capptain voor het eerst naar 1.2.1 migreren en pas de volgende procedures.

Deze versie van de Mobile Engagement Web SDK biedt geen ondersteuning voor Samsung Smart TV, Opera TV, webOS of de Reach-functie.

> [!IMPORTANT]
> Capptain en Azure Mobile Engagement zijn niet dezelfde service. De volgende procedure wordt uitgelegd hoe u alleen voor het migreren van de client-app. Migreren van de Mobile Engagement Web SDK in de app wordt niet uw gegevens migreren vanaf een server Capptain met een Mobile Engagement-server.
> 
> 

### <a name="javascript-files"></a>JavaScript-bestanden
Het bestand capptain-sdk.js vervangen door de azure-engagement.js bestand, en werk vervolgens de invoer van uw script dienovereenkomstig.

### <a name="remove-capptain-reach"></a>Capptain Reach verwijderen
Deze versie van de Mobile Engagement Web SDK biedt geen ondersteuning voor de Reach-functie. Als u Capptain Reach geïntegreerd in uw toepassing, moet u deze verwijderen.

Het importeren van CSS bereiken verwijderen vanaf de pagina en verwijderen van het gerelateerde CSS-bestand (capptain-reach.css, standaard).

Verwijderen van de volgende Reach-resources: de installatiekopie van het sluiten (capptain-close.png, standaard) en het merk-pictogram (standaard capptain-melding-pictogram).

Verwijder de gebruikersinterface bereiken voor in-app-meldingen. De standaardindeling ziet er als volgt:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Verwijder de gebruikersinterface bereiken voor tekst- en aankondigingen van web- en polls. De standaardindeling ziet er als volgt:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Verwijder de `reach` object uit uw configuratie, indien aanwezig. Als volgt uitziet:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Verwijder eventuele andere Reach-aanpassing, zoals categorieën.

### <a name="remove-deprecated-apis"></a>Afgeschafte API's verwijderen
Sommige-API's van Capptain zijn afgeschaft in de Mobile Engagement Web SDK.

Verwijder alle aanroepen naar de volgende API's: `agent.connect`, `agent.disconnect`, `agent.pause`, en `agent.sendMessageToDevice`.

Verwijder alle exemplaren van de volgende retouraanroepen uit de configuratie van uw Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, en `onPushMessageReceived`.

### <a name="configuration"></a>Configuratie
Mobile Engagement gebruikt een verbindingsreeks voor het configureren van de SDK-id's, bijvoorbeeld de toepassings-id.

Vervang de toepassings-ID door de verbindingsreeks. Let op het globale object voor de SDK-configuratie wordt gewijzigd van `capptain` naar `azureEngagement`.

Vóór de migratie:

    window.capptain = {
      appId: ...,
      [...]
    };

Na de migratie:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

De verbindingsreeks voor uw toepassing wordt weergegeven in de Azure-Portal.

### <a name="javascript-apis"></a>JavaScript-API 's
Het globale JavaScript-object `window.capptain` heeft gekregen `window.azureEngagement` , maar u kunt de `window.engagement` alias voor de API-aanroepen. U kunt de alias niet gebruiken voor het definiëren van de configuratie van de SDK.

Bijvoorbeeld: `capptain.deviceId` wordt `engagement.deviceId`, `capptain.agent.startActivity` wordt `engagement.agent.startActivity`, enzovoort.

