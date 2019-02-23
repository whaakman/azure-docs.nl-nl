---
title: Uw cluster visualiseren met Azure Service Fabric Explorer | Microsoft Docs
description: Service Fabric Explorer is een toepassing voor het inspecteren en beheren van cloudtoepassingen en knooppunten in een Microsoft Azure Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: ''
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2019
ms.author: mikhegn
ms.openlocfilehash: ba6a9a09945f0e822dfe02a74f2cdb8e21a1691c
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56674664"
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Visualisern van uw cluster met Service Fabric Explorer

Service Fabric Explorer (SFX) is een open-source-hulpprogramma voor het inspecteren en beheren van Azure Service Fabric-clusters. Service Fabric Explorer is een bureaubladtoepassing voor Windows, macOS en Linux.

## <a name="service-fabric-explorer-download"></a>Service Fabric Explorer downloaden

Gebruik de volgende koppelingen voor het downloaden van Service Fabric Explorer als een bureaubladtoepassing:

- Windows
  - https://aka.ms/sfx-windows

- Linux
  - https://aka.ms/sfx-linux-x86
  - https://aka.ms/sfx-linux-x64

- macOS
  - https://aka.ms/sfx-macos

> [!NOTE]
> De bureaubladversie van Service Fabric Explorer kan meer of minder functies dan de clusterondersteuning hebben. U kunt terugschakelen naar de versie van de Service Fabric Explorer geïmplementeerd in het cluster volledige functiecompatibiliteit te garanderen.
>
>

### <a name="running-service-fabric-explorer-from-the-cluster"></a>Service Fabric Explorer uitvoeren vanaf het cluster

Service Fabric Explorer is ook die wordt gehost in een Service Fabric-cluster HTTP-eindpunt. Als u wilt starten SFX in een webbrowser, blader naar HTTP-eindpunt van het cluster vanuit elke browser - bijvoorbeeld https://clusterFQDN:19080.

Voor instellingen voor ontwikkelaars werkstation, kunt u Service Fabric Explorer starten op uw lokale cluster door te navigeren naar https://localhost:19080/Explorer. Dit artikel om te kijken [uw ontwikkelomgeving voorbereiden](service-fabric-get-started.md).

> [!NOTE]
> Als uw cluster is beveiligd door een zelfondertekend certificaat dat u ontvangt een foutbericht van de webbrowser is' deze site niet beveiligd'. U kunt gewoon doorgaan via de meeste moderne webbrowsers door de waarschuwing overschrijven. In een productieomgeving moet uw cluster zijn beveiligd met behulp van de algemene naam en een certificaat uitgegeven certificaat. 
>
>

## <a name="connect-to-a-service-fabric-cluster"></a>Verbinding maken met een Service Fabric-cluster
Voor verbinding met een Service Fabric-cluster, moet u het eindpunt voor clusters (FQDN-naam/IP-) en de HTTP-eindpunt-beheerpoort (19080 standaard). Bijvoorbeeld https\:/ / mysfcluster.westus.cloudapp.azure.com:19080. Gebruik het selectievakje 'Verbinding maken met localhost' verbinding maken met een lokaal cluster op uw werkstation.

### <a name="connect-to-a-secure-cluster"></a>Verbinding maken met een beveiligd cluster
U kunt clienttoegang tot uw Service Fabric-cluster aan certificaten of met behulp van Azure Active Directory (AAD) beheren.

Als u probeert om verbinding met een beveiligd cluster te maken, klikt u vervolgens afhankelijk van de configuratie van het cluster moet u een clientcertificaat of zich aanmelden met AAD.

## <a name="understand-the-service-fabric-explorer-layout"></a>Inzicht in de indeling van de Service Fabric Explorer
U kunt navigeren via de Service Fabric Explorer met behulp van de boomstructuur aan de linkerkant. In de hoofdmap van de structuur bevat het clusterdashboard een overzicht van het cluster, inclusief een overzicht van de toepassings- en knooppuntstatus.

![Service Fabric Explorer cluster-dashboard][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>De indeling van het cluster weergeven
Knooppunten in een Service Fabric-cluster worden geplaatst in een tweedimensionale raster van foutdomeinen en upgradedomeinen. Deze plaatsing zorgt ervoor dat uw toepassingen met problemen met de hardware en upgrades van toepassingen beschikbaar blijven. U kunt bekijken hoe het huidige cluster is ingedeeld met behulp van de cluster-kaart.

![Service Fabric Explorer cluster-kaart][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Bekijk toepassingen en services
Het cluster bevat twee substructuren: één voor toepassingen en andere knooppunten.

U kunt de toepassingsweergave gebruiken om te navigeren door de Service Fabric logische hiërarchie: toepassingen, services, partities en replica's.

In het volgende voorbeeld wordt de toepassing **Mijntoep** bestaat uit twee services **MyStatefulService** en **WebService**. Aangezien **MyStatefulService** stateful is, is het een partitie met één primair knooppunt en twee secundaire replica's bevat. Daarentegen WebSvcService is staatloos en één exemplaar bevat.

![Service Fabric Explorer toepassing weergeven][sfx-application-tree]

In het hoofdvenster ziet op elk niveau van de structuur van de relevante informatie over het item. Bijvoorbeeld, ziet u de status van en de versie voor een bepaalde service.

![Service Fabric Explorer essentials-deelvenster][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Knooppunten van het cluster weergeven
In de knooppuntweergave ziet u de fysieke indeling van het cluster. Voor elk knooppunt kunt u controleren voor welke toepassingen er op het knooppunt code is geïmplementeerd. Meer specifiek, kunt u zien welke replica's er momenteel worden uitgevoerd.

## <a name="actions"></a>Acties
Service Fabric Explorer biedt een snelle manier om aan te roepen acties op de knooppunten, toepassingen en services in uw cluster.

Bijvoorbeeld, als u wilt verwijderen van exemplaar van een toepassing, kiest u de toepassing uit de structuur aan de linkerkant en kies vervolgens **acties** > **Delete Application**.

![Verwijderen van een toepassing in Service Fabric Explorer][sfx-delete-application]

> [!TIP]
> U kunt dezelfde acties uitvoeren door te klikken op het weglatingsteken naast elk element.
>
> Elke actie die kan worden uitgevoerd via de Service Fabric Explorer kan ook worden uitgevoerd via PowerShell of een REST-API-automatisering inschakelen.
>
>

U kunt ook Service Fabric Explorer gebruiken om instanties van de toepassing voor een bepaald type en de versie te maken. Kies het toepassingstype in de structuurweergave wordt weergegeven en klik vervolgens op de **maken-app-exemplaar** koppelen naast de versie die u in het rechter deelvenster dat wilt.

![Het exemplaar van een toepassing maken in Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> Service Fabric Explorer biedt geen ondersteuning voor parameters bij het maken van exemplaren van de toepassing. Exemplaren van een toepassing gebruik standaardwaarden voor parameters.
>
>

## <a name="event-store"></a>Gebeurtenis-Store
EventStore is een functie die wordt aangeboden door het platform waarmee u Service Fabric-platformgebeurtenissen beschikbaar in de Service Fabric Explorer en via REST-API. Hier ziet u een momentopname van wat in uw cluster voor elke entiteit bijvoorbeeld gebeurt er knooppunt, service, toepassing en query's op basis van de tijd van de gebeurtenis. U kunt ook meer informatie over de EventStore op de [overzicht van EventStore](service-fabric-diagnostics-eventstore.md).   

![EventStore][sfx-eventstore]

>[!NOTE]
>Vanaf het Service Fabric versie 6.4. EventStore is niet standaard ingeschakeld en moet zijn ingeschakeld in het resource manager-sjabloon

>[!NOTE]
>Vanaf het Service Fabric versie 6.4. De APIs EventStore zijn alleen beschikbaar voor Windows-clusters wordt alleen uitgevoerd op Azure. Er wordt gewerkt aan het overzetten van deze functionaliteit voor zowel Linux als onze zelfstandige clusters.


## <a name="next-steps"></a>Volgende stappen
* [Beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Implementatie van service Fabric-toepassing met behulp van PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
[sfx-eventstore]: ./media/service-fabric-diagnostics-eventstore/eventstore.png
