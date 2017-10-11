---
title: Overzicht van Azure Mobile Engagement Web SDK | Microsoft Docs
description: De nieuwste updates en procedures voor de webservice-SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 5bbc2fda-0f3f-43d0-a73d-0f2c0f8dc25b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 10/18/2016
ms.author: piyushjo
ms.openlocfilehash: 770a83131a3e661771db50b22ce7de25b2d541cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement-web-sdk"></a>Azure Mobile Engagement Web SDK
Begin hier als u de details over het integreren van Azure Mobile Engagement in een web-app. Als u wilt u het proberen voordat het aan de slag met uw eigen web-app, raadpleegt u onze [15 minuten zelfstudie](mobile-engagement-web-app-get-started.md).

## <a name="integration-procedures"></a>Integratie procedures
1. Meer informatie over [Mobile Engagement integreren in uw web-app](mobile-engagement-web-integrate-engagement.md).
2. Voor implementatie van tag plan, meer [hoe u de geavanceerde Mobile Engagement tags API in uw web-app](mobile-engagement-web-use-engagement-api.md).

## <a name="release-notes"></a>Releaseopmerkingen
### <a name="202-10182016"></a>2.0.2 (10/18/2016)
* Vaste crash op persoonlijke bladeren (Safari).
* Vaste crash browsers met cookies is uitgeschakeld.

Raadpleeg voor alle versies, de [voltooien releaseopmerkingen](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>Upgradeprocedures
### <a name="upgrade-from-121-to-200"></a>Upgrade uitvoeren van 1.2.1 naar 2.0.0
De volgende secties wordt beschreven hoe een Mobile Engagement Web SDK-integratie migreren vanaf de Capptain-service, die worden aangeboden door Capptain SAS, naar een Azure Mobile Engagement-app. Als u vanaf een versie ouder dan 1.2.1 migreert, neem contact op met de website Capptain om te migreren naar 1.2.1 eerst en pas de volgende procedures.

Deze versie van de Mobile Engagement Web SDK biedt geen ondersteuning voor Samsung Smart TV, Opera TV, webOS of de Reach-functie.

> [!IMPORTANT]
> Capptain en Azure Mobile Engagement zijn niet dezelfde service en de volgende procedures Markeer alleen het migreren van de client-app. Migreren van de Mobile Engagement Web SDK in de app wordt niet uw gegevens migreren vanaf een server Capptain met een Mobile Engagement-server.
> 
> 

#### <a name="javascript-files"></a>JavaScript-bestanden
Het bestand capptain-sdk.js vervangen door de azure-engagement.js bestand, en werk vervolgens de invoer van uw script dienovereenkomstig.

#### <a name="remove-capptain-reach"></a>Capptain Reach verwijderen
Deze versie van de Mobile Engagement Web SDK biedt geen ondersteuning voor de Reach-functie. Als u hebt Capptain Reach geïntegreerd in uw toepassing, moet u om deze te verwijderen.

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

#### <a name="remove-deprecated-apis"></a>Afgeschafte API's verwijderen
Sommige-API's van Capptain zijn afgeschaft in de Mobile Engagement Web SDK.

Verwijder alle aanroepen naar de volgende API's: `agent.connect`, `agent.disconnect`, `agent.pause`, en `agent.sendMessageToDevice`.

Verwijder een van de volgende callbacks van uw configuratie Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, en `onPushMessageReceived`.

#### <a name="configuration"></a>Configuratie
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

De verbindingsreeks voor uw toepassing wordt weergegeven in de Azure-portal.

#### <a name="javascript-apis"></a>JavaScript-API 's
Het globale JavaScript-object `window.capptain` heeft gekregen `window.azureEngagement`, maar u kunt de `window.engagement` alias voor de API-aanroepen. U kunt deze alias niet gebruiken voor het definiëren van de configuratie van de SDK.

Bijvoorbeeld: `capptain.deviceId` wordt `engagement.deviceId`, `capptain.agent.startActivity` wordt `engagement.agent.startActivity`, enzovoort.

Als u hebt al een eerdere versie van Azure Mobile Engagement Web SDK geïntegreerd in uw toepassing, leest u over [procedures upgrade](mobile-engagement-web-upgrade-procedure.md).

