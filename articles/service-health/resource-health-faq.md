---
title: Azure Resource Health Veelgestelde vragen over | Microsoft Docs
description: Overzicht van Azure Resource Health
author: stephbaron
ms.author: stbaron
ms.topic: conceptual
ms.service: service-health
ms.date: 01/29/2019
ms.workload: Supportability
ms.openlocfilehash: c0de42935c1e46fd51e82a58083f85f6cee83ce3
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884534"
---
# <a name="azure-resource-health-faq"></a>Azure Resource Health Veelgestelde vragen
Meer informatie over de antwoorden op veelgestelde vragen over Azure Resource Health.

## <a name="what-is-azure-resource-health"></a>Wat is Azure Resource Health?
Resource Health helpt u bij het diagnosticeren en krijgen van ondersteuning wanneer een Azure-probleem van invloed is op uw resources. Het biedt u informatie over de huidige en eerdere status van uw resources en helpt u problemen verhelpen. Resource Health biedt technische ondersteuning als u hulp nodig heeft bij problemen met Azure-services.  

## <a name="what-is-the-resource-health-intended-for"></a>Wat is de resourcestatus bedoeld?
Zodra een probleem met een resource is aangetroffen, kunt Resource Health u achterhalen wat de hoofdoorzaak. Het biedt Help-informatie om het probleem op en technische ondersteuning als u meer hulp nodig bij problemen met Azure-service.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Welke statuscontroles worden uitgevoerd door de Resource Health?
Resourcestatus voert diverse controles op basis van de [resourcetype](resource-health-checks-resource-types.md). Deze controles zijn ontworpen voor het implementeren van drie typen problemen: 
- Niet-geplande gebeurtenissen, bijvoorbeeld een hostserver onverwacht opnieuw opstarten
- Geplande gebeurtenissen, zoals hostbesturingssysteem geplande updates
- Gebeurtenissen die worden geactiveerd door acties van de gebruiker, bijvoorbeeld een gebruiker een virtuele machine opnieuw opstarten

## <a name="what-does-each-of-the-health-status-mean"></a>Wat betekent elk van de status?
Er zijn drie verschillende health-statussen:
- Beschikbaar: Er zijn geen bekende problemen in de Azure-platform dat kan worden invloed heeft op deze resource
- Niet beschikbaar: Resourcestatus heeft gedetecteerd problemen die de resource van invloed zijn op
- Onbekend: Resourcestatus kan de status van een resource niet bepalen omdat deze informatie ontvangt over deze is gestopt. 

## <a name="what-does-the-unknown-status-mean-is-something-wrong-with-my-resource"></a>Wat houdt de status onbekend? Er is iets mis met mijn resource?
De status is ingesteld op Onbekend als Resource Health stopt informatie ontvangt over een specifieke resource. Terwijl deze status is niet een definitieve indicatie van de status van de resource, in gevallen waarbij u problemen ondervindt waarvan, kan dit betekenen dat er is een probleem met Azure.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Hoe kan ik hulp krijgen voor een resource die niet beschikbaar is?
U kunt een aanvraag voor ondersteuning van de Resource Health-blade indienen. U hoeft niet een ondersteuningsovereenkomst met Microsoft om te vragen wanneer de resource niet beschikbaar is omdat platform gebeurtenissen.

## <a name="does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did"></a>Resource Health onderscheid maken tussen de onbeschikbaarheid van problemen voor het platform versus iets die ik deed-indeling?
Ja, wanneer een resource niet beschikbaar is, identificeert Resource Health de hoofdoorzaak binnen een van deze categorieën: 
-   Door de gebruiker geïnitieerde actie
-   Geplande gebeurtenis 
-   Niet-gepland voorval

De gebruiker geïnitieerde acties worden weergegeven met een blauw pictogram tijdens geplande en ongeplande gebeurtenissen worden weergegeven met een rood waarschuwingspictogram weergegeven in de portal. Meer informatie vindt u in de [Resource Health overzicht](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>Integreer ik Resource Health met mijn controleprogramma's?
Resource health is een service waarmee u kunt vaststellen en oplossen van problemen met Azure-service die van invloed zijn op uw resources. Hoewel de Resource Health API kunt u de integriteitsstatus van via een programma te verkrijgen, adviseren we dat u metrische gegevens gebruiken voor het bewaken van uw resources. Zodra er een probleem wordt gedetecteerd, worden de Resource Health helpt u bij het bepalen van de hoofdoorzaak en leidt u door acties voor deze. Ga naar [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) voor meer informatie over hoe u metrische gegevens gebruiken kunt om te controleren of uw resources.

## <a name="where-do-i-find-resource-health"></a>Waar vind ik Resource Health?
Nadat u zich hebt aangemeld bij Azure portal, zijn er meerdere manieren waarop u hebt toegang tot Resource Health:
- Navigeer naar uw resource. Selecteer in de navigatiebalk links **Resource health**
- Ga naar de Azure Service Health-blade.  Selecteer in de navigatiebalk links **resourcestatus**.
- Open de **Help en ondersteuning** blade door het vraagteken selecteren in de rechterbovenhoek van de portal en selecteer vervolgens **Help en ondersteuning**. Zodra de blade wordt geopend, selecteert u **Resource health**

U kunt ook de Resource Health API gebruiken om informatie over de status van uw resources te verkrijgen.

## <a name="is-resource-health-available-for-all-resource-types"></a>Resource Health beschikbaar is voor alle resourcetypen?
De lijst met statuscontroles en resourcetypen ondersteund via Resource Health vindt [hier](resource-health-checks-resource-types.md).

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Wat moet ik doen als mijn resource wordt weergegeven als beschikbaar, maar ik dat dit niet?"
Tijdens het controleren van de status van een resource, direct onder de status, kunt u **rapporteren onjuiste integriteitsstatus**. Voordat u het rapport verzendt, hebt u de mogelijkheid van het leveren van aanvullende informatie op waarom u denkt dat ze dat de huidige status is onjuist.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Resource Health beschikbaar is voor alle Azure-regio's? 
Resource health is beschikbaar in voor alle geografische gebieden voor Azure, met uitzondering van de volgende regio's:
- VS (overheid) - Virginia
- US Gov - Iowa
- US DoD - oost
- US DoD - centraal
- Duitsland - centraal
- Duitsland - noordoost

## <a name="how-is-resource-health-different-from-azure-status-or-the-service-health-dashboard"></a>Wat is Resource Health verschil met Azure-status of de Service Health-dashboard?
De informatie die door Resource Health is ook specifieker dan wat wordt verstrekt door Azure status of de Service Health-dashboard.

Terwijl [Azure status](https://status.azure.com) en de Service Health dashboard voor u wordt geïnformeerd over problemen met de service die invloed hebben op een breed scala aan klanten (bijvoorbeeld een Azure-regio), Resource Health beschrijft gedetailleerdere gebeurtenissen die alleen relevant zijn voor de specifieke resource. Bijvoorbeeld, als een host onverwacht opnieuw is opgestart, waarschuwingen Resource Health alleen die klanten waarvan de virtuele machines werden uitgevoerd op die host.

Het is belangrijk dat u ziet dat biedt volledige zichtbaarheid van gebeurtenissen die invloed hebben op uw resources, Resource Health toont ook gebeurtenissen die zijn gepubliceerd in het dashboard status van de Service.

## <a name="do-i-need-to-activate-resource-health-for-each-resource"></a>Moet ik de resourcestatus voor elke resource activeren?
Nee, de statusinformatie is beschikbaar voor alle brontypen die beschikbaar zijn via Resource Health. 

## <a name="do-we-need-to-enable-resource-health-for-my-organization"></a>Moeten we Resource Health inschakelen voor mijn organisatie?
Nee.  Azure Resource Health is toegankelijk vanuit Azure portal zonder een setup-vereisten.

## <a name="is-resource-health-available-free-of-charge"></a>Resource Health beschikbaar is gratis?
Ja.  Azure Resource Health is gratis.

## <a name="what-are-the-recommendations-that-resource-health-provides"></a>Wat zijn de aanbevelingen die Resource Health biedt?
Op basis van de status, biedt Resource Health u aanbevelingen met het doel van de tijd die u besteed aan het oplossen van problemen. Voor de beschikbare resources, de focus aanbevelingen over het oplossen van de meest voorkomende problemen die klanten ondervinden. Als de resource niet beschikbaar vanwege een niet-gepland voorval van Azure is, wordt de focus zich op u te helpen tijdens en na het herstelproces. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Resource Health:
-  [Overzicht van Azure Resource Health](Resource-health-overview.md)
-  [Resourcetypen en statuscontroles die beschikbaar zijn via Azure Resource Health](resource-health-checks-resource-types.md)
