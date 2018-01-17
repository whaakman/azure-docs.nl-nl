---
title: Uw cluster met behulp van Azure Service Fabric Explorer visualiseren | Microsoft Docs
description: Service Fabric Explorer is een verzoek om te bekijken en beheren van cloudtoepassingen en -knooppunten in een Microsoft Azure Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/08/2018
ms.author: mikhegn
ms.openlocfilehash: 34e00058591bc5a0a02bc408cfc3fcc11010f17c
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Visualisern van uw cluster met Service Fabric Explorer

Service Fabric Explorer (SFX) is een open source-hulpprogramma te bekijken en beheren van Azure Service Fabric-clusters. Service Fabric Explorer is een bureaubladtoepassing voor Windows en Linux. Ondersteuning voor Mac OS is binnenkort beschikbaar.

## <a name="service-fabric-explorer-download"></a>Service Fabric Explorer downloaden

Gebruik de volgende koppelingen voor het downloaden van Service Fabric Explorer als een bureaubladtoepassing:

- Windows
  - https://aka.ms/sfx-windows

- Linux
  - https://aka.ms/sfx-linux-x86
  - https://aka.ms/sfx-linux-x64

> [!NOTE]
> De bureaubladversie van Service Fabric Explorer kan meer of minder mogelijkheden dan de clusterondersteuning hebben. U kunt terugvallen op de versie van de Service Fabric Explorer geïmplementeerd op het cluster volledig functie compatibiliteit te garanderen.
>
>

### <a name="running-service-fabric-explorer-from-the-cluster"></a>Service Fabric Explorer uitvoeren vanaf het cluster

Service Fabric Explorer wordt ook in een Service Fabric-cluster HTTP-eindpunt gehost. Als u wilt starten SFX in een webbrowser, blader naar HTTP-eindpunt van het cluster vanuit elke browser - bijvoorbeeld https://clusterFQDN:19080.

Voor ontwikkelaars werkstation setup, kunt u op uw lokale cluster Service Fabric Explorer starten door te navigeren naar http://localhost: 19080/Explorer. Dit artikel om te kijken [uw ontwikkelingsomgeving voorbereiden](service-fabric-get-started.md).

## <a name="connect-to-a-service-fabric-cluster"></a>Verbinding maken met een Service Fabric-cluster
Voor verbinding met een Service Fabric-cluster, moet u de clusters eindpunt (FQDN-naam/het IP-) en het hulpprogramma voor het eindpunt van de HTTP-beheerpoort (19080 standaard). Bijvoorbeeld https://mysfcluster.westus.cloudapp.azure.com:19080. Gebruik het selectievakje "Localhost verbinden" verbinding maken met een lokaal cluster op uw werkstation.

### <a name="connect-to-a-secure-cluster"></a>Verbinding maken met een beveiligd cluster
U kunt de clienttoegang beheren met uw Service Fabric-cluster met certificaten of met behulp van Azure Active Directory (AAD).

Als u probeert verbinding maken met een beveiligde cluster, vervolgens afhankelijk van de configuratie van het cluster moet u een clientcertificaat of meld u aan met AAD.

## <a name="video-tutorial"></a>Zelfstudievideo

Voor informatie over het gebruik van Service Fabric Explorer, bekijk de volgende video voor Microsoft Virtual Academy:

> [!NOTE]
> Deze video ziet dat service Fabric Explorer gehost in een Service Fabric-cluster, de bureaubladversie.
>
>

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="understand-the-service-fabric-explorer-layout"></a>Inzicht in de indeling van de Service Fabric Explorer
U kunt via Service Fabric Explorer navigeren met behulp van de structuur aan de linkerkant. In de hoofdmap van de structuur overzicht cluster-dashboard van uw cluster, inclusief een overzicht van de toepassing en het knooppunt status.

![Service Fabric Explorer cluster-dashboard][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>De indeling van het cluster weergeven
Knooppunten in een Service Fabric-cluster worden geplaatst in een tweedimensionale raster van domeinen met fouten en upgradedomeinen. Deze plaatsing zorgt ervoor dat uw toepassingen beschikbaar in de aanwezigheid van hardwarefouten en toepassingsupgrades blijven. U kunt bekijken hoe de huidige cluster met behulp van de cluster-kaart worden verspreid.

![Service Fabric Explorer clustertoewijzing][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Weergave-toepassingen en services
Het cluster bevat twee substructuren: één voor toepassingen en andere voor knooppunten.

U kunt de toepassingsweergave om te navigeren door logische hiërarchie van Service Fabric: toepassingen, services, partities en replica's.

In het volgende voorbeeld wordt de toepassing **MyApp** bestaat uit twee services **MyStatefulService** en **WebService**. Aangezien **MyStatefulService** stateful is, bevat een partitie met een primaire en twee secundaire replica's. Daarentegen is WebSvcService staatloze en slechts één exemplaar bevat.

![Service Fabric Explorer toepassing weergeven][sfx-application-tree]

Op elk niveau van de structuur bevat het hoofdvenster relevante informatie over het item. Bijvoorbeeld, ziet u de health-status en de versie voor een bepaalde service.

![Service Fabric Explorer essentials deelvenster][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Knooppunten van het cluster weergeven
In de knooppuntweergave ziet u de fysieke indeling van het cluster. Voor elk knooppunt kunt u controleren voor welke toepassingen er op het knooppunt code is geïmplementeerd. Meer specifiek, kunt u zien welke replica er momenteel worden uitgevoerd.

## <a name="actions"></a>Acties
Service Fabric Explorer biedt een snelle manier om aan te roepen acties op de knooppunten, toepassingen en services in uw cluster.

Bijvoorbeeld, als u wilt verwijderen van instantie van een toepassing, kies de toepassing uit de structuur aan de linkerkant en kies vervolgens **acties** > **toepassing verwijderen**.

![Verwijderen van een toepassing in Service Fabric Explorer][sfx-delete-application]

> [!TIP]
> U kunt dezelfde acties uitvoeren door te klikken op het weglatingsteken naast elk element.
>
> Elke actie die kan worden uitgevoerd via Service Fabric Explorer kan ook worden uitgevoerd via PowerShell of REST-API om te automatiseren.
>
>

U kunt ook een Service Fabric Explorer gebruiken om toepassingsexemplaren voor een bepaalde toepassingstype en de versie te maken. Kies het toepassingstype in de structuurweergave en klik op de **app-exemplaar maken** koppeling naast de versie die u in het rechterdeelvenster dat wilt.

![Instantie van een toepassing maken in Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> Service Fabric Explorer biedt geen ondersteuning voor parameters bij het maken van exemplaren van een toepassing. Exemplaren van een toepassing gebruik standaardwaarden voor parameters.
>
>

## <a name="next-steps"></a>Volgende stappen
* [Het beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Implementatie van de service Fabric-toepassing met behulp van PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png