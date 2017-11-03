---
title: Resource Manager-architectuur | Microsoft Docs
description: Een overzicht van architectuur van Service Fabric Cluster Resource Manager.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 6c4421f9-834b-450c-939f-1cb4ff456b9b
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: f0d2202c17bf4d378a625a61e941edf7f3f24636
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="cluster-resource-manager-architecture-overview"></a>Overzicht van de cluster resource manager-architectuur
De Service Fabric Cluster Resource Manager is een centrale service die wordt uitgevoerd in het cluster. Hiermee beheert u de gewenste status van de services in het cluster, met name met betrekking tot brongebruik en regels voor de plaatsing. 

Voor het beheren van de resources in uw cluster moet de Service Fabric Cluster Resource Manager hebben verschillende soorten informatie:

- Welke services bestaat
- Brongebruik door elke service is de huidige (of standaard) 
- De resterende cluster-capaciteit 
- De capaciteit van de knooppunten in het cluster 
- De hoeveelheid resources verbruikt op elk knooppunt

Het brongebruik van een bepaalde service kunt wijzigen na verloop van tijd en meer dan één type resource services meestal interesseren. Alle andere services worden echt fysieke en fysieke resources wordt gemeten. Services kunnen fysieke metrische gegevens zoals verbruik van geheugen en schijfruimte bijhouden. Services kunnen vaker voorkomt, logische metrics - items zoals 'WorkQueueDepth' of 'TotalRequests' hechten. Logische en fysieke metrische gegevens kan worden gebruikt in hetzelfde cluster. Metrische gegevens kunnen worden gedeeld door veel services of specifiek zijn voor een bepaalde service.

## <a name="other-considerations"></a>Andere overwegingen
De eigenaren en gebruiken van het cluster kunnen afwijken van de service en toepassing auteurs of ten minste zijn de wearing verschillende rollen voor dezelfde personen. Bij het ontwikkelen van uw toepassing weet u leest over wat is vereist. U hebt een schatting van de resources dit verbruikt en hoe de andere services moeten worden geïmplementeerd. Bijvoorbeeld, moet de weblaag worden uitgevoerd op knooppunten die zijn blootgesteld aan Internet, terwijl de databaseservices niet moeten. De webservices worden bijvoorbeeld waarschijnlijk beperkt door CPU en het netwerk, terwijl de gegevens laag services zorg meer informatie over het verbruik van geheugen en schijfruimte. De persoon die het verwerken van een incident live-site voor de service in productie of die wordt beheerd door een upgrade naar de service heeft echter een andere taak te doen, en vereist verschillende hulpprogramma's. 

Het cluster en de services zijn dynamisch:

- Het aantal knooppunten in het cluster kunt vergroten of verkleinen
- Knooppunten van verschillende grootte en de typen kunnen worden geleverd en Ga
- Services kunnen worden gemaakt, verwijderd, en hun resourcetoewijzingen van de gewenste en de regels voor de plaatsing wijzigen
- Upgrades of andere beheerbewerkingen kunnen draaien door middel van het cluster op de toepassing op infrastructuur niveaus
- Fouten kunnen optreden op elk gewenst moment.

## <a name="cluster-resource-manager-components-and-data-flow"></a>Cluster resource manager-onderdelen en gegevensstroom
De Cluster Resource Manager heeft voor het bijhouden van de vereisten van elke service en het verbruik van bronnen door elke serviceobject in deze services. De Cluster Resource Manager bestaat uit twee delen van de conceptuele: agents die worden uitgevoerd op elk knooppunt en een fouttolerante-service. De agents op elk knooppunt bijhouden load rapporten van services, cumulatieve ze en deze regelmatig te melden. De Cluster Resource Manager-service maakt een aggregatie van alle gegevens van de lokale agents en omdat zij reageren op basis van de huidige configuratie.

Bekijk het volgende diagram:

<center>
![Resource Balancer architectuur][Image1]
</center>

Er zijn veel wijzigingen die kunnen optreden tijdens runtime. Bijvoorbeeld, laten we zeggen de hoeveelheid resources gebruiken voor sommige services wordt gewijzigd, sommige services is mislukt, en sommige knooppunten join en laat het cluster. De wijzigingen op een knooppunt zijn geaggregeerd en periodiek verzonden naar het Cluster Resource Manager-service (1,2) waarin ze zijn geaggregeerd opnieuw, geanalyseerd en opgeslagen. Om de paar seconden die service kijkt naar de wijzigingen en bepaalt of de acties die het nodig zijn (3). Het kan bijvoorbeeld merkt dat sommige leeg knooppunten zijn toegevoegd aan het cluster. Als gevolg hiervan besluit sommige services verplaatsen naar die knooppunten. De Cluster Resource Manager ook kan er een bepaald knooppunt overbelast is of dat bepaalde services is mislukt of verwijderd, resources elders vrijmaken.

Laten we kijken naar het volgende diagram en kijk wat gebeurt er nu. Stel dat de Cluster Resource Manager bepaalt of er wijzigingen nodig zijn. Deze coördineert met andere systeemservices (met name de Failover Manager) om de benodigde wijzigingen aanbrengen. Vervolgens worden de benodigde opdrachten verzonden naar de juiste knooppunten (4). Stel dat de Resource Manager al opgemerkt dat Knooppunt5 is overbelast en dus besloten service B van Knooppunt5 naar Knooppunt4 verplaatsen. Aan het einde van de nieuwe configuratie (5) uitziet het cluster:

<center>
![Resource Balancer architectuur][Image2]
</center>

## <a name="next-steps"></a>Volgende stappen
- De Cluster Resource Manager bevat veel opties voor het beschrijven van het cluster. Meer informatie over deze, Bekijk dit artikel op [met een beschrijving van een Service Fabric-cluster](./service-fabric-cluster-resource-manager-cluster-description.md)
- De Cluster Resource Manager primaire rechten zijn herverdeling van het cluster en het afdwingen van regels voor de plaatsing. Zie voor meer informatie over het configureren van deze problemen [balancing Service Fabric-cluster](./service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png
