---
title: Overzicht van Azure Resource health | Microsoft Docs
description: Overzicht van Azure-resourcestatus
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/01/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: 9912911d6ed43a70a7a0f9316079d8f66d063f64
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="azure-resource-health-overview"></a>Overzicht van Azure-resource-status
 
Resource Health helpt u bij het diagnosticeren en krijgen van ondersteuning wanneer een Azure-probleem van invloed is op uw resources. Het biedt u informatie over de huidige en eerdere status van uw resources en helpt u problemen verhelpen. Resource Health biedt technische ondersteuning als u hulp nodig heeft bij problemen met Azure-services.

Terwijl [Azure Status](https://status.azure.com) biedt u informatie over problemen met de service die invloed hebben op een uitgebreide reeks Azure-klanten, resourcestatus biedt u een aangepaste dashboard van de status van uw resources. Resourcestatus ziet u alle tijden uw resources niet beschikbaar in het verleden vanwege problemen met de Azure-service zijn, zodat u gemakkelijk kunt begrijpen als een SLA is geschonden. 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a>Wat wordt beschouwd als een resource en hoe gaat resourcestatus besluit als een bron in orde is?
Een bron is een specifiek exemplaar van een Azure-service, bijvoorbeeld: een virtuele machine, een web-app of een SQL-database.

Resourcestatus is afhankelijk van signalen verzonden door de verschillende Azure-services om te bepalen of een resource of niet in orde is. Als een resource niet in orde is, analyseert resourcestatus aanvullende informatie om de oorzaak van het probleem vast te stellen. Identificeert ook acties Microsoft duurt het probleem op te lossen of welke acties u kunt ondernemen om de oorzaak van het probleem. 

Bekijk de volledige lijst met brontypen en health ingecheckt [Azure resourcestatus](resource-health-checks-resource-types.md) voor meer informatie over hoe status wordt beoordeeld.

## <a name="health-status-provided-by-resource-health"></a>Gezondheidsstatus geleverd door de resourcestatus
De status van een bron is een van de volgende statussen:

### <a name="available"></a>Beschikbaar
De service niet alle gebeurtenissen die invloed hebben op de status van de resource niet gedetecteerd. In gevallen waar de resource is hersteld van een niet-geplande uitvaltijd tijdens de afgelopen 24 uur ziet u de **recent is hersteld** melding.

![Bron health beschikbare virtuele machine](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Niet beschikbaar
De service heeft gedetecteerd een lopende platformupdate of niet-platformgebeurtenis die invloed hebben op de status van de resource.

#### <a name="platform-events"></a>Platform-gebeurtenissen
Deze gebeurtenissen worden geactiveerd door meerdere onderdelen van de Azure-infrastructuur. Ze bevatten zowel geplande acties (zoals gepland onderhoud) en onverwachte incidenten (bijvoorbeeld een niet-geplande host opnieuw opstarten).

Resourcestatus biedt aanvullende details over de gebeurtenis en het herstelproces. Ook kunt u contact op met ondersteuning, zelfs als u een actieve Microsoft overeenkomst ondersteunen geen hebt.

![Resource health niet via een virtuele machine vanwege platformgebeurtenis](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Niet-Platform-gebeurtenissen
Deze gebeurtenissen worden geactiveerd door acties die worden uitgevoerd door gebruikers. Bijvoorbeeld: een virtuele machine stoppen of het maximum aantal verbindingen met een Redis-Cache is bereikt.

![Resource health niet via een virtuele machine vanwege niet-platformgebeurtenis](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Onbekend
Deze health-status geeft aan dat resourcestatus informatie over deze bron voor meer dan 10 minuten nog niet ontvangen. Deze status niet een definitieve indicatie van de status van de bron, zijn maar er een punt belangrijke gegevens in het proces voor probleemoplossing:
* Als de bron wordt uitgevoerd zoals verwacht, wordt de status van de resource op beschikbaar na een paar minuten bijgewerkt.
* Als u problemen met de bron ondervindt, kan de status onbekend voorgesteld dat de bron is van invloed op een gebeurtenis in het platform.

![Bron health onbekende virtuele machine](./media/resource-health-overview/Unknown.png)

### <a name="degraded"></a>Verminderd
Deze health-status geeft aan dat uw resource verlies is gedetecteerd in de prestaties, hoewel het nog steeds beschikbaar voor gebruik.
Andere bronnen hebben hun eigen criteria voor bij het opgeven van dat een bron is gedegradeerd.

![Resourcestatus gedegradeerd virtuele machine](./media/resource-health-overview/degraded.png)

## <a name="report-an-incorrect-status"></a>Een onjuiste status rapporteren
Als u de huidige status is onjuist op elk gewenst moment denkt, kunt u laat ons weten door te klikken op **rapporteren onjuist gezondheidsstatus**. In gevallen waarin u worden beïnvloed door een probleem met de Azure, raden we u aan contact op met ondersteuning van resourcestatus. 

![Resourcestatus rapport onjuiste Status](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Historische gegevens
U kunt maximaal 14 dagen van de geschiedenis van openen door te klikken op **Historie weergeven** in resourcestatus. 

![Resourcestatus rapportgeschiedenis](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Aan de slag
Status van resources voor één resource openen
1.  Meld u aan bij de Azure portal.
2.  Navigeer naar de resource.
3.  Klik in het menu resource zich in de linkerkant op **resourcestatus**.

![Resourcestatus openen vanuit Resource weergeven](./media/resource-health-overview/from-resource-blade.png)

U kunt de resourcestatus ook openen door te klikken op **alle services**, en typ **resourcestatus** in het filtertekstvak openen de **Help + ondersteuning** blade. Klik tot slot [ **resourcestatus**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Open de resourcestatus van alle service](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Volgende stappen

Bekijk deze resources voor meer informatie over de resourcestatus:
-  [Status van resourcetypen en controleert in Azure resourcestatus](resource-health-checks-resource-types.md)
-  [Veelgestelde vragen over Azure-resource-status](resource-health-faq.md)




