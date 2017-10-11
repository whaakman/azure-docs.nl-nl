---
title: Azure Service Fabric controle en diagnostische gegevens overzicht | Microsoft Docs
description: Meer informatie over controle en diagnostische gegevens voor Azure Service Fabric-clusters, toepassingen en services.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 88f4a23f89a1c8fd88db1df3a7ff03ae5df64c0f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Controle en diagnostische gegevens voor Azure Service Fabric

Controle en diagnostische gegevens zijn essentieel om te ontwikkelen, testen en implementeren van toepassingen en services in elke omgeving. Service Fabric-oplossingen werken het beste wanneer u plannen en implementeren van controle en diagnostische gegevens waarmee Zorg ervoor toepassingen dat en services werkt zoals verwacht in een lokale ontwikkelingsomgeving of in de productieomgeving.

De belangrijkste doelstellingen van controle en diagnostische gegevens zijn naar:
* Detecteren en onderzoeken van problemen met hardware- en infrastructuur
* Problemen met software- en app detecteren, de service uitvaltijd beperken
* Resource gebruiks- en help station operations beslissingen begrijpen
* Toepassings-, service- en infrastructuur optimaliseren
* Zakelijke inzichten genereren en te identificeren gebieden van de gebruikerservaring


De algemene werkstroom van controle en diagnostische gegevens bestaat uit drie stappen:

1. **Genereren van gebeurtenis**: dit bevat gebeurtenissen (Logboeken, traceringen, aangepaste gebeurtenissen) op de infrastructuur (cluster), platform en toepassing / service niveau
2. **Gebeurtenis aggregatie**: gegenereerde gebeurtenissen moeten worden verzameld en geaggregeerd voordat ze kunnen worden weergegeven
3. **Analyse**: gebeurtenissen moeten worden gevisualiseerde en toegankelijk zijn in sommige indeling, toestaan voor analyse en indien nodig wordt weergegeven

Meerdere producten zijn beschikbaar die betrekking op deze drie gebieden en u bent gratis verschillende technologieën voor elk kiezen. Het is belangrijk om ervoor te zorgen dat de verschillende onderdelen samenwerken voor het leveren van een oplossing voor de end-to-end-controle voor uw cluster.

## <a name="event-generation"></a>Genereren van gebeurtenis

De eerste stap in de werkstroom voor controle en diagnostische gegevens is het maken en het genereren van gebeurtenissen en Logboeken. Deze gebeurtenissen, logboeken en traceringen worden gegenereerd op twee niveaus: de platformlaag (met inbegrip van het cluster, de machines of acties van de Service Fabric) of de toepassingslaag (alle instrumentation toegevoegd aan de apps en services die zijn geïmplementeerd op het cluster). Gebeurtenissen op elk niveau worden aangepast, hoewel Service Fabric een aantal instrumentation standaard biedt.

Lees meer over [platform niveau gebeurtenissen](service-fabric-diagnostics-event-generation-infra.md) en [niveau toepassingsgebeurtenissen](service-fabric-diagnostics-event-generation-app.md) om te begrijpen wat is opgegeven en het toevoegen van meer instrumentation.

Nadat u een beslissing op de logboekregistratie-provider die u wilt gebruiken, moet u controleren of uw logboeken worden samengevoegd en de juiste manier opgeslagen.

## <a name="event-aggregation"></a>Aggregatie van gebeurtenis

Voor het verzamelen van Logboeken en gebeurtenissen die door uw toepassingen en het cluster wordt gegenereerd, meestal aangeraden [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) (meer vergelijkbaar met logboekverzameling op basis van een agent) of [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) () in-process logboekgegevens verzameld).

Verzamelen van toepassingslogboeken met behulp van Azure Diagnostics-extensie is een goede optie voor Service Fabric-services als de set van logboek-bronnen en -doelen niet vaak wordt gewijzigd en er een eenvoudige toewijzing tussen de bronnen en hun doelen is. De reden voor dit is het configureren van Azure Diagnostics vindt plaats op de Resource Manager-laag, zodat u belangrijke wijzigingen doorvoert aan de configuratie vereist updaten/opnieuw distribueren van het cluster. Bovendien best gebruikt om er zeker van te zijn uw logboeken worden opgeslagen op een iets meer permanente van waar ze toegankelijk zijn voor verschillende platforms voor analyse. Dit betekent dat wordt minder efficiënt van een pijplijn terechtkomt dan gaat met de optie zoals EventFlow.

Met behulp van [EventFlow](https://github.com/Azure/diagnostics-eventflow) kunt u de logboeken verzenden rechtstreeks naar een platform voor analyse en visualisatie en/of storage-services hebben. Andere bibliotheken (ILogger, Serilog, enz.) kunnen worden gebruikt voor hetzelfde doel, maar EventFlow heeft het voordeel van een specifiek voor in-process logboekgegevens verzameld en ter ondersteuning van Service Fabric-services die zijn ontworpen. Dit heeft vaak verschillende mogelijke voordelen:

* Eenvoudige configuratie en implementatie
    * De configuratie van verzamelen van diagnostische gegevens net is onderdeel van de serviceconfiguratie. Het is gemakkelijk te altijd synchroon te houden deze "" met de rest van de toepassing
    * De configuratie van per toepassing of per-service is eenvoudig haalbare
    * Alleen een kwestie van het juiste NuGet-pakket toe te voegen en te wijzigen voor het configureren van gegevensbestemmingen via EventFlow is de *eventFlowConfig.json* bestand
* Flexibiliteit
    * De toepassing de gegevens kan verzenden, waar nodig om te gaan, zolang er is een clientbibliotheek die ondersteuning biedt voor het opslagsysteem van gerichte gegevens. Nieuwe doelen kunnen worden toegevoegd naar wens
    * Complexe regels voor het vastleggen, filteren en samenvoegen van gegevens kunnen worden geïmplementeerd
* Toegang tot interne toepassingsgegevens en -context
    * Het diagnostische subsysteem uitvoering binnen het proces van de toepassing/service kan eenvoudig de traceringen met contextafhankelijke informatie verbeteren

Er is alleen Houd er rekening mee dat deze twee opties elkaar niet uitsluiten en het is mogelijk dat dezelfde functie uitgevoerd met behulp van een of andere, het kan ook nuttig voor u voor het instellen van beide. In de meeste gevallen een agent combineren met process-verzameling kan ertoe leiden dat een meer betrouwbare bewaking werkstroom. De extensie Azure Diagnostics (agent) kan worden uw gekozen pad voor platform-niveau logboeken terwijl EventFlow (in-process verzameling) kunt u gebruiken voor uw toepassing niveau Logboeken. Zodra u hebt doorberekend af wat het beste voor u werkt, is het tijd om na te denken over hoe u wilt dat uw gegevens worden weergegeven en geanalyseerd.

## <a name="event-analysis"></a>Analyse van gebeurtenis

Er zijn verschillende geweldige platforms die aanwezig zijn in de markt als het gaat om de analyse- en visualisatie van controle en diagnostische gegevens. De twee die wij adviseren [OMS](service-fabric-diagnostics-event-analysis-oms.md) en [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) vanwege hun beter ook integratie met Service Fabric, maar u moet zoeken naar de [elastische Stack](https://www.elastic.co/products) () met name als u een cluster uitvoert in een offline omgeving overweegt), [Splunk](https://www.splunk.com/), of een andere platform van uw voorkeur.

De belangrijkste punten opgeven die u voor elk platform dat u kiest moeten zijn opgenomen hoe vertrouwd u zich met de gebruikersinterface en query-opties, kunnen gegevens visualiseren en gemakkelijk leesbare dashboards en de aanvullende hulpprogramma's die ze bieden voor het verbeteren van de bewaking maken geautomatiseerd, zoals waarschuwingen.

Naast het platform dat u kiest, bij het instellen van een Service Fabric-cluster als een Azure-resource, krijgt u ook toegang tot Azure out-of-the-box-bewaking voor machines, die nuttig voor specifieke prestaties zijn kunnen en metrische bewaking.

### <a name="azure-monitor"></a>Azure Monitor

U kunt [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) voor het bewaken van veel van de Azure-resources waarop een Service Fabric-cluster is gebouwd. Een set van metrische gegevens voor de [virtuele-machineschaalset](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) en afzonderlijke [virtuele machines](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) automatisch worden verzameld en weergegeven in de Azure-portal. Als u wilt de verzamelde gegevens weergeven in de Azure portal, selecteer de resourcegroep die de Service Fabric-cluster bevat. Selecteer vervolgens de virtuele-machineschaalset die u wilt weergeven. In de **bewaking** sectie **metrische gegevens** om een grafiek van de waarden weer te geven.

![Azure portal weergave van verzamelde metrische gegevens](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

Volg de instructies in voor het aanpassen van de grafieken [metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md). Ook kunt u waarschuwingen op basis van deze metrische gegevens, zoals beschreven in [waarschuwingen in de Azure-Monitor maken voor Azure-services](../monitoring-and-diagnostics/insights-alerts-portal.md). U kunt waarschuwingen verzenden naar een notification-service met behulp van web haken, zoals beschreven in [een haakje web configureren op een Azure metrische waarschuwing](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Monitor voor Azure ondersteunt slechts één abonnement. Als u wilt bewaken van meerdere abonnementen, of als u extra functies, moet [logboekanalyse](https://azure.microsoft.com/documentation/services/log-analytics/), onderdeel van de Microsoft Operations Management Suite biedt een holistische oplossing voor IT-beheer voor on-premises en cloud-gebaseerde infrastructuur. U kunt gegevens van Azure Monitor rechtstreeks naar het Log Analytics versturen, zodat u Logboeken en metrische gegevens voor uw hele omgeving op één plaats kunt zien.

## <a name="next-steps"></a>Volgende stappen

### <a name="watchdogs"></a>Watchdogs

Een watchdog is een afzonderlijke service die u kunt health bekijken en laden tussen services en het rapport status voor alles in de hiërarchie van de health-model. Dit kan helpen voorkomen dat er fouten die niet kunnen worden gedetecteerd op basis van de weergave van een service. Watchdogs zijn ook een goede plaats naar host-code die corrigerende acties zonder tussenkomst van de gebruiker (bijvoorbeeld het opschonen van logboekbestanden in de opslag op bepaalde tijdsintervallen) uitvoert. U vindt een steekproef watchdog service-implementatie [hier](https://github.com/Azure-Samples/service-fabric-watchdog-service).

Aan de slag met informatie over hoe gebeurtenissen en logboeken ophalen gegenereerd tijdens de [platform niveau](service-fabric-diagnostics-event-generation-infra.md) en de [toepassingsniveau](service-fabric-diagnostics-event-generation-app.md).