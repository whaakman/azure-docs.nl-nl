---
title: Overzicht van Azure resourcestatus | Microsoft Docs
description: Overzicht van Azure resourcestatus
services: Resource health
documentationcenter: ''
author: shawntabrizi
manager: ''
editor: ''
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 03/27/2018
ms.author: shawn.tabrizi
ms.openlocfilehash: 99e996f182aac774f2e2565d87fd0debaba1b2d1
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
# <a name="azure-resource-health-overview"></a>Overzicht van Azure resourcestatus
 
Azure Resource Health helpt u bij het diagnosticeren en ondersteuning als het probleem van een Azure-service is van invloed op uw resources. Informeert u over de huidige en eerdere status van uw resources. En biedt technische ondersteuning om te helpen u bij het beperken van problemen.

Terwijl [Azure Status](https://status.azure.com) biedt u informatie over problemen die invloed hebben op een uitgebreide reeks Azure-klanten, resourcestatus biedt u een aangepaste dashboard van de status van uw resources. Resourcestatus ziet u alle tijden uw resources niet beschikbaar in het verleden vanwege problemen met Azure-service zijn. Vervolgens wordt het eenvoudig om te begrijpen als een SLA is geschonden. 

## <a name="resource-definition-and-health-assessment"></a>Definitie- en beoordeling van resource
Een bron is een specifiek exemplaar van een Azure-service: bijvoorbeeld een virtuele machine, een web-app of een SQL-database.

Resourcestatus is afhankelijk van signalen verzonden door de verschillende Azure-services om te bepalen of een resource of niet in orde is. Als een resource niet in orde is, analyseert resourcestatus aanvullende informatie om de oorzaak van het probleem vast te stellen. Identificeert ook acties Microsoft is dat het probleem op te lossen of de acties die u ondernemen kunt om de oorzaak van het probleem te verhelpen. 

Voor meer informatie over hoe status wordt beoordeeld, de volledige lijst met brontypen bekijken en health ingecheckt [Azure resourcestatus](resource-health-checks-resource-types.md).

## <a name="health-status"></a>Health-status
De status van een resource wordt weergegeven als een van de volgende statussen.

### <a name="available"></a>Beschikbaar
De status van **beschikbaar** betekent dat de service alle gebeurtenissen die betrekking hebben op de status van de resource is niet gevonden. In gevallen waar de resource is hersteld van een niet-geplande uitvaltijd tijdens de afgelopen 24 uur, ziet u de **recent opgeloste problemen met** melding.

![Status van 'Beschikbaar' voor een virtuele machine met een 'Recent opgeloste problemen' melding](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Niet beschikbaar
De status van **niet beschikbaar** betekent dat de service een lopende platformupdate of niet-platform-gebeurtenis die invloed heeft op de status van de resource heeft gevonden.

#### <a name="platform-events"></a>Platform-gebeurtenissen
Platform gebeurtenissen worden geactiveerd door meerdere onderdelen van de Azure-infrastructuur. Ze bevatten zowel geplande acties (bijvoorbeeld gepland onderhoud) en onverwachte incidenten (bijvoorbeeld een niet-geplande host opnieuw opstarten).

Resourcestatus biedt aanvullende details over de gebeurtenis en het herstelproces. Ook kunt u contact op met ondersteuning, zelfs als u een actieve Microsoft overeenkomst ondersteunen geen hebt.

![Status van 'Niet beschikbaar' voor een virtuele machine vanwege een platformgebeurtenis](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Niet-platform-gebeurtenissen
Niet-platform gebeurtenissen worden geactiveerd door acties van gebruikers. Voorbeelden zijn als u een virtuele machine of het maximum aantal verbindingen met een Redis-cache is bereikt.

![Status van 'Niet beschikbaar' voor een virtuele machine vanwege een niet-platform-gebeurtenis](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Onbekend
De status van **onbekende** geeft aan dat resourcestatus informatie over deze bron voor meer dan 10 minuten nog niet ontvangen. Deze status niet een definitieve indicatie van de status van de bron, is maar het een belangrijke gegevenspunt in het proces voor het oplossen van problemen.

Als de bron wordt uitgevoerd zoals verwacht, de status van de bron wordt gewijzigd in **beschikbaar** na een paar minuten.

Als u problemen met de bron ondervindt, de **onbekende** gezondheidsstatus kan wijzen dat een gebeurtenis in het platform invloed op de resource.

![Status van 'Onbekend' voor een virtuele machine](./media/resource-health-overview/Unknown.png)

### <a name="degraded"></a>Verminderd
De status van **gedegradeerd** geeft aan dat de resource een verlies is gedetecteerd in prestaties, hoewel het nog steeds beschikbaar voor gebruik.
Andere bronnen hebben hun eigen criteria voor wanneer ze opgeven dat een bron is gedegradeerd.

![Status van 'Gedegradeerd' voor een virtuele machine](./media/resource-health-overview/degraded.png)

## <a name="reporting-an-incorrect-status"></a>Melden van een onjuiste status
Als u denkt dat de huidige status onjuist is, kunt u laat ons weten als u selecteert **rapporteren onjuist gezondheidsstatus**. In gevallen waar een probleem met de Azure invloed op u, raden we u aan contact op met ondersteuning van resourcestatus. 

![Vak voor het indienen van informatie over een onjuiste status](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Historische gegevens
U toegang hebt tot 14 dagen van de geschiedenis van in de **statusgeschiedenis** sectie van de status van resources. 

![Lijst van gebeurtenissen die gedurende de afgelopen twee weken resourcestatus](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Aan de slag
Status van resources voor één resource openen:
1.  Meld u aan bij Azure Portal.
2.  Blader naar de resource.
3.  Selecteer in het menu resource in het linkerdeelvenster **resourcestatus**.

![Resourcestatus openen vanuit de weergave van resources](./media/resource-health-overview/from-resource-blade.png)

U kunt de resourcestatus ook openen door te selecteren **alle services** en typen **resourcestatus** in het tekstvak filter. In de **Help + ondersteuning** deelvenster [resourcestatus](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Het openen van de resourcestatus van 'Alle services'](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Volgende stappen

Bekijk deze resources voor meer informatie over de resourcestatus:
-  [Resourcetypen en health controleert in Azure-resourcestatus](resource-health-checks-resource-types.md)
-  [Veelgestelde vragen over Azure-resourcestatus](resource-health-faq.md)




