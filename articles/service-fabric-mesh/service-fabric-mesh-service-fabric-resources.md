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
ms.date: 07/12/2018
ms.author: vturecek
ms.custom: mvc, devcenter
ms.openlocfilehash: 6a96995a46cd2ad0a1afe4f4afe49d62409f2b68
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076032"
---
# <a name="introduction-to-service-fabric-resource-model"></a>Inleiding tot Service Fabric-Resourcemodel

Service Fabric-Resourcemodel beschrijft een eenvoudige benadering voor het definiëren van de resources die deel uitmaken van een Service Fabric-NET-toepassing. De volgende typen resources worden momenteel ondersteund in dit model:

- Toepassingen
- Services
- Volumes
- Netwerken

Elke resource wordt declaratief beschreven in een resource-bestand, dat een eenvoudige YAML is- of JSON-document dat de toepassing NET beschrijft en die door de Service Fabric-platform is ingericht.

## <a name="resource-overview"></a>Overzicht van bronnen

### <a name="applications-and-services"></a>Toepassingen en Services

De bron van een toepassing is de eenheid van de implementatie, versiebeheer en levensduur van een Mesh-toepassing. Deze bestaat uit een of meer van de Service-resources die een microservice vertegenwoordigen. Elke resource Service wordt op zijn beurt bestaat uit een of meer codepakketten die worden beschreven van alles wat nodig is om uit te voeren van de installatiekopie van de container zijn gekoppeld aan de codepakket, met inbegrip van de volgende:

- De containernaam van de, versie en -register
- CPU en geheugen-resources die vereist voor elke container
- Network-eindpunten
- Volumes te koppelen aan de container, die verwijst naar een ander volume-resource.

Alle codepakketten die worden gedefinieerd als onderdeel van de bron van een Service worden geïmplementeerd en geactiveerd als een groep. Resource voor de Service beschrijft ook hoeveel exemplaren van de service om uit te voeren en andere bronnen (bijvoorbeeld netwerkbron) Dit is afhankelijk van een verwijzing naar.

Als een toepassing NET uit meer dan één Service bestaat, zijn ze niet gegarandeerd om uit te voeren bij elkaar op hetzelfde knooppunt. Tijdens een upgrade van de toepassing ook resulteert fout van het upgraden van een enkele Service in alle Services die worden teruggedraaid naar de vorige versie.



```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

Zoals eerder alluded, kan de levenscyclus van elke toepassingsinstantie onafhankelijk worden beheerd. Bijvoorbeeld, kan één exemplaar van de toepassing onafhankelijk van de andere exemplaren van de toepassing worden bijgewerkt. Normaal gesproken u het aantal services in een toepassing vrij klein, zoals de meer services die u in een toepassing, het moeilijker wordt plaatst voor het beheren van elke service onafhankelijk van elkaar.

### <a name="networks"></a>Netwerken

Netwerkbron wordt afzonderlijk implementeerbare resource, onafhankelijk van een toepassing of Service-resource die als hun afhankelijkheid naar deze verwijzen kan. Het wordt gebruikt voor het maken van een particulier netwerk voor uw toepassingen. Meerdere Services van verschillende toepassingen kunnen deel uitmaken van hetzelfde netwerk.

> [!NOTE]
> De huidige Preview-versie ondersteunt alleen een één-op-een-toewijzing tussen toepassingen en -netwerken

### <a name="volumes"></a>Volumes

Volumes zijn mappen die in uw containerinstanties die u gebruiken kunt om vast te leggen de status worden gekoppeld. De bron Volume is een declaratieve manier om te beschrijven hoe u een map is gekoppeld en de opslag van de back-ups maken voor deze.

## <a name="programming-models"></a>Programmeringsmodellen
Serviceresource vereist alleen een containerinstallatiekopie uit te voeren, waarnaar wordt verwezen in de code-pakketten die zijn gekoppeld aan de resource. U kunt de code die is geschreven in elke taal, uitvoeren van elk gewenst framework in de container zonder te weten of net van Service Fabric-specifieke API's gebruiken. 

Code van uw toepassing blijft zelfs buiten een Service Fabric NET draagbare en uw implementaties van toepassingen blijven consistent, ongeacht de taal of framework gebruikt voor het implementeren van uw services. Of uw toepassing ASP.NET Core, Go, of alleen een set van processen en scripts is, blijft het implementatiemodel van Service Fabric NET Resource hetzelfde. 

## <a name="deployment"></a>Implementatie

Bij het implementeren op Service Fabric NET, kunnen Resources worden geïmplementeerd als Azure Resource Manager-sjablonen naar Azure via HTTP of de Azure CLI. 


## <a name="next-steps"></a>Volgende stappen 
Lees het overzicht voor meer informatie over Service Fabric NET:
- [Service Fabric-NET-overzicht](service-fabric-mesh-overview.md)
