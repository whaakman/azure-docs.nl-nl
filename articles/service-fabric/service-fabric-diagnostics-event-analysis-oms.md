---
title: Analyse van Azure Service Fabric-gebeurtenis met OMS | Microsoft Docs
description: Meer informatie over het visualiseren en analyseren van gebeurtenissen met OMS voor controle en diagnostische gegevens van Azure Service Fabric-clusters.
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
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 977c5d64a32157b39aa6b618196dde20c4c3cc8e
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="event-analysis-and-visualization-with-oms"></a>Analyse van gebeurtenis en visualisatie met OMS

Operations Management Suite (OMS) is een verzameling van management-services die u bij de controle en diagnostische gegevens voor toepassingen helpen en services die worden gehost in de cloud. Als u een meer gedetailleerd overzicht van OMS en wat biedt, lezen [wat OMS is?](../operations-management-suite/operations-management-suite-overview.md)

## <a name="log-analytics-and-the-oms-workspace"></a>Log Analytics en de OMS-werkruimte

Log Analytics verzamelt gegevens van beheerde bronnen, met inbegrip van de tabel van een Azure-opslag of een agent en onderhoudt dat bij een centrale opslagplaats. De gegevens kunnen vervolgens worden gebruikt voor analyse, waarschuwingen en visualisatie of meer geëxporteerd. Log Analytics ondersteunt gebeurtenissen, prestatiegegevens of andere aangepaste gegevens.

Wanneer OMS is geconfigureerd, hebt u toegang tot een specifieke *OMS-werkruimte*, uit waarbij gegevens kunnen worden opgevraagd of weergegeven in dashboards.

Nadat de gegevens worden ontvangen door logboekanalyse OMS heeft diverse *beheeroplossingen* die voorverpakte oplossingen voor het bewaken van binnenkomende gegevens, aangepast aan verschillende scenario's zijn. Deze omvatten een *Service Fabric Analytics* oplossing en een *Containers* oplossing, zijn de twee meest relevante diagnostische gegevens en controleren wanneer met behulp van Service Fabric-clusters. Er zijn verschillende evenals met waard en OMS biedt tevens mogelijkheden voor het maken van aangepaste oplossingen die u kunt meer lezen over [hier](../operations-management-suite/operations-management-suite-solutions.md). Elke oplossing die u wilt gebruiken voor een cluster kan worden geconfigureerd in de dezelfde OMS-werkruimte, samen met logboekanalyse. Werkruimten kunnen u aangepaste dashboards en visualisatie van gegevens en wijzigingen in de gegevens die u wilt verzamelen, te verwerken en analyseren.

## <a name="setting-up-an-oms-workspace-with-the-service-fabric-analytics-solution"></a>Instellen van een OMS-werkruimte met de Service Fabric Analytics-oplossing
Het is raadzaam dat u de Service Fabric-oplossing in uw werkruimte OMS opnemen-bevat een dashboard dat toont de verschillende binnenkomende logboekkanalen van het platform en toepassing niveau en biedt de mogelijkheid om query Service Fabric in specifieke Logboeken. Hier ziet u hoe een relatief eenvoudige oplossing voor Service Fabric uitziet, met één toepassing geïmplementeerd op het cluster:

![OMS SF-oplossing](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Zie [OMS Log Analytics instellen](service-fabric-diagnostics-oms-setup.md) aan de slag met deze voor uw cluster.

## <a name="using-the-oms-agent"></a>Met behulp van de OMS-Agent

Het is aanbevolen en te gebruiken EventFlow af als aggregatieoplossingen omdat ze is toegestaan voor een meer modulaire benadering voor diagnostische gegevens en bewaking. Bijvoorbeeld: als u wilt wijzigen van de uitvoer van EventFlow, heeft deze geen wijziging in uw werkelijke instrumentation, alleen een eenvoudige wijziging aan het configuratiebestand. Als u echter besluit te investeren in OMS Log Analytics gebruiken, moet u instellen de [OMS-agent](../log-analytics/log-analytics-windows-agent.md). U moet ook de OMS-agent gebruiken bij het implementeren van containers met uw cluster, zoals hieronder beschreven. 

HEAD via naar [de OMS-Agent toevoegen aan een cluster](service-fabric-diagnostics-oms-agent.md) voor stapsgewijze instructies voor dit.

De voordelen hiervan zijn de volgende:

* Rijkere gegevens op de prestaties tellers en metrische gegevens aan clientzijde
* Eenvoudig te configureren van de metrische gegevens worden verzameld uit het cluster en zonder de configuratie van het cluster bij te werken. Wijzigingen in de instellingen van de agent kunnen worden gedaan vanuit de OMS-portal en de agent wordt automatisch opnieuw opgestart zodat deze overeenkomen met de vereiste configuratie. Ga naar de werkruimte voor het configureren van de OMS-agent om op te halen specifieke prestatiemeteritems **Start > Instellingen > gegevens > Windows-prestatiemeteritems** en selecteer de gegevens die u zou willen zien die worden verzameld
* Gegevens weergegeven sneller dan het hoeft te worden opgeslagen voordat deze wordt opgehaald door OMS / Log Analytics
* Bewaking van containers is veel eenvoudiger, omdat deze docker-Logboeken (stdout, stderr) en statistieken (maatstaven voor prestaties op knooppunt en container niveaus ophalen kan)

De belangrijkste overweging hier is of omdat de agent op uw cluster naast al uw toepassingen worden geïmplementeerd, kan er enige impact op de prestaties van uw toepassingen op het cluster.

## <a name="monitoring-containers"></a>Bewaking van Containers

Bij het implementeren van containers naar een Service Fabric-cluster, wordt het aanbevolen dat het cluster is ingesteld met de OMS-agent en dat de oplossing Containers is toegevoegd aan de OMS-werkruimte bewaking wilt inschakelen en diagnostische gegevens. Hier ziet u hoe de oplossing containers in een werkruimte uitziet:

![Basic OMS-Dashboard](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

De agent kunt het verzamelen van verschillende container-specifieke logboeken die kan worden doorzocht in OMS of gebruikt voor het gevisualiseerde prestatie-indicatoren. De logboek-typen die zijn verzameld zijn:

* ContainerInventory: geeft informatie over de containerlocatie, naam en installatiekopieën
* ContainerImageInventory: informatie over geïmplementeerde installatiekopieën, met inbegrip van id's of grootten
* ContainerLog: specifieke foutenlogboeken, docker-Logboeken (stdout, enzovoort) en andere items
* ContainerServiceLog: docker-daemon opdrachten die zijn uitgevoerd
* Prestaties: prestatiemeteritems met inbegrip van container cpu, geheugen, netwerkverkeer, schijf-i/o en aangepaste metrische gegevens van de hostmachines

[Containers met OMS Log Analytics bewaken](service-fabric-diagnostics-oms-containers.md) bevat informatie over de stappen die nodig zijn voor het instellen van de bewaking van de container voor uw cluster. Bekijk voor meer informatie over de OMS-Containers oplossing hun [documentatie](../log-analytics/log-analytics-containers.md).

## <a name="next-steps"></a>Volgende stappen

Bekijk de volgende OMS-hulpprogramma's en opties voor het aanpassen van een werkruimte aan uw behoeften:

* OMS biedt voor lokale clusters, een Gateway (http-doorsturen Proxy) die kan worden gebruikt om gegevens te verzenden aan OMS. Lees meer over die in [computers zonder toegang tot het Internet verbinden met OMS met behulp van de OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md)
* Configureren van OMS om in te stellen [geautomatiseerde waarschuwingen](../log-analytics/log-analytics-alerts.md) om te helpen detecteren en diagnostische gegevens
* Familiarized ophalen met de [zoeken en uitvoeren van query's in logboek registreren](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van logboekanalyse