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
ms.openlocfilehash: 040d58a81a9b41fe660e4276d698bf884f90bb6c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-resource-health-overview"></a>Overzicht van Azure-resource-status
 
Resource Health helpt u bij het diagnosticeren en krijgen van ondersteuning wanneer een Azure-probleem van invloed is op uw resources. Het biedt u informatie over de huidige en eerdere status van uw resources en helpt u problemen verhelpen. Resource Health biedt technische ondersteuning als u hulp nodig heeft bij problemen met Azure-services.

Terwijl [Azure Status](https://status.azure.com) biedt u informatie over problemen met de service die invloed hebben op een uitgebreide reeks Azure-klanten, resourcestatus biedt u een aangepaste dashboard van de status van uw resources. Resourcestatus ziet u alle tijden uw resources niet beschikbaar in het verleden vanwege problemen met de Azure-service zijn. Dit maakt het eenvoudig om te begrijpen als een SLA is geschonden. 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a>Wat wordt beschouwd als een resource en hoe gaat resourcestatus besluit als een bron in orde is?
Een bron is een exemplaar van een resourcetype die worden aangeboden door een Azure-service via Azure Resource Manager, bijvoorbeeld: een virtuele machine, een web-app of een SQL-database.

Resourcestatus is afhankelijk van signalen verzonden door de verschillende Azure-services te beoordelen of een resource in orde of niet is. Als een resource niet in orde is, analyseert resourcestatus aanvullende informatie om de oorzaak van het probleem vast te stellen. Identificeert ook acties Microsoft duurt het probleem op te lossen of welke acties u kunt ondernemen om de oorzaak van het probleem. 

Bekijk de volledige lijst met brontypen en health ingecheckt [Azure resourcestatus](resource-health-checks-resource-types.md) voor meer informatie over hoe status wordt beoordeeld.

## <a name="health-status-provided-by-resource-health"></a>Gezondheidsstatus geleverd door de resourcestatus
De status van een bron is een van de volgende statussen:

### <a name="available"></a>Beschikbaar
De service heeft alle gebeurtenissen die invloed hebben op de status van de resource niet gevonden. In gevallen waar de resource is hersteld van een niet-geplande uitvaltijd tijdens de afgelopen 24 uur ziet u hoe de **recent is hersteld** melding.

![Bron health beschikbare virtuele machine](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Niet beschikbaar
De service heeft gedetecteerd een lopende platformupdate of niet-platformgebeurtenis die invloed hebben op de status van de resource.

#### <a name="platform-events"></a>Platform-gebeurtenissen
Deze gebeurtenissen worden geactiveerd door meerdere onderdelen van de Azure-infrastructuur en omvatten zowel geplande acties zoals gepland onderhoud en onverwachte incidenten, zoals een niet-geplande host opnieuw worden opgestart.

Resourcestatus biedt aanvullende details over de gebeurtenis, het herstelproces en kunt u contact op met ondersteuning, zelfs als u een actieve Microsoft overeenkomst ondersteunen geen hebt.

![Resource health niet via een virtuele machine vanwege platformgebeurtenis](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Niet-Platform-gebeurtenissen
Deze gebeurtenissen worden geactiveerd door acties die worden uitgevoerd door gebruikers, bijvoorbeeld een virtuele machine stoppen of het maximum aantal verbindingen met een Redis-Cache is bereikt.

![Resource health niet via een virtuele machine vanwege niet-platformgebeurtenis](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Onbekend
Deze health-status geeft aan dat resourcestatus informatie over deze bron voor meer dan 10 minuten niet heeft ontvangen. Deze status is niet een definitieve indicatie van de status van de bron, zijn maar er een punt belangrijke gegevens in het proces voor probleemoplossing:
* Als de bron wordt uitgevoerd zoals verwacht de status van de resource wordt bijgewerkt naar beschikbaar na enkele minuten duren.
* Als u problemen met de bron ondervindt, kan de status onbekend voorgesteld dat de bron is van invloed op een gebeurtenis in het platform.

![Bron health onbekende virtuele machine](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a>Een onjuiste status rapporteren
Als u de huidige status is onjuist op elk gewenst moment denkt, kunt u laat ons weten door te klikken op **rapporteren onjuist gezondheidsstatus**. In gevallen waarin u worden beïnvloed door een probleem met de Azure, raden we u aan contact op met ondersteuning van de resourceblade van de status. 

![Resourcestatus rapport onjuiste Status](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Historische gegevens
U kunt maximaal 14 dagen van historische gegevens openen door te klikken op **Historie weergeven** in de resourceblade van de status. 

![Resourcestatus rapportgeschiedenis](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Aan de slag
Status van resources voor één resource openen
1.  Meld u aan bij de Azure portal.
2.  Navigeer naar de resource.
3.  Klik in het menu resource zich in de linkerkant op **resourcestatus**.

![Open resourcestatus van Resource-blade](./media/resource-health-overview/from-resource-blade.png)

U kunt de resourcestatus ook openen door te klikken op **meer services**, en typ **resourcestatus** in het filtertekstvak openen de **Help + ondersteuning** blade. Klik tot slot [ **resourcestatus**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Resourcestatus openen vanuit meer service](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Volgende stappen

Bekijk deze resources voor meer informatie over de resourcestatus:
-  [Status van resourcetypen en controleert in Azure resourcestatus](resource-health-checks-resource-types.md)
-  [Veelgestelde vragen over Azure-resource-status](resource-health-faq.md)




