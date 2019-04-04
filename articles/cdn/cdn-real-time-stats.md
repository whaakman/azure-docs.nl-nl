---
title: Realtime statistieken in Azure CDN | Microsoft Docs
description: Realtime statistieken biedt realtime-gegevens over de prestaties van Azure CDN bij het leveren van inhoud aan uw clients.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: c7989340-1172-4315-acbb-186ba34dd52a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: eb20630533735fb46ea7743be75448329281938a
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916560"
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Realtime statistieken in Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht
Dit document wordt uitgelegd realtime statistieken in Microsoft Azure CDN.  Deze functionaliteit biedt realtime gegevens, zoals bandbreedte, cachestatussen en gelijktijdige verbindingen met uw CDN-profiel bij het leveren van inhoud aan uw clients. Hierdoor kunnen de voortdurende bewaking van de status van uw service op elk gewenst moment, met inbegrip van go live gebeurtenissen.

De volgende grafieken zijn beschikbaar:

* [Bandbreedte](#bandwidth)
* [Statuscodes](#status-codes)
* [Statusbepaling van de cache](#cache-statuses)
* [Verbindingen](#connections)

## <a name="accessing-real-time-stats"></a>Toegang tot realtime statistieken
1. In de [Azure Portal](https://portal.azure.com), blader naar uw CDN-profiel.
   
    ![Blade CDN-profiel](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop blade CDN-profiel beheren](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    De CDN-beheerportal wordt geopend.
3. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **realtime statistieken** flyout.  Klik op **HTTP Large Object**.
   
    ![CDN-beheerportal](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    De grafieken realtime statistieken worden weergegeven.

Elk van de grafieken geeft realtime statistieken voor de geselecteerde periode wordt gestart wanneer de pagina wordt geladen.  De grafieken worden automatisch bijgewerkt om de paar seconden.  De **vernieuwen Graph** knop, indien aanwezig, wordt schakelt u de grafiek, waarna deze alleen de geselecteerde gegevens worden weergegeven.

## <a name="bandwidth"></a>Bandbreedte
![Bandbreedte-grafiek](./media/cdn-real-time-stats/cdn-bandwidth.png)

De **bandbreedte** diagram toont de hoeveelheid bandbreedte die wordt gebruikt voor het huidige platform via de geselecteerde periode. Het gearceerde gedeelte van de grafiek geeft bandbreedtegebruik. De exacte hoeveelheid bandbreedte die momenteel wordt gebruikt, wordt direct onder het lijndiagram weergegeven.

## <a name="status-codes"></a>Statuscodes
![Status code graph](./media/cdn-real-time-stats/cdn-status-codes.png)

De **statuscodes** grafiek geeft aan hoe vaak bepaalde HTTP-responscodes plaatsvinden via de geselecteerde periode.

> [!TIP]
> Zie voor een beschrijving van elke optie voor HTTP-status code [Azure CDN HTTP-statuscodes](/previous-versions/azure/mt759238(v=azure.100)).
> 
> 

Een lijst met HTTP-statuscodes wordt direct boven de grafiek weergegeven. Deze lijst geeft aan dat elke statuscode die kan worden opgenomen in het lijndiagram en het huidige aantal exemplaren per seconde voor deze statuscode. Een regel wordt standaard weergegeven voor elk van deze statuscodes in de grafiek. U kunt echter kiezen voor het bewaken van alleen de statuscodes die speciale betekenis voor uw CDN-configuratie hebben. U doet dit door de gewenste statuscodes controleren en schakelt u alle andere opties en klik vervolgens op **vernieuwen Graph**. 

U kunt tijdelijk gegevens in het logboek voor een bepaalde statuscode verbergen.  Klik op de statuscode die u wilt verbergen in de legenda direct onder de grafiek. De statuscode onmiddellijk verborgen in de grafiek. Nogmaals op te klikken die statuscode zorgt ervoor dat deze optie moet opnieuw worden weergegeven.

## <a name="cache-statuses"></a>Statusbepaling van de cache
![Cache statussen graph](./media/cdn-real-time-stats/cdn-cache-status.png)

De **Cachestatussen** grafiek geeft aan hoe vaak bepaalde typen cachestatussen plaatsvinden via de geselecteerde periode. 

> [!TIP]
> Zie voor een beschrijving van elke optie cache status code [statuscodes voor Azure CDN-Cache](/previous-versions/azure/mt759237(v=azure.100)).
> 
> 

Een lijst van cache-statuscodes wordt direct boven de grafiek weergegeven. Deze lijst geeft aan dat elke statuscode die kan worden opgenomen in het lijndiagram en het huidige aantal exemplaren per seconde voor deze statuscode. Een regel wordt standaard weergegeven voor elk van deze statuscodes in de grafiek. U kunt echter kiezen voor het bewaken van alleen de statuscodes die speciale betekenis voor uw CDN-configuratie hebben. U doet dit door de gewenste statuscodes controleren en schakelt u alle andere opties en klik vervolgens op **vernieuwen Graph**. 

U kunt tijdelijk gegevens in het logboek voor een bepaalde statuscode verbergen.  Klik op de statuscode die u wilt verbergen in de legenda direct onder de grafiek. De statuscode onmiddellijk verborgen in de grafiek. Nogmaals op te klikken die statuscode zorgt ervoor dat deze optie moet opnieuw worden weergegeven.

## <a name="connections"></a>Verbindingen
![Verbindingen graph](./media/cdn-real-time-stats/cdn-connections.png)

Deze grafiek geeft aan hoeveel verbindingen naar uw edge-servers zijn gemaakt. Elke aanvraag voor een asset die wordt doorgegeven via onze CDN-resultaten in een verbinding.

## <a name="next-steps"></a>Volgende stappen
* Blijf op de hoogte met [realtime waarschuwingen in Azure CDN](cdn-real-time-alerts.md)
* Dieper graven met [geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
* Analyseren [gebruikspatronen](cdn-analyze-usage-patterns.md)

