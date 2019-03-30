---
title: Resource Manager-architectuur | Microsoft Docs
description: Een architectuuroverzicht van Service Fabric Cluster Resource Manager.
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 6c4421f9-834b-450c-939f-1cb4ff456b9b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: bfbdb05e8d2764d2b878e22d236cae30519da176
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666788"
---
# <a name="cluster-resource-manager-architecture-overview"></a>Overzicht van cluster resource manager-architectuur
De Service Fabric Cluster Resource Manager is een centrale service die wordt uitgevoerd in het cluster. Hiermee worden de gewenste status van de services in het cluster, met name met betrekking tot gebruik van resources en eventuele plaatsingsregels beheerd. 

Voor het beheren van de resources in uw cluster, moet de Service Fabric Cluster Resource Manager hebben verschillende soorten gegevens:

- Welke services momenteel aanwezig zijn
- Resourceverbruik door elke service is de huidige (of standaard) 
- De resterende clustercapaciteit 
- De capaciteit van de knooppunten in het cluster 
- De hoeveelheid resources die worden gebruikt op elk knooppunt

Het Resourcegebruik van een bepaalde service na verloop van tijd kunt wijzigen, en services zorgt meestal over meer dan één type resource. Naar verschillende services, kunnen er echt fysieke en fysieke resources die wordt gemeten. Services kunnen fysieke metrische gegevens zoals verbruik van geheugen en schijfruimte bijhouden. Meer over het algemeen services kunnen het belangrijkst logische metrische gegevens over - zaken zoals "WorkQueueDepth" of 'TotalRequests'. Metrische gegevens over logische en fysieke kan worden gebruikt in hetzelfde cluster. Metrische gegevens kunnen worden gedeeld met veel services of specifiek zijn voor een bepaalde service.

## <a name="other-considerations"></a>Andere overwegingen
De eigenaren en exploitanten van het cluster kunnen afwijken van de service en toepassing auteurs of ten minste zijn de wearing verschillende rollen voor dezelfde personen. Bij het ontwikkelen van uw toepassing weet u een aantal dingen over wat er is vereist. U hebt een schatting van de resources die deze wordt gebruikt en hoe de verschillende services moeten worden geïmplementeerd. Bijvoorbeeld, moet de weblaag worden uitgevoerd op knooppunten die toegang heeft tot het Internet, terwijl de databaseservices niet moeten. De webservices worden waarschijnlijk nog een voorbeeld: beperkt door CPU en het netwerk, terwijl de gegevens laag services verzorging meer informatie over het verbruik van geheugen en schijfruimte. De persoon die het verwerken van een live-site-incident voor die service in productie, of die wordt beheerd door een upgrade voor de service heeft echter een andere taak te doen, en vereist verschillende hulpprogramma's. 

Het cluster en de services zijn dynamisch:

- Het aantal knooppunten in het cluster kunt vergroten of verkleinen
- Knooppunten van verschillende grootten en -typen kunnen worden geleverd en Ga
- Services kunnen worden gemaakt, verwijderd, en de gewenste resourcetoewijzingen en regels voor de plaatsing wijzigen
- Upgrades of andere beheerbewerkingen kunnen implementeren via het cluster op de toepassing op infrastructuur-niveaus
- Fouten kunnen op elk gewenst moment plaatsvinden.

## <a name="cluster-resource-manager-components-and-data-flow"></a>Cluster resource manager-onderdelen en de gegevensstroom
Cluster Resource Manager heeft voor het bijhouden van de vereisten van elke service en het verbruik van resources door elke serviceobject in die services. Cluster Resource Manager bestaat uit twee delen van de conceptuele: agents die worden uitgevoerd op elk knooppunt en een fouttolerantie-service. De agents op elk knooppunt bijhouden load rapporten van services, statistische ze en periodiek meldt deze. De Cluster Resource Manager-service combineert de gegevens van de lokale agenten en omdat zij reageren op basis van de huidige configuratie.

Bekijk het volgende diagram:

<center>

![Resource Balancer-architectuur][Image1]
</center>

Tijdens runtime zijn er veel wijzigingen die kunnen optreden. Bijvoorbeeld, laten we bijvoorbeeld de hoeveelheid resources gebruiken voor sommige services wordt gewijzigd, sommige services is mislukt, en sommige knooppunten toevoegen aan en verwijderen het cluster. De wijzigingen op een knooppunt zijn samengevoegd en periodiek verzonden naar de Cluster Resource Manager-service (1,2) waar ze opnieuw worden samengevoegd, geanalyseerd en opgeslagen. Elke paar seconden met service kijkt naar de wijzigingen en bepaalt of alle acties die nodig zijn zijn (3). Het kan bijvoorbeeld ziet dat sommige lege knooppunten zijn toegevoegd aan het cluster. Als gevolg hiervan besluit sommige services verplaatsen naar die knooppunten. Cluster Resource Manager kan ook ziet u dat een bepaald knooppunt overbelast is of dat bepaalde services zijn mislukt of is verwijderd, resources elders vrijmaken.

Laten we kijken naar het volgende diagram en zien wat er daarna gebeurt. Stel dat de Cluster Resource Manager bepaalt dat er wijzigingen nodig zijn. Deze coördineert met andere systeemservices (met name de Failover Manager) als de benodigde wijzigingen wilt aanbrengen. Vervolgens worden de benodigde opdrachten verzonden naar de juiste knooppunten (4). Stel dat de Resource Manager al opgemerkt dat Knooppunt5 is overbelast, waardoor dus besloten service B van Knooppunt5 naar Knooppunt4 verplaatsen. Aan het einde van de nieuwe configuratie (5), is het cluster ziet er als volgt:

<center>

![Resource Balancer-architectuur][Image2]
</center>

## <a name="next-steps"></a>Volgende stappen
- Cluster Resource Manager beschikt over veel opties voor het beschrijven van het cluster. Meer informatie over deze, Bekijk dit artikel op [met een beschrijving van een Service Fabric-cluster](./service-fabric-cluster-resource-manager-cluster-description.md)
- Primaire functies van de Cluster Resource Manager zijn herverdeling van het cluster en afdwingen van regels voor de plaatsing. Zie voor meer informatie over het configureren van deze problemen [taakverdeling van uw Service Fabric-cluster](./service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png
