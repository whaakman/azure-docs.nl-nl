---
title: Overzicht van Azure Resource Health | Microsoft Docs
description: Overzicht van Azure Resource Health
services: Resource health
documentationcenter: ''
author: stephbaron
manager: ''
editor: ''
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 11/16/2018
ms.author: stbaron
ms.openlocfilehash: 918ff674e57a14f685c7f9ef2aaaca5ca064976e
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53012841"
---
# <a name="azure-resource-health-overview"></a>Overzicht van Azure Resource Health
 
Azure Resource Health helpt u bij het diagnosticeren en ondersteuning krijgen wanneer een probleem met de Azure-service is van invloed op uw resources. U informeren te over de huidige en eerdere status van uw resources. En het biedt technische ondersteuning om te helpen u bij het beperken van problemen.

Terwijl [Azure Status](https://status.azure.com) u wordt geïnformeerd over problemen die invloed hebben op een breed scala aan Azure-klanten, Resource Health biedt u een gepersonaliseerd dashboard van de status van uw resources. Resource Health ziet u alle keren uw resources in het verleden niet beschikbaar vanwege problemen met Azure-service zijn. Vervolgens wordt het eenvoudig voor u te begrijpen als een SLA wordt geschonden. 

## <a name="resource-definition-and-health-assessment"></a>De evaluatie van de definitie en de status van de resource
Een resource is een specifiek exemplaar van een Azure-service: bijvoorbeeld een virtuele machine, een web-app of een SQL-database.

Resource Health is afhankelijk van signalen verzonden door de verschillende Azure-services om te beoordelen of een resource of niet in orde is. Als een resource niet in orde is, analyseert Resource Health aanvullende informatie om de oorzaak van het probleem vast te stellen. Het identificeert ook acties Microsoft is dat het probleem op te lossen of de acties die u ondernemen kunt om de oorzaak van het probleem. 

Voor meer informatie over hoe status wordt beoordeeld, bekijk de volledige lijst met resourcetypen en statuscontroles in [Azure Resource Health](resource-health-checks-resource-types.md).

## <a name="health-status"></a>De integriteitsstatus van de
De status van een resource wordt weergegeven als een van de volgende statussen.

### <a name="available"></a>Beschikbaar
De status van **beschikbaar** betekent dat de service alle gebeurtenissen die invloed hebben op de status van de resource is niet gevonden. In gevallen waarin de resource is hersteld van niet-geplande uitvaltijd tijdens de afgelopen 24 uur, ziet u de **recent opgeloste problemen met** melding.

![Status van 'Beschikbaar' voor een virtuele machine met een 'Recent opgeloste problemen' melding](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Niet beschikbaar
De status van **niet beschikbaar** betekent dat de service een lopende of een gebeurtenis voor niet-platform die van invloed is op de status van de resource heeft gedetecteerd.

#### <a name="platform-events"></a>Platform-gebeurtenissen
Platform-gebeurtenissen worden geactiveerd door meerdere onderdelen van de Azure-infrastructuur. Ze bevatten zowel geplande acties (bijvoorbeeld: gepland onderhoud) en onverwachte incidenten (bijvoorbeeld een niet-geplande host opnieuw opstarten).

Resource Health biedt extra informatie over de gebeurtenis en het herstelproces. Ook kunt u contact op met ondersteuning, zelfs als u geen een actieve Microsoft ondersteuning voor overeenkomst.

![Status van 'Niet beschikbaar' voor een virtuele machine vanwege een platformgebeurtenis](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a>Niet-platform-gebeurtenissen
Niet-platform gebeurtenissen worden geactiveerd door de acties van gebruikers. Voorbeelden zijn een virtuele machine stoppen of het maximum aantal verbindingen met een Azure-Cache voor Redis bereikt.

![Status van 'Niet beschikbaar' voor een virtuele machine vanwege een niet-platformgebeurtenis](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a>Onbekend
De status van **onbekende** geeft aan dat Resource Health informatie over deze bron voor meer dan 10 minuten nog niet ontvangen. Deze status is niet een definitieve indicatie van de status van de resource, is maar het een belangrijk gegevenspunt in het proces voor het oplossen van problemen.

Als de resource wordt uitgevoerd zoals verwacht, de status van de resource wordt gewijzigd in **beschikbaar** na een paar minuten.

Als u problemen met de resource ondervindt, de **onbekende** integriteitsstatus kan wijzen dat een gebeurtenis in het platform is die betrekking hebben op de resource.

![Status van "Onbekend" voor een virtuele machine](./media/resource-health-overview/Unknown.png)

### <a name="degraded"></a>Verminderd
De status van **gedegradeerd** geeft aan dat de resource een verlies is gedetecteerd in de prestaties, maar dit nog steeds beschikbaar voor gebruik is.
Andere bronnen hebben hun eigen criteria voor wanneer ze opgeeft dat een resource is gedegradeerd.

![Status van 'Gedegradeerd' voor een virtuele machine](./media/resource-health-overview/degraded.png)

## <a name="reporting-an-incorrect-status"></a>Een onjuiste status rapporteren
Als u denkt dat de huidige integriteitsstatus van onjuist is, kunt u ons laten weten door te selecteren **rapporteren onjuiste integriteitsstatus**. In gevallen waarbij een Azure-probleem gevolgen u heeft, raden we u aan contact op met ondersteuning van Resource Health. 

![Vak voor het verzenden van informatie over een onjuiste status](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a>Historische gegevens
U hebt toegang tot maximaal 14 dagen van de geschiedenis van in de **statusgeschiedenis** sectie van de Resource Health. 

![Lijst met Resource Health-gebeurtenissen in de afgelopen twee weken](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a>Aan de slag
Resource Health openen voor een resource:
1.  Meld u aan bij Azure Portal.
2.  Blader naar de resource.
3.  Selecteer op het resourcemenu in het linkerdeelvenster **resourcestatus**.

![Resource Health openen vanuit de weergave van resources](./media/resource-health-overview/from-resource-blade.png)

U kunt ook toegang tot Resource Health hiervoor **alle services** en typen **resourcestatus** in het filtertekstvak. In de **Help en ondersteuning** venster [resourcestatus](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).

![Resource Health uit 'Alle services' openen](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a>Volgende stappen

Bekijk deze bronnen voor meer informatie over Resource Health:
-  [Resourcetypen en statuscontroles in Azure Resource Health](resource-health-checks-resource-types.md)
-  [Veelgestelde vragen over Azure Resource Health](resource-health-faq.md)




