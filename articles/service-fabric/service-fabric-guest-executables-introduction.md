---
title: Implementeren van een bestaand uitvoerbaar bestand op Azure Service Fabric | Microsoft Docs
description: "Meer informatie over het verpakken van een bestaande toepassing als gast uitvoerbare, zodat het kan worden geïmplementeerd naar een Service Fabric-cluster."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 03/15/2018
ms.author: mfussell;mikhegn
ms.openlocfilehash: 328c00697a3c81f5af8488d4303feb7618d81301
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="deploy-an-existing-executable-to-service-fabric"></a>Implementeren van een bestaand uitvoerbaar bestand voor Service Fabric
Als een service kunt u verschillende typen code, zoals Node.js, Java of C++ uitvoeren in Azure Service Fabric. Service Fabric verwijst naar deze soorten services als gast uitvoerbare bestanden.

Gast uitvoerbare bestanden worden behandeld door het Service Fabric zoals stateless services. Als gevolg hiervan worden deze geplaatst op knooppunten in een cluster, op basis van beschikbaarheid en andere metrische gegevens. In dit artikel wordt beschreven hoe het pakket en een gast uitvoerbare aan een Service Fabric-cluster implementeren met behulp van Visual Studio of een opdrachtregelprogramma.

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Voordelen van het uitvoeren van een gast uitvoerbare in Service Fabric
Er zijn enkele voordelen voor het uitvoeren van een gast uitvoerbare in een Service Fabric-cluster:

* Hoge beschikbaarheid. Toepassingen die worden uitgevoerd in Service Fabric zijn maximaal beschikbaar is gemaakt. Service Fabric zorgt ervoor dat de exemplaren van een toepassing worden uitgevoerd.
* Statuscontrole. Service Fabric-statuscontrole detecteert als een toepassing wordt uitgevoerd, en bevat de diagnostische gegevens als er een storing optreedt.   
* Levenscyclus van Toepassingsbeheer. Service Fabric biedt automatisch herstel naar de vorige versie naast het bieden van upgrades zonder uitvaltijd, als er een ongeldige statusgebeurtenis gerapporteerd tijdens een upgrade.    
* Dichtheid. U kunt meerdere toepassingen uitvoeren in een cluster die niet meer nodig voor elke toepassing uitvoeren op een eigen hardware.
* U kunt de naamgeving van Service Fabric-service om andere services in het cluster zichtbaarheid: Met behulp van REST aanroepen. 

## <a name="samples"></a>Voorbeelden
* [Voorbeeld voor verpakken en distribueren van een gast uitvoerbaar bestand](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Voorbeeld van twee Gast uitvoerbare bestanden (C# en nodejs) communiceren via de Naming service met behulp van REST](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a>Overzicht van de toepassing en service manifest-bestanden
Als onderdeel van de implementatie van een gast uitvoerbaar bestand is het nuttig om het Service Fabric-model voor pakketten en implementatie begrijpen, zoals beschreven in [toepassingsmodel](service-fabric-application-model.md). Het model van Service Fabric-pakket is afhankelijk van de twee XML-bestanden: de toepassing en service manifesten. De schemadefinitie voor de bestanden ApplicationManifest.xml en ServiceManifest.xml is geïnstalleerd met de Service Fabric SDK in *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Het toepassingsmanifest** het toepassingsmanifest wordt gebruikt om de toepassing te beschrijven. Geeft een lijst van de services die het opstellen en andere parameters die worden gebruikt om te definiëren hoe een of meer services moeten worden geïmplementeerd, zoals het aantal exemplaren.

  Een toepassing is in Service Fabric eenheid van de implementatie en upgrade. Een toepassing kan worden bijgewerkt als één eenheid waar de potentiële fouten en mogelijke Rollback worden beheerd. Service Fabric zorgt ervoor dat het upgradeproces een is geslaagd, of als de upgrade mislukt, laat de toepassing in een onbekende of onstabiele status heeft.
* **Servicemanifest** de servicemanifest beschrijving van de onderdelen van een service. Dit omvat gegevens, zoals de naam en type van service, en de code en configuratie. Het servicemanifest bevat ook een aantal extra parameters die kunnen worden gebruikt voor het configureren van de service nadat deze is geïmplementeerd.

## <a name="application-package-file-structure"></a>Bestandsstructuur voor Application-pakket
De toepassing moet een vooraf gedefinieerde mapstructuur Volg voor het implementeren van een Service Fabric-toepassing. Hier volgt een voorbeeld van die structuur.

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

De ApplicationPackageRoot bevat de ApplicationManifest.xml-bestand waarin de toepassing. Een submap voor elke service die is opgenomen in de toepassing wordt gebruikt voor het bevatten van alle artefacten die de service vereist. Deze submappen zijn de ServiceManifest.xml en gewoonlijk het volgende:

* *Code*. Deze map bevat de servicecode.
* *Config*. Deze map bevat een Settings.xml-bestand (en andere bestanden indien nodig) dat de service toegang heeft tot tijdens runtime specifieke configuratie-instellingen op te halen.
* *Gegevens*. Dit is een extra map voor het opslaan van aanvullende lokale gegevens die de service moet mogelijk. Gegevens moet worden gebruikt voor het opslaan van alleen kortstondige gegevens. Service Fabric niet gekopieerd of wijzigingen repliceren naar de map data als de service moet worden verplaatst (bijvoorbeeld tijdens failover).

> [!NOTE]
> U hoeft te maken van de `config` en `data` mappen als u deze niet nodig hebt.
>
>

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie en taken.
* [Een toepassing implementeren die door een gast kan worden uitgevoerd](service-fabric-deploy-existing-app.md)
* [Meerdere toepassingen implementeren die door gasten kunnen worden uitgevoerd](service-fabric-deploy-multiple-apps.md)
* [Uw eerste Gast uitvoerbare toepassing met Visual Studio maken](quickstart-guest-app.md)
* [Voorbeeld voor verpakken en distribueren van een gast uitvoerbaar bestand](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), inclusief een koppeling naar de voorlopige versie van het hulpprogramma verpakking
* [Voorbeeld van twee Gast uitvoerbare bestanden (C# en nodejs) communiceren via de Naming service met behulp van REST](https://github.com/Azure-Samples/service-fabric-containers)

