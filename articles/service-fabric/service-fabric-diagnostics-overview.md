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
ms.date: 01/10/2018
ms.author: dekapur
ms.openlocfilehash: 43a45a31efffcd623e6381049876c3607663ec4f
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Controle en diagnostische gegevens voor Azure Service Fabric

Dit artikel bevat een overzicht van het instellen van controle en diagnostische gegevens van uw toepassingen die worden uitgevoerd in Service Fabric-clusters. Controle en diagnostische gegevens zijn essentieel om te ontwikkelen, testen en implementeren van werkbelastingen in een cloudomgeving. Bewaking, kunt u bijhouden hoe uw toepassingen worden gebruikt, het brongebruik en de algehele status van het cluster. U kunt deze informatie gebruiken voor het opsporen en corrigeren van problemen in het cluster en om te voorkomen dat er problemen optreden in de toekomst. 

De belangrijkste doelstellingen van controle en diagnostische gegevens zijn naar:
* Detecteren en onderzoeken van problemen met de infrastructuur
* Problemen met uw toepassing detecteren
* Brongebruik begrijpen
* Prestaties van de toepassing en service-infrastructuur

In een Service Fabric-cluster, controle en diagnostische gegevens afkomstig zijn van de drie niveaus: toepassing, het platform (cluster) en infrastructuur. 
* De [toepassingsniveau](service-fabric-diagnostics-event-generation-app.md) bevat gegevens over de prestaties van uw toepassingen en eventuele aanvullende aangepaste logboekregistratie die u hebt toegevoegd. Bewakingsgegevens van toepassing moet eindigen in Application Insights (AI) van de toepassing zelf. Deze kan worden geleverd via de SDK AI, EventFlow of een andere pijplijn van uw keuze.
* De [platform niveau](service-fabric-diagnostics-event-generation-infra.md) gebeurtenissen uit acties worden uitgevoerd op uw cluster, gerelateerd aan het beheer van het cluster en de toepassingen die erop worden uitgevoerd. Platform bewakingsgegevens moeten naar OMS Log Analytics, met de Service Fabric Analytics-oplossing om u te helpen bij het visualiseren van binnenkomende gebeurtenissen worden verzonden. Deze gegevens worden meestal verzonden naar een opslagaccount via de extensie Windows of Linux Azure Diagnostics uit waarop deze is geopend door OMS Log Analytics. 
* Het niveau van de infrastructuur is gericht op [prestatiebewaking](service-fabric-diagnostics-event-generation-perf.md)Zoek op de belangrijkste metrische gegevens en prestatiemeteritems om te bepalen brongebruik en laden. Bewaking van toepassingsprestaties kan worden bereikt met behulp van een agent - wordt u aangeraden de Agent OMS (Microsoft Monitoring), zodat u prestatiegegevens op dezelfde plaats als uw platform-gebeurtenissen, waardoor een betere ervaring voor diagnostische gegevens belandt als u correleren wijzigingen in een cluster. 

![Diagnostische gegevens overzicht grafiek](media/service-fabric-diagnostics-overview/diagnostics-overview.png)

## <a name="monitoring-scenarios"></a>Bewakingsscenario 's

Deze sectie worden de belangrijkste scenario's voor het bewaken van een Service Fabric-cluster - toepassing, cluster, prestaties en statuscontrole behandeld. Voor elk scenario, wordt de opzet en de algehele benadering voor de bewaking besproken. Meer informatie over deze en andere algemene bewaking aanbevelingen voor de Azure-resources kunnen worden gevonden op [Best Practices - controle en diagnostische gegevens](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 

Deze scenario's zijn ook los toegewezen aan de drie niveaus van controle en diagnostische gegevens, zoals hierboven, dat wil zeggen beschreven voor elk scenario moet op de juiste wijze worden verwerkt in het cluster, moet u de controle en diagnostische gegevens die afkomstig zijn op het bijbehorende niveau hebben . Voor de health monitoring scenario is een uitzondering omdat deze omdat het cluster en alles in het uitgevoerd.

## <a name="application-monitoring"></a>Toepassingen bewaken
Toepassingsbewaking houdt hoe functies en onderdelen van een toepassing die u hebt gemaakt, worden gebruikt. U wilt bewaken van uw toepassingen en zorg ervoor dat problemen die gebruikers zijn opgepikt impact. Bewaking van uw toepassingen kan worden gebruikt voor:
* het bepalen van laden van toepassingen en gebruiker verkeer - moet u uw services om te voldoen aan de eisen van de gebruiker of een potentiële knelpunten in uw toepassing adres schalen?
* identificeren van problemen met servicecommunicatie en externe toegang via het cluster
* uitzoeken wat gebruikers met uw toepassing doen-het instrumenteren van uw toepassingen kunt u handleiding toekomstige functie ontwikkeling en betere diagnostische gegevens voor app-fouten

Voor bewaking op toepassingsniveau in Service Fabric, is het raadzaam dat u Application Insights (AI). AI van integratie met Service Fabric bevat tooling ervaringen voor Visual Studio en Azure-portal en een goed begrip van de context van Service Fabric-service en externe toegang in het AI-dashboard en de toepassing-kaart, wat leidt tot een uitgebreide out-of-the-box-logboekregistratie ervaring. Hoewel veel logboeken automatisch worden gemaakt en die voor u worden verzameld bij gebruik van AI, wordt aangeraden dat u meer aangepaste logboekregistratie aan uw toepassingen toevoegen en die worden weergegeven in AI hebben naast wat is opgegeven voor het maken van een rijkere ervaring voor diagnostische gegevens voor het verwerken van problemen in de toekomst. Zie voor meer informatie over het gebruik van AI met Service Fabric op [analyse van gebeurtenis met Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md). 

Als u wilt beginnen met het instrumenteren van uw code om uw toepassingen te bewaken, Ga naar [toepassing niveau gebeurtenis- en logboekbestanden generatie](service-fabric-diagnostics-event-generation-app.md). 

### <a name="monitoring-application-availability"></a>Bewaking van de beschikbaarheid van toepassingen
Het is mogelijk dat alles in uw cluster lijkt te worden uitgevoerd zoals verwacht en als goed rapporteert, maar uw toepassingen goed ontoegankelijk of niet bereikbaar lijken. Hoewel er geen veel gevallen waarin dit gebeurt, is het belangrijk te weten wanneer dit gebeurt kunnen zo snel mogelijk beperken. Bewaking op beschikbaarheid is grotendeels betrokken bij het bijhouden van de beschikbaarheid van onderdelen van uw systeem te begrijpen van de algemene 'bedrijfstijd"van het systeem. We richten op beschikbaarheid vanuit het perspectief van uw toepassing - de platform werkt om ervoor te zorgen dat lifecycle toepassing beheerscenario's veroorzaken geen uitvaltijd in een cluster. Niettemin verschillende problemen in het cluster kunnen leiden tot die invloed hebben op actieve tijdsduur van uw toepassing en in dergelijke gevallen, als de eigenaar van een toepassing wilt u waarschijnlijk meteen. Het is raadzaam dat samen met alle andere toepassingen, u een watchdog in uw cluster implementeert. Het doel van een dergelijke watchdog zou zijn om te controleren van de eindpunten voor uw toepassing met een set tijdsinterval en rapporteren dat ze toegankelijk zijn. U kunt dit ook doen met behulp van een externe service die het eindpunt pingt en retourneert een rapport met het opgegeven tijdsinterval.

## <a name="cluster-monitoring"></a>Bewaking van het cluster
Bewaking van uw Service Fabric-cluster is essentieel om ervoor te zorgen dat het platform en alle werkbelastingen die worden uitgevoerd op het de bedoeling worden uitgevoerd. Een van de doelstellingen van de Service Fabric is dat toepassingen via hardwarefouten. Dit wordt bereikt door middel van het platform systeemservices mogelijkheid voor het detecteren van infrastructuur problemen en snel failover werkbelastingen naar andere knooppunten in het cluster. Maar in dit geval, wat gebeurt er als de systeemservices zelf problemen? Of als bij een poging te verplaatsen van een werkbelasting, regels voor de plaatsing van de services zijn geschonden? Bewaking van het cluster, kunt u op de hoogte over activiteit plaatsvinden in uw cluster, helpt bij het opsporen van problemen en corrigeren effectief blijven. Er zijn een aantal belangrijke zaken die u wilt zoeken naar:
* Is de Service Fabric werkt zoals verwacht, wat betreft het plaatsen van uw toepassingen en netwerktaakverdeling werk op het cluster? 
* Zijn acties worden uitgevoerd op de configuratie van het cluster wordt bevestigd en zoals verwacht heeft gehandeld wijzigen? Dit is vooral van belang bij het schalen van een cluster.
* Service Fabric verwerkt uw gegevens en uw service-service-communicatie binnen het cluster correct?

Service Fabric bevat een uitgebreide reeks gebeurtenissen uit het vak via de operationele en de gegevens en de Messaging-kanalen. In Windows, zijn in de vorm van een enkele ETW-provider met een reeks relevante `logLevelKeywordFilters` gebruikt om te kiezen tussen verschillende kanalen. Op Linux, alle van de platform-gebeurtenissen worden geleverd via LTTng en van waar ze kunnen worden gefilterd zo nodig in één tabel zijn geplaatst. 

Deze kanalen bevatten curated, gestructureerde gebeurtenissen die kunnen worden gebruikt om de status van uw cluster beter te begrijpen. ' Diagnostische gegevens ' is standaard ingeschakeld tijdens de aanmaak van het cluster die u met een Azure Storage-tabel waar de gebeurtenissen uit deze kanalen voor u een query in de toekomst worden verzonden. Meer informatie over het bewaken van uw cluster op [Platform niveau gebeurtenis- en logboekbestanden generatie](service-fabric-diagnostics-event-generation-infra.md). U wordt aangeraden OMS logboekanalyse voor het bewaken van uw cluster. OMS Log Analytics biedt een Service Fabric specifieke oplossing, Service Fabric Analytics, dat voorziet in een aangepast dashboard voor het bewaken van Service Fabric-clusters, en kunt u query uitvoeren op uw cluster gebeurtenissen en waarschuwingen instellen. Meer informatie over deze [analyse van gebeurtenis met OMS](service-fabric-diagnostics-event-analysis-oms.md). 

### <a name="watchdogs"></a>Watchdogs
In het algemeen is een watchdog een afzonderlijke service die u kunt bekijken van de gezondheid en laden, ping eindpunten en rapport health-services voor alles in het cluster. Dit kan helpen voorkomen dat er fouten die niet kunnen worden gedetecteerd op basis van de weergave van een service. Watchdogs zijn ook een goede plaats naar host-code die corrigerende acties zonder tussenkomst van de gebruiker (bijvoorbeeld het opschonen van logboekbestanden in de opslag op bepaalde tijdsintervallen) uitvoert. U vindt een steekproef watchdog service-implementatie [hier](https://github.com/Azure-Samples/service-fabric-watchdog-service).

## <a name="performance-monitoring"></a>Prestatiebewaking
Bewaking van de onderliggende infrastructuur is een belangrijk onderdeel van de informatie over de status van het cluster en de bronnen beter worden benut. Meten van de systeemprestaties, is afhankelijk van verschillende factoren, die elk gewoonlijk via een Key Performance Indicators (KPI's gemeten worden). Service Fabric die relevante KPI's kunnen worden toegewezen aan de metrische gegevens die kunnen worden verzameld van de knooppunten in het cluster als prestatiemeteritems.
Deze KPI's kunnen helpen bij:
* informatie over Resourcegebruik en load - omwille van de schaal van uw cluster of voor het optimaliseren van uw service wordt verwerkt
* infrastructuurfouten - voorspellen veel problemen worden voorafgegaan door plotselinge veranderingen (neerzetten) van de prestaties, zodat u KPI's zoals netwerk-i/o en CPU-gebruik gebruiken kunt voor het voorspellen van en het infrastructurele problemen onderzoeken

Een lijst met prestatiemeteritems die moeten worden verzameld op het niveau van de infrastructuur kan worden gevonden op [maatstaven voor prestaties](service-fabric-diagnostics-event-generation-perf.md). 

Voor niveau bewaking van toepassingsprestaties voorziet Service Fabric in een set van prestatiemeteritems voor de programmeermodellen Reliable Services en actoren. Als u een van deze modellen worden gebruikt, krijgt deze prestatiemeteritems u KPI's waarmee u ervoor zorgen dat uw actoren zijn draait omhoog en omlaag correct, of dat uw betrouwbare serviceaanvragen snel genoeg worden verwerkt. Zie [bewaking voor betrouwbare Service voor externe toegang](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) en [Prestatiebewaking voor Reliable Actors](service-fabric-reliable-actors-diagnostics.md#performance-counters) voor meer informatie hierover. Hiernaast Application Insights beschikt ook over een reeks maatstaven voor prestaties die het wordt verzameld, indien geconfigureerd met uw toepassing.

De OMS-agent gebruiken voor het verzamelen van de relevante prestatiemeteritems en weergeven van deze KPI's in de OMS-logboekanalyse. U kunt ook de extensie Azure Diagnostics-agent (of andere vergelijkbare agent) voor het verzamelen en opslaan van de metrische gegevens voor analyse. 

## <a name="health-monitoring"></a>Statuscontrole
Het Service Fabric-platform omvat een statusmodel, wat zorgt voor extensible de statusrapportage voor de status van de entiteiten in een cluster. Elk knooppunt, toepassing, service, partitie, replica of -exemplaar heeft een voortdurend bij te werken gezondheidsstatus. De status kan zijn 'OK', 'Waarschuwing' of 'Fout'. De status wordt gewijzigd via de health-rapporten die worden gegenereerd voor elke entiteit, op basis van de problemen in het cluster. De status van de entiteiten kan worden gecontroleerd op elk gewenst moment in Service Fabric Explorer (SFX), zoals hieronder wordt weergegeven, of via de platforms Health API kan worden opgevraagd. U kunt ook statusrapporten aanpassen en wijzigen van de status van een entiteit met het toevoegen van uw eigen statusrapporten of met behulp van de Health-API. Meer informatie over het statusmodel kunnen worden gevonden op [Inleiding tot Service Fabric-statuscontrole](service-fabric-health-introduction.md).

![SFX health dashboard](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)

Naast de meest recente statusrapporten in SFX ziet, is elk rapport ook beschikbaar als een gebeurtenis. Health-gebeurtenissen kunnen worden verzameld via het operationele kanaal (Zie [gebeurtenis aggregatie met Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md#collect-health-and-load-events)), en opgeslagen in OMS Log Analytics voor waarschuwingen en gegevensquery's in de toekomst. Dit helpt u problemen die mogelijk van invloed op de beschikbaarheid van uw toepassing, zodat het is raadzaam dat u waarschuwingen voor scenario's juiste fouten (aangepaste waarschuwingen via OMS instellen) detecteren.

## <a name="monitoring-workflow"></a>Bewaking van de werkstroom 

De algemene werkstroom van controle en diagnostische gegevens bestaat uit drie stappen:

1. **Genereren van gebeurtenis**: dit bevat gebeurtenissen (Logboeken, traceringen, aangepaste gebeurtenissen) op het niveau van de infrastructuur, het platform (cluster) en toepassing
2. **Gebeurtenis aggregatie**: deze fase is in feite de pijplijn voor uw gebeurtenissen om uiteindelijk in een hulpprogramma waar u ze kunt analyseren met de extensie voor diagnostische gegevens van Azure of EventFlow
3. **Analyse**: gebeurtenissen toegankelijk moeten zijn in een hulpmiddel om toe te staan voor analyse - visualisatie, uitvoeren van query's, waarschuwingen, enzovoort.

Meerdere producten zijn beschikbaar die betrekking op deze drie gebieden en u bent gratis verschillende technologieën voor elk kiezen. Het is belangrijk om ervoor te zorgen dat de verschillende onderdelen samenwerken voor het leveren van een oplossing voor de end-to-end-controle voor uw cluster.

## <a name="event-generation"></a>Genereren van gebeurtenis

De eerste stap in de werkstroom voor controle en diagnostische gegevens is voor het instellen van het maken en het genereren van gebeurtenis-en logboekbestanden. Deze gebeurtenissen, logboeken en prestatiemeteritems worden verzonden op alle drie niveaus: het niveau van de toepassing (instrumentation toegevoegd aan de apps en services die zijn geïmplementeerd met het cluster), platform (gebeurtenissen uit het cluster op basis van de werking van Service Fabric verzonden), en het niveau van de infrastructuur (maatstaven voor prestaties van elk knooppunt). Diagnostics-gegevens die worden verzameld op elk niveau kan worden aangepast, hoewel Service Fabric sommige instrumentation standaard schakelt. 

Lees meer over [platform niveau gebeurtenissen](service-fabric-diagnostics-event-generation-infra.md) en [niveau toepassingsgebeurtenissen](service-fabric-diagnostics-event-generation-app.md) om te begrijpen wat is opgegeven en het toevoegen van meer instrumentation. De belangrijkste beslissing hebt u hier is hoe wilt u uw toepassing instrumenteren. Voor .NET-toepassingen kunt het beste ILogger gebruikt, maar u kunt ook verkennen EventSource, Serilog en andere vergelijkbare bibliotheken. Voor Java, wordt u aangeraden Log4j. Afgezien van deze opties zijn er verschillende andere opties beschikbaar die kunnen worden gebruikt op basis van de aard van de toepassing. Gerust verkennen wat zou worden aanbevolen voor uw specifieke gebruiksvoorbeeld of kies een dat u het meest vertrouwd bent met. 

Nadat u een beslissing op de logboekregistratie-provider die u wilt gebruiken, moet u controleren of uw logboeken worden samengevoegd en de juiste manier opgeslagen.

## <a name="event-aggregation"></a>Aggregatie van gebeurtenis

Voor het verzamelen van Logboeken en gebeurtenissen die door uw toepassingen en het cluster wordt gegenereerd, in het algemeen wordt aangeraden met behulp van de [extensie voor diagnostische gegevens van Azure](service-fabric-diagnostics-event-aggregation-wad.md) (meer vergelijkbaar met logboekverzameling op basis van een agent) of [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) (in-process logboekgegevens verzameld). Als u met behulp van Application Insights en in .NET of Java ontwikkelt, wordt met behulp van de Application Insights-SDK in plaats van EventFlow aanbevolen.

Het is mogelijk dat een vergelijkbare werk kan verrichten via met slechts een van deze in de meeste situaties leidt de agent van de extensie Azure Diagnostics combineren met een pijplijn in-process-verzameling (AI SDK of EventFlow) tot een werkstroom betrouwbaarder, uitgebreide, bewaking . De agent van de extensie Azure Diagnostics is het pad voor het platform niveau gebeurtenissen, terwijl u AI SDK of EventFlow (in-process verzameling) voor uw toepassing niveau Logboeken gebruiken. 

Hier volgen enkele belangrijke punten rekening moet houden in het geval dat u wilt gebruiken op slechts één van deze.
* Verzamelen van toepassingslogboeken met de extensie Azure Diagnostics is een goede optie voor Service Fabric-services als de set van logboek-bronnen en -doelen niet vaak wordt gewijzigd en er een eenvoudige toewijzing tussen de bronnen en hun doelen is. De reden voor dit is het configureren van Azure Diagnostics vindt plaats op de Resource Manager-laag, zodat u belangrijke wijzigingen doorvoert aan de configuratie vereist voor het bijwerken van het volledige cluster. Dit betekent dat vaak wordt minder efficiënt terechtkomt dan met AI SDK of EventFlow gaan.
* Met behulp van EventFlow, kunt u de logboeken verzenden rechtstreeks naar een platform voor analyse en visualisatie en/of storage-services hebben. Andere bibliotheken (ILogger, Serilog, enz.) kunnen ook worden gebruikt voor hetzelfde doel, maar EventFlow heeft het voordeel van een specifiek voor in-process logboekgegevens verzameld en ter ondersteuning van Service Fabric-services die zijn ontworpen. Dit heeft vaak verschillende voordelen in termen van eenvoudige configuratie en implementatie en biedt meer flexibiliteit bij het ondersteunen van verschillende logboekregistratie bibliotheken en hulpprogramma's voor analyse. 

## <a name="event-analysis"></a>Gebeurtenisanalyse

Er zijn verschillende geweldige platforms die aanwezig zijn in de markt als het gaat om de analyse- en visualisatie van controle en diagnostische gegevens. De twee die wij adviseren [OMS](service-fabric-diagnostics-event-analysis-oms.md) en [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) vanwege hun integratie met Service Fabric. U moet ook zoeken in de [elastische Stack](https://www.elastic.co/products) (met name als u een cluster uitvoert in een offline omgeving overweegt), [Splunk](https://www.splunk.com/), of een andere platform van uw voorkeur. 

De belangrijkste punten opgeven die u voor elk platform dat u kiest moeten zijn opgenomen hoe vertrouwd u zich met de gebruikersinterface en query-opties, kunnen gegevens visualiseren en gemakkelijk leesbare dashboards en de aanvullende hulpprogramma's die ze bieden voor het verbeteren van de bewaking maken geautomatiseerd, zoals waarschuwingen.

Naast het platform dat u kiest, bij het instellen van een Service Fabric-cluster als een Azure-resource, krijgt u ook toegang tot Azure out-of-the-box Prestatiebewaking voor uw machines.

### <a name="azure-monitor"></a>Azure Monitor

U kunt [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) voor het bewaken van veel van de Azure-resources waarop een Service Fabric-cluster is gebouwd. Een set van metrische gegevens voor de [virtuele-machineschaalset](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) en afzonderlijke [virtuele machines](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) automatisch worden verzameld en weergegeven in de Azure-portal. Als u wilt de verzamelde gegevens weergeven in de Azure portal, selecteer de resourcegroep die de Service Fabric-cluster bevat. Selecteer vervolgens de virtuele-machineschaalset die u wilt weergeven. In de **bewaking** sectie (aan de Linkernavigatie), selecteer **metrische gegevens** om een grafiek van de waarden weer te geven.

![Azure portal weergave van verzamelde metrische gegevens](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

Volg de instructies in voor het aanpassen van de grafieken [metrische gegevens in Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md). Ook kunt u waarschuwingen op basis van deze metrische gegevens, zoals beschreven in [waarschuwingen in de Azure-Monitor maken voor Azure-services](../monitoring-and-diagnostics/monitoring-overview-alerts.md). 

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het controleren van het platform en de Service Fabric bevat voor gebeurtenissen [Platform niveau gebeurtenis- en logboekbestanden genereren](service-fabric-diagnostics-event-generation-infra.md)
* Zie voor aan de slag met het instrumenteren van uw toepassingen, [toepassing niveau gebeurtenis- en logboekbestanden genereren](service-fabric-diagnostics-event-generation-app.md)
* Gaat u de zelfstudie [bewaken en onderzoeken van een toepassing ASP.NET Core in Service Fabric](service-fabric-tutorial-monitoring-aspnet.md)

