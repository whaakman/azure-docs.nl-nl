---
title: Realtime statistieken in Azure CDN | Microsoft Docs
description: Realtime statistieken biedt realtime-gegevens over de prestaties van Azure CDN wanneer het leveren van inhoud aan uw clients.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: c7989340-1172-4315-acbb-186ba34dd52a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e9b9522de6b2c54dc794b00100ffe358296ecfdd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Realtime statistieken in Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht
Dit document wordt uitgelegd realtime statistieken in Microsoft Azure CDN.  Deze functionaliteit biedt realtime gegevens, zoals bandbreedte, cache statussen en gelijktijdige verbindingen met uw CDN-profiel wanneer het leveren van inhoud aan uw clients. Hierdoor kan de doorlopende bewaking van de status van uw service op elk gewenst moment, waaronder Ga live gebeurtenissen.

De volgende grafieken zijn beschikbaar:

* [Bandbreedte](#bandwidth)
* [Statuscodes](#status-codes)
* [Cache-statussen](#cache-statuses)
* [Verbindingen](#connections)

## <a name="accessing-real-time-stats"></a>Toegang tot realtime statistieken
1. In de [Azure Portal](https://portal.azure.com), blader naar uw CDN-profiel.
   
    ![Blade CDN-profiel](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop blade CDN-profiel beheren](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
3. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **realtime statistieken** doel.  Klik op **HTTP Large Object**.
   
    ![CDN-beheerportal](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    De grafieken realtime statistieken worden weergegeven.

Elk van de grafieken realtime geeft statistieken weer voor de geselecteerde tijdsspanne gestart wanneer de pagina wordt geladen.  De grafieken worden automatisch bijgewerkt om de paar seconden.  De **grafiek vernieuwen** knop, indien aanwezig, wordt gewist de grafiek, waarna deze alleen de geselecteerde gegevens weergegeven.

## <a name="bandwidth"></a>Bandbreedte
![Bandbreedte-grafiek](./media/cdn-real-time-stats/cdn-bandwidth.png)

De **bandbreedte** diagram toont de hoeveelheid bandbreedte die wordt gebruikt voor het huidige platform via de geselecteerde tijdsperiode. Het gearceerde gedeelte van de grafiek geeft bandbreedtegebruik. De exacte hoeveelheid bandbreedte die momenteel wordt gebruikt, wordt direct onder de lijngrafiek weergegeven.

## <a name="status-codes"></a>Statuscodes
![Status code grafiek](./media/cdn-real-time-stats/cdn-status-codes.png)

De **statuscodes** grafiek geeft aan hoe vaak bepaalde HTTP-antwoordcodes via de geselecteerde tijdsspanne plaatsvinden.

> [!TIP]
> Zie voor een beschrijving van elke optie voor HTTP-status code [Azure CDN HTTP-statuscodes](https://msdn.microsoft.com/library/mt759238.aspx).
> 
> 

Een lijst met HTTP-statuscodes wordt direct boven de grafiek weergegeven. Deze lijst geeft aan dat elke statuscode die kan worden opgenomen in de lijngrafiek en het huidige aantal exemplaren per seconde voor deze statuscode. Een regel wordt standaard weergegeven voor elk van deze statuscodes in de grafiek. U kunt echter alleen de statuscodes waarvoor speciale betekenis voor uw CDN-configuratie te controleren. U doet dit door de gewenste statuscodes controleren en wissen van alle andere opties en klik vervolgens op **grafiek vernieuwen**. 

U kunt tijdelijk logboekgegevens voor een bepaalde statuscode verbergen.  Klik op de statuscode die u wilt verbergen in de legenda direct onder de grafiek. De statuscode wordt onmiddellijk worden verborgen in de grafiek. Die statuscode opnieuw te klikken, wordt deze optie moet opnieuw worden weergegeven.

## <a name="cache-statuses"></a>Cache-statussen
![Cache statussen grafiek](./media/cdn-real-time-stats/cdn-cache-status.png)

De **Cache statussen** grafiek geeft aan hoe vaak bepaalde typen statussen van de cache via de geselecteerde tijdsspanne plaatsvinden. 

> [!TIP]
> Zie voor een beschrijving van elke optie cache status code [Azure CDN Cache statuscodes](https://msdn.microsoft.com/library/mt759237.aspx).
> 
> 

Een lijst met de statuscodes cache wordt direct boven de grafiek weergegeven. Deze lijst geeft aan dat elke statuscode die kan worden opgenomen in de lijngrafiek en het huidige aantal exemplaren per seconde voor deze statuscode. Een regel wordt standaard weergegeven voor elk van deze statuscodes in de grafiek. U kunt echter alleen de statuscodes waarvoor speciale betekenis voor uw CDN-configuratie te controleren. U doet dit door de gewenste statuscodes controleren en wissen van alle andere opties en klik vervolgens op **grafiek vernieuwen**. 

U kunt tijdelijk logboekgegevens voor een bepaalde statuscode verbergen.  Klik op de statuscode die u wilt verbergen in de legenda direct onder de grafiek. De statuscode wordt onmiddellijk worden verborgen in de grafiek. Die statuscode opnieuw te klikken, wordt deze optie moet opnieuw worden weergegeven.

## <a name="connections"></a>Verbindingen
![Verbindingen grafiek](./media/cdn-real-time-stats/cdn-connections.png)

Deze grafiek geeft aan hoeveel verbindingen zijn gemaakt aan de randservers. Elke aanvraag voor een asset die wordt doorgegeven via onze CDN resulteert in een verbinding.

## <a name="next-steps"></a>Volgende stappen
* Blijf op de hoogte met [realtime waarschuwingen in Azure CDN](cdn-real-time-alerts.md)
* Meer gedetailleerde met dig [geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
* Analyseren [gebruikspatronen](cdn-analyze-usage-patterns.md)

