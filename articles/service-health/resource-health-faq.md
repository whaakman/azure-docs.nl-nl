---
title: Azure resourcestatus Veelgestelde vragen | Microsoft Docs
description: Overzicht van Azure resourcestatus
services: Resource health
documentationcenter: dev-center-name
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/05/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: 692d03ad80989a969a73b15bb9042a01c286d1ec
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="azure-resource-health-faq"></a>Azure resourcestatus Veelgestelde vragen
Meer informatie over de antwoorden op veelgestelde vragen over Azure-resourcestatus.

## <a name="what-is-azure-resource-health"></a>Wat is Azure resourcestatus?
Resource Health helpt u bij het diagnosticeren en krijgen van ondersteuning wanneer een Azure-probleem van invloed is op uw resources. Het biedt u informatie over de huidige en eerdere status van uw resources en helpt u problemen verhelpen. Resource Health biedt technische ondersteuning als u hulp nodig heeft bij problemen met Azure-services.  

## <a name="what-is-the-resource-health-intended-for"></a>Wat is de resourcestatus bedoeld?
Zodra er een probleem met een bron is gedetecteerd, kunt resourcestatus u de hoofdoorzaak onderzoeken. Help voor het beperken van het probleem en de technische ondersteuning als u meer hulp nodig bij problemen met de Azure service biedt.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Welke controles worden uitgevoerd door de resourcestatus?
Resourcestatus voert diverse controles op basis van de [brontype](resource-health-checks-resource-types.md). Deze controles zijn ontworpen voor het implementeren van de drie typen problemen: 
- Niet-geplande gebeurtenissen, bijvoorbeeld een onverwachte host opnieuw worden opgestart
- Geplande gebeurtenissen, zoals geplande hostbesturingssysteem updates
- Gebeurtenissen die worden geactiveerd door acties van gebruikers, bijvoorbeeld een gebruiker een virtuele machine opnieuw opstarten

## <a name="what-does-each-of-the-health-status-mean"></a>Wat betekent elk van de status?
Er zijn drie verschillende health-statussen:
- Beschikbaar: Er zijn geen bekende problemen in de Azure-platform die kan van invloed op deze bron
- Niet beschikbaar: Resourcestatus heeft gevonden problemen die de bron van invloed zijn op
- Onbekende: Resourcestatus kan niet bepalen de status van een resource omdat deze informatie over deze ontvangst is gestopt. 

## <a name="what-does-the-unknown-status-mean-is-something-wrong-with-my-resource"></a>Wat houdt de status onbekend? Er is iets mis met mijn resource?
De status is ingesteld op Onbekend wanneer resourcestatus ontvangt geen informatie over een specifieke bron. Terwijl deze status is niet een definitieve indicatie van de status van de bron, in gevallen waar zich problemen voordoen, kan dit duiden dat er is een probleem met Azure.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Hoe krijg hulp voor een resource die niet beschikbaar is?
U kunt een verzoek om ondersteuning op de blade resourcestatus verzenden. U hoeft niet een support-overeenkomst met Microsoft om te vragen wanneer de bron niet beschikbaar is omdat de platform-gebeurtenissen.

## <a name="does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did"></a>Resourcestatus onderscheid maken tussen niet beschikbaar zijn geïntegreerd met problemen versus iets die ik deed-platform?
Wanneer een bron niet beschikbaar is, identificeert resourcestatus Ja, de hoofdoorzaak in een van deze categorieën: 
-   Gebruiker gestarte actie
-   Geplande gebeurtenis 
-   Niet-geplande gebeurtenis

In de portal worden acties van gebruikers die zijn gestart met een blauw pictogram tijdens geplande en ongeplande gebeurtenissen worden weergegeven met behulp van een rood waarschuwingspictogram weergegeven. Meer informatie vindt u in de [resourcestatus overzicht](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>Kan ik resourcestatus worden geïntegreerd met mijn controleprogramma's?
De resourcestatus is een service waarmee u kunt onderzoeken en verhelpen van problemen met de Azure-service die invloed zijn op uw resources. Terwijl de Resource Health API kunt u via programmacode verkrijgen van de status, wordt u aangeraden dat u metrische gegevens voor het bewaken van uw resources gebruiken. Zodra er een probleem is gedetecteerd, worden de resourcestatus kunt u de hoofdoorzaak vaststellen en leidt u door acties deze. Ga naar [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) voor meer informatie over hoe u metrische gegevens kunt om te controleren van uw resources.

## <a name="where-do-i-find-resource-health"></a>Waar vind ik resourcestatus
Nadat u zich bij de Azure-portal aanmelden, zijn er meerdere manieren u toegang hebt tot resourcestatus:
- Navigeer naar de resource. Selecteer in de linkernavigatiebalk **resourcestatus**
- Ga naar de blade van Azure-Monitor.  Selecteer in de linkernavigatiebalk **resourcestatus**.
- Open de **Help + ondersteuning** blade door het vraagteken selecteren in de rechterbovenhoek van de portal **Help + ondersteuning**. Zodra de blade wordt geopend, selecteert u **resourcestatus**

U kunt ook de bron Health-API gebruiken om informatie over de status van uw resources te verkrijgen.

## <a name="is-resource-health-available-for-all-resource-types"></a>Resourcestatus beschikbaar is voor alle brontypen?
De lijst met statuscontroles en resourcetypen ondersteund via resourcestatus vindt [hier](resource-health-checks-resource-types.md).

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Wat moet ik doen als mijn resource wordt weergegeven als beschikbaar, maar ik zijn ervan overtuigd dat het niet?"
Bij het controleren van de status van een resource direct onder de status kunt u **rapporteren onjuist gezondheidsstatus**. Vóór het verzenden van het rapport, hebt u de mogelijkheid biedt aanvullende details over waarom u van mening bent dat de huidige status is onjuist.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Resourcestatus beschikbaar is voor alle Azure-regio's? 
De resourcestatus is beschikbaar in over alle Azure geografische gebieden met uitzondering van de volgende gebieden:
- VS (overheid) - Virginia
- VS (overheid) - Iowa
- US DoD - oost
- US DoD - centraal
- Duitsland - centraal
- Duitsland - noordoost

## <a name="how-is-resource-health-different-from-the-service-health-dashboard-or-the-azure-portal-service-notifications"></a>Hoe wordt resourcestatus van het Service Health Dashboard of de Azure portal servicemeldingen?
De informatie die door de resourcestatus is specifieker dan is opgegeven door de Azure Service Health Dashboard.

Terwijl [Azure Status](https://status.azure.com) en de portal servicemeldingen u wordt geïnformeerd over problemen met de service die invloed hebben op een uitgebreide reeks klanten (bijvoorbeeld een Azure-regio), resourcestatus beschrijft gedetailleerdere gebeurtenissen die alleen relevant zijn voor de bepaalde resource. Als een host onverwacht opnieuw wordt opgestart waarschuwingen resourcestatus alleen klanten waarvan virtuele machines zijn uitgevoerd op die host.

Het is belangrijk dat u ziet dat bieden u een compleet overzicht van de gebeurtenissen die invloed hebben op uw resources, resourcestatus geeft ook weer voor gebeurtenissen die zijn gepubliceerd in het Dashboard van de Health-Service en Service-meldingen.

## <a name="do-i-need-to-activate-resource-health-for-each-resource"></a>Moet ik resourcestatus voor elke resource activeren?
Nee, de statusgegevens zijn beschikbaar voor alle resourcetypen beschikbaar via de resourcestatus. 

## <a name="do-we-need-to-enable-resource-health-for-my-organization"></a>Moet ik resourcestatus inschakelen voor mijn organisatie?
Nee.  Azure resourcestatus is toegankelijk vanuit de Azure-portal zonder eventuele installatievereisten.

## <a name="is-resource-health-available-free-of-charge"></a>Resourcestatus beschikbaar is gratis?
Ja.  Azure resourcestatus is gratis.

## <a name="what-are-the-recommendations-that-resource-health-provides"></a>Wat zijn de aanbevelingen die resourcestatus biedt?
Op basis van de status, biedt resourcestatus u aanbevelingen met het doel van het minder tijd u besteed aan het oplossen van problemen. Voor beschikbare resources ondervinden de focus aanbevelingen over het oplossen van de meest voorkomende problemen-klanten. Als de bron niet beschikbaar vanwege een Azure niet-geplande gebeurtenis is, worden de focus op die u helpt tijdens en na het herstelproces. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de resourcestatus:
-  [Overzicht van Azure resourcestatus](Resource-health-overview.md)
-  [Resourcetypen en statuscontroles die beschikbaar zijn via Azure Resource Health](resource-health-checks-resource-types.md)
