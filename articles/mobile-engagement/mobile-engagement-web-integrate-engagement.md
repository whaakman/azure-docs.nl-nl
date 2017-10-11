---
title: Azure Mobile Engagement Web SDK-integratie | Microsoft Docs
description: De nieuwste updates en procedures voor het Azure Mobile Engagement Web SDK
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: b5daa2a2-942b-489d-aa1d-568c3b25e56f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 02/29/2016
ms.author: piyushjo
ms.openlocfilehash: 7d8eaa180e277741a583522ee62d68f5247b92bb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Azure Mobile Engagement integreren in een webtoepassing
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

De procedures in dit artikel beschrijven de eenvoudigste manier om het activeren van de analyses en bewaking functies in Azure Mobile Engagement in uw webtoepassing.

Volg de stappen voor het activeren van de logboek-rapporten die nodig zijn voor alle statistische gegevens over gebruikers, sessies, activiteiten, crashes en technicals berekenen. Voor statistieken van afhankelijk zijn van toepassing, zoals gebeurtenissen, fouten en taken, moet u handmatig log-rapporten activeren met behulp van de API van Azure Mobile Engagement. Informatie voor meer informatie over [hoe u de geavanceerde Mobile Engagement API tags in een webtoepassing](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Inleiding
[Download het Azure Mobile Engagement Web SDK](http://aka.ms/P7b453).
De Mobile Engagement Web SDK wordt geleverd als een JavaScript-bestand van één azure-engagement.js, die u wilt opnemen in elke pagina van de toepassing van uw site of webtoepassing.

> [!IMPORTANT]
> Voordat u dit script uitvoert, moet u een script of codefragment die u schrijft voor het configureren van Mobile Engagement voor uw toepassing uitvoeren.
> 
> 

## <a name="browser-compatibility"></a>Browsercompatibiliteit
De Mobile Engagement Web SDK maakt gebruik van systeemeigen JSON coderen en decoderen, naast de AJAX-aanvragen tussen domeinen (afhankelijk van de specificatie W3C CORS). Is compatibel met de volgende browsers:

* Microsoft Edge 12 +
* Internet Explorer 10 +
* Firefox 3.5 +
* Chrome 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Configureren van Mobile Engagement
Een script schrijven dat wordt gemaakt van een globale `azureEngagement` JavaScript-object, zoals in het volgende voorbeeld. Omdat de site meerdere pagina's hebben mogelijk, in dit voorbeeld wordt ervan uitgegaan dat dit script wordt opgenomen in elke pagina. In dit voorbeeld wordt de JavaScript-object met de naam `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

De `connectionString` waarde voor uw toepassing wordt weergegeven in de Azure-portal.

> [!NOTE]
> `appVersionName`en `appVersionCode` zijn optioneel. We raden echter aan dat u ze configureren zodat analytics versie-informatie kan verwerken.
> 
> 

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Mobile Engagement scripts opnemen in uw pagina 's
Mobile Engagement-scripts toevoegen aan uw pagina's in een van de volgende manieren:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Of dit:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias
Nadat het script Mobile Engagement Web SDK is geladen, maakt de **engagement** alias voor toegang tot de SDK-API's. U kunt deze alias niet gebruiken voor het definiëren van de configuratie van de SDK. Deze alias wordt gebruikt als een verwijzing in deze documentatie.

Houd er rekening mee dat als de standaardalias met een andere globale variabele van uw pagina conflicteert, u het in de configuratie als volgt desgewenst kunt voordat het laden van de Mobile Engagement Web SDK:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Basic-rapportage
Basic rapportage in Mobile Engagement bevat informatie over de statistieken van de sessie-niveau, zoals statistieken over gebruikers, sessies, activiteiten en crashes.

### <a name="session-tracking"></a>Sessie bijhouden
Een sessie Mobile Engagement is onderverdeeld in een volgorde van activiteiten, elk aangeduid met een naam.

In een klassieke website raden wij u aan een andere activiteit te declareren op elke pagina van uw site. Voor een website of webtoepassing toepassing waarin de huidige pagina nooit verandert, kunt u de activiteiten bijhouden op kleinere schaal, zoals op de pagina.

In beide gevallen aanroepen om te starten of het wijzigen van de huidige gebruikersactiviteit, de `engagement.agent.startActivity` functie. Bijvoorbeeld:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

De Mobile Engagement-server wordt automatisch een geopende sessie binnen drie minuten nadat de pagina van de toepassing wordt gesloten beëindigd.

U kunt ook u kunt een sessie beëindigen handmatig door het aanroepen van `engagement.agent.endActivity`. Hiermee stelt u de huidige gebruikersactiviteit 'Inactief'.  De sessie beëindigd 10 seconden later tenzij een nieuwe aanroep `engagement.agent.startActivity` hervatten van de sessie.

U kunt de vertraging van 10 seconden in het object globale engagement als volgt configureren:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [!NOTE]
> U kunt geen gebruiken `engagement.agent.endActivity` in de `onunload` retouraanroep omdat u niet AJAX-aanroepen in deze fase.
> 
> 

## <a name="advanced-reporting"></a>Geavanceerde rapportage
Als u rapporteren van specifieke gebeurtenissen, fouten en taken wilt, moet u eventueel de Mobile Engagement-API gebruiken. U toegang tot de API van de Mobile Engagement via de `engagement.agent` object.

U kunt toegang tot alle van de geavanceerde functies in Mobile Engagement in de Mobile Engagement-API. De API wordt beschreven in het artikel [hoe u de geavanceerde Mobile Engagement API tags in een webtoepassing](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-the-urls-used-for-ajax-calls"></a>De URL's gebruikt voor AJAX-aanroepen aanpassen
U kunt URL's die gebruikmaakt van de Mobile Engagement Web SDK aanpassen. Bijvoorbeeld, als u wilt definiëren de logboek-URL (het SDK-eindpunt voor logboekregistratie), kunt u de configuratie als volgt vervangen:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Als de functies van uw URL als een tekenreeks die begint resultaat met `/`, `//`, `http://`, of `https://`, het standaardschema wordt niet gebruikt. Standaard de `https://` schema wordt gebruikt voor deze URL's. Als u aanpassen van het standaardschema wilt, overschrijven de configuratie, als volgt:

    window.azureEngagement = {
      ...
      urls: {
        ...         
        scheme: '//'
      }
    };
