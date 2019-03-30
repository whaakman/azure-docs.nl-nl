---
title: Een bestaand uitvoerbaar bestand implementeren naar Azure Service Fabric | Microsoft Docs
description: Meer informatie over het inpakken van een bestaande toepassing als gast uitvoerbare bestanden, zodat deze kan worden geïmplementeerd in een Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 03/15/2018
ms.author: aljo
ms.openlocfilehash: b7efeb1b4d83f6a6b372f73a7c0a5ca9bffdc052
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58670590"
---
# <a name="deploy-an-existing-executable-to-service-fabric"></a>Een bestaand uitvoerbaar bestand implementeren naar Service Fabric
Als een service kunt u elk type code, zoals Node.js, Java of C++ uitvoeren in Azure Service Fabric. Service Fabric verwijst naar deze typen services als uitvoerbare gastbestanden.

Gasten uitvoerbare bestanden worden, zoals stateless services door Service Fabric behandeld. Als gevolg hiervan, worden deze geplaatst op knooppunten in een cluster, op basis van beschikbaarheid en andere metrische gegevens. In dit artikel wordt beschreven hoe u verpakken en implementeren van een door gasten uitvoerbare bestanden naar een Service Fabric-cluster met behulp van Visual Studio of een opdrachtregelprogramma.

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Voordelen van het uitvoeren van een door gasten uitvoerbare bestanden in Service Fabric
Er zijn diverse voordelen voor het uitvoeren van een door gasten uitvoerbare bestanden in een Service Fabric-cluster:

* Hoge beschikbaarheid. Toepassingen die worden uitgevoerd in Service Fabric worden maximaal beschikbaar gemaakt. Service Fabric zorgt ervoor dat de exemplaren van een toepassing worden uitgevoerd.
* Statuscontrole. Service Fabric-statuscontrole detecteert als een toepassing wordt uitgevoerd, en diagnostische gegevens biedt als er een storing optreedt.   
* Levensduurbeheer van toepassingen. Service Fabric biedt naast het invoeren van upgrades zonder downtime, automatisch teruggedraaid naar de vorige versie als er een ongeldige statusgebeurtenis gerapporteerd tijdens een upgrade.    
* Dichtheid. U kunt meerdere toepassingen uitvoeren in een cluster, wat elimineert de noodzaak voor elke toepassing uit te voeren op een eigen hardware.
* Detectie: U kunt de Service Fabric Naming-service voor het vinden van andere services in het cluster met behulp van REST aanroepen. 

## <a name="samples"></a>Voorbeelden
* [Voorbeeld voor het verpakken en implementeren van een Gast kan worden uitgevoerd](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Voorbeeld van twee Gast uitvoerbare bestanden (C# en nodejs) communiceren via de Naming-service met behulp van REST](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a>Overzicht van de toepassing en service-manifestbestanden
Als onderdeel van de implementatie van een Gast kan worden uitgevoerd, is het handig om te begrijpen van de Service Fabric-verpakking en implementatie van het model zoals beschreven in [toepassingsmodel](service-fabric-application-model.md). Het model van Service Fabric verpakken, is afhankelijk van twee XML-bestanden: de toepassing en service-manifesten. De schemadefinitie voor de bestanden ApplicationManifest.xml en ServiceManifest.xml is geïnstalleerd met de Service Fabric SDK in *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Toepassingsmanifest** manifest voor de toepassing wordt gebruikt om te beschrijven van de toepassing. Geeft een lijst van de services die samen deze vormen en andere parameters die worden gebruikt om te definiëren hoe een of meer services moeten worden geïmplementeerd, zoals het aantal exemplaren.

  In Service Fabric is een toepassing een eenheid van de implementatie en upgrade. Een toepassing kan worden bijgewerkt als één eenheid waar mogelijke fouten en mogelijke terugdraaiacties worden beheerd. Service Fabric zorgt ervoor dat het upgradeproces een is geslaagd, of als de upgrade mislukt, laat de toepassing in een onbekende of instabiele status.
* **Servicemanifest** het servicemanifest beschrijft de onderdelen van een service. Het bevat gegevens, zoals de naam en type van de service, en de code en configuratie. Het servicemanifest bevat ook enkele extra parameters die kunnen worden gebruikt voor het configureren van de service nadat deze is geïmplementeerd.

## <a name="application-package-file-structure"></a>Bestandsstructuur Application-pakket
De toepassing moet een vooraf gedefinieerde mapstructuur Volg voor het implementeren van een toepassing in Service Fabric. Hier volgt een voorbeeld van die structuur.

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

De ApplicationPackageRoot bevat het bestand ApplicationManifest.xml waarin de toepassing. Een submap voor elke service die is opgenomen in de toepassing wordt gebruikt om de artefacten die is vereist voor de service bevatten. Deze submappen zijn de ServiceManifest.xml en normaal gesproken de volgende:

* *Code*. Deze map bevat de servicecode.
* *Config*. Deze map bevat een Settings.xml-bestand (en andere bestanden indien nodig) of de service toegang hebt tot tijdens runtime specifieke configuratie-instellingen op te halen.
* *Data*. Dit is een extra map voor het opslaan van aanvullende lokale gegevens die de service moet mogelijk. Gegevens moeten worden gebruikt voor het opslaan van alleen tijdelijke gegevens. Service Fabric niet kopiëren of repliceren van wijzigingen naar de gegevensmap als de service moet worden verplaatst (bijvoorbeeld tijdens failover).

> [!NOTE]
> U hoeft te maken de `config` en `data` mappen als u deze niet nodig hebt.
>
>

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor gerelateerde informatie en taken.
* [Een toepassing implementeren die door een gast kan worden uitgevoerd](service-fabric-deploy-existing-app.md)
* [Meerdere toepassingen implementeren die door gasten kunnen worden uitgevoerd](service-fabric-deploy-multiple-apps.md)
* [Uw eerste Gast uitvoerbare toepassing met Visual Studio maken](quickstart-guest-app.md)
* [Voorbeeld voor het verpakken en implementeren van een Gast kan worden uitgevoerd](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), met inbegrip van een koppeling naar de voorlopige versie van het hulpprogramma verpakking
* [Voorbeeld van twee Gast uitvoerbare bestanden (C# en nodejs) communiceren via de Naming-service met behulp van REST](https://github.com/Azure-Samples/service-fabric-containers)

