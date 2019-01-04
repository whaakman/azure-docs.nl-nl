---
title: Inleiding tot Azure Service Fabric-Resourcemodel | Microsoft Docs
description: Meer informatie over het Model in de Resource van Service Fabric, een eenvoudige benadering voor het definiëren van Service Fabric-NET-toepassingen.
services: service-fabric-mesh
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/23/2018
ms.author: vturecek
ms.custom: mvc, devcenter
ms.openlocfilehash: 702e1ef9c8593c2106be256e6fd7de602bf41aa7
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54019993"
---
# <a name="introduction-to-service-fabric-resource-model"></a>Inleiding tot Service Fabric-Resourcemodel

Het Service Fabric-Resourcemodel beschrijft een eenvoudige benadering voor het definiëren van de resources die deel uitmaken van een Service Fabric-NET-toepassing. Afzonderlijke resources kunnen worden geïmplementeerd op elke willekeurige Service Fabric-omgeving.  Het Service Fabric-Resourcemodel is ook compatibel met het Azure Resource Manager-Model. De volgende typen resources worden momenteel ondersteund in dit model:

- Toepassingen en services
- Netwerken
- Gateways
- Geheimen en geheimen/waarden
- Volumes

Elke resource wordt declaratief beschreven in een resource-bestand, dat een eenvoudige YAML is- of JSON-document dat de toepassing NET beschrijft en die door de Service Fabric-platform is ingericht.

## <a name="applications-and-services"></a>Toepassingen en Services

De bron van een toepassing is de eenheid van de implementatie, versiebeheer en levensduur van een Mesh-toepassing. Deze bestaat uit een of meer, Service-resources die een microservice vertegenwoordigen. Elke resource Service wordt op zijn beurt bestaat uit een of meer codepakketten die alles wat nodig is om uit te voeren van de containerinstallatiekopie die is gekoppeld aan het codepakket wordt beschreven.

![Apps en services][Image1]

Een Serviceresource declareert de volgende:

- De containernaam van de, versie en -register
- CPU en geheugen-resources die vereist voor elke container
- Network-eindpunten
- Verwijzingen naar andere resources, zoals netwerken, volumes en geheimen 

Alle codepakketten die worden gedefinieerd als onderdeel van de bron van een Service worden geïmplementeerd en geactiveerd als een groep. Resource voor de Service beschrijft ook hoeveel exemplaren van de service om uit te voeren en andere bronnen (bijvoorbeeld netwerkbron) Dit is afhankelijk van een verwijzing naar.

Als een toepassing NET uit meer dan één Service bestaat, zijn ze niet gegarandeerd om uit te voeren bij elkaar op hetzelfde knooppunt. Tijdens een upgrade van de toepassing ook resulteert fout van het upgraden van een enkele Service in alle Services die worden teruggedraaid naar de vorige versie.

Zoals eerder alluded, kan de levenscyclus van elke toepassingsinstantie onafhankelijk worden beheerd. Bijvoorbeeld, kan één exemplaar van de toepassing onafhankelijk van de andere exemplaren van de toepassing worden bijgewerkt. Normaal gesproken u het aantal services in een toepassing vrij klein, zoals de meer services die u in een toepassing, het moeilijker wordt plaatst voor het beheren van elke service onafhankelijk van elkaar.

## <a name="networks"></a>Netwerken

Netwerkbron wordt afzonderlijk implementeerbare resource, onafhankelijk van een toepassing of Service-resource die als hun afhankelijkheid naar deze verwijzen kan. Het wordt gebruikt voor het maken van een netwerk voor uw toepassingen. Meerdere Services van verschillende toepassingen kunnen deel uitmaken van hetzelfde netwerk.  Lees voor meer informatie over [networking in Service Fabric-NET-toepassingen](service-fabric-mesh-networks-and-gateways.md).

> [!NOTE]
> De huidige Preview-versie ondersteunt alleen een één-op-een-toewijzing tussen toepassingen en -netwerken

![Netwerk- en gateway][Image2]

## <a name="gateways"></a>Gateways
Een resource van een twee netwerken verbindt en routeert verkeer.  Een gateway kunt uw services om te communiceren met externe clients en biedt een inkomend verkeer naar uw service (s).  Een gateway kan ook worden gebruikt om uw toepassing net met uw eigen, bestaande virtuele netwerk verbinding te maken. Lees voor meer informatie over [networking in Service Fabric-NET-toepassingen](service-fabric-mesh-networks-and-gateways.md).

![Netwerk- en gateway][Image2]

## <a name="secrets"></a>Geheimen

Geheimen resources zijn geïmplementeerd, onafhankelijk van een toepassing of Serviceresource die als hun afhankelijkheid naar deze verwijzen kan. Deze wordt gebruikt om geheimen veilig leveren aan uw toepassingen. Meerdere services van andere toepassingen kunnen verwijzen naar waarden van hetzelfde geheim.

## <a name="volumes"></a>Volumes

Containers maken vaak tijdelijke schijven beschikbaar. Tijdelijke schijven zijn echter kortstondige, zodat u een nieuwe tijdelijke schijf ophalen en de gegevens gaan verloren wanneer een container vastloopt. Het is ook moeilijk om gegevens op schijven met tijdelijke delen met andere containers. Volumes zijn mappen die in uw containerinstanties die u gebruiken kunt om vast te leggen de status worden gekoppeld. Volumes bieden u de opslag voor algemeen gebruik en kunnen u bestanden met behulp van API's van een normale schijf i/o-bestand voor lezen/schrijven. De bron Volume is een declaratieve manier om te beschrijven hoe u een map is gekoppeld en de opslag van de back-ups maken voor deze (Azure-bestanden Volume of Service Fabric betrouwbare Volume).  Lees voor meer informatie, [status opslaan](service-fabric-mesh-storing-state.md#volumes).

![Volumes][Image3]

## <a name="programming-models"></a>Programmeringsmodellen
Serviceresource vereist alleen een containerinstallatiekopie uit te voeren, waarnaar wordt verwezen in de code-pakketten die zijn gekoppeld aan de resource. U kunt de code die is geschreven in elke taal, uitvoeren van elk gewenst framework in de container zonder te weten of net van Service Fabric-specifieke API's gebruiken. 

Code van uw toepassing blijft zelfs buiten een Service Fabric NET draagbare en uw implementaties van toepassingen blijven consistent, ongeacht de taal of framework gebruikt voor het implementeren van uw services. Of uw toepassing ASP.NET Core, Go, of alleen een set van processen en scripts is, blijft het implementatiemodel van Service Fabric NET Resource hetzelfde. 

## <a name="packaging-and-deployment"></a>Verpakking en implementatie

Service Fabric-NET-toepassingen op basis van de resourcemodel zijn verpakt als Docker-containers.  Service Fabric-NET is een gedeelde omgeving met meerdere tenants en containers bieden u een hoge mate van isolatie.  Deze toepassingen worden beschreven met behulp van een JSON-indeling of een YAML-indeling (die vervolgens wordt omgezet naar JSON). Bij het implementeren van een Mesh-toepassing in Azure Service Fabric NET, is de JSON die wordt gebruikt om te beschrijven van de toepassing een Azure Resource Manager-sjabloon. Resources worden toegewezen aan Azure-resources.  Bij het implementeren van een Mesh-toepassing naar een Service Fabric-cluster (zelfstandige of wordt gehost op Azure), de JSON die wordt gebruikt om te beschrijven van de toepassing is een indeling die vergelijkbaar is met een Azure Resource Manager-sjabloon.  Zodra geïmplementeerd, kunnen NET-toepassingen worden beheerd via HTTP-interfaces of Azure CLI. 


## <a name="next-steps"></a>Volgende stappen 
Lees het overzicht voor meer informatie over Service Fabric Mesh:
- [Service Fabric Mesh - overzicht](service-fabric-mesh-overview.md)

[Image1]: media/service-fabric-mesh-service-fabric-resources/AppsAndServices.png
[Image2]: media/service-fabric-mesh-service-fabric-resources/NetworkAndGateway.png
[Image3]: media/service-fabric-mesh-service-fabric-resources/volumes.png
