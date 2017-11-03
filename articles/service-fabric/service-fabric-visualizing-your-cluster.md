---
title: Uw cluster met behulp van Service Fabric Explorer visualiseren | Microsoft Docs
description: Service Fabric Explorer is een Webhulpprogramma voor te bekijken en beheren van cloudtoepassingen en -knooppunten in een Microsoft Azure Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: ryanwi
ms.openlocfilehash: 965ffc0f8cec26cccbe6e6459731afc234111f4d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Visualisern van uw cluster met Service Fabric Explorer
Service Fabric Explorer is een Webhulpprogramma voor te bekijken en beheren van toepassingen en de knooppunten in een Azure Service Fabric-cluster. Service Fabric Explorer wordt gehost rechtstreeks in het cluster, zodat deze altijd beschikbaar is, ongeacht waar uw cluster wordt uitgevoerd.

## <a name="video-tutorial"></a>Zelfstudievideo

Voor informatie over het gebruik van Service Fabric Explorer, bekijk de volgende video voor Microsoft Virtual Academy:

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-to-service-fabric-explorer"></a>Verbinding maken met Service Fabric Explorer
Als u de instructies voor het hebt gevolgd [uw ontwikkelingsomgeving voorbereiden](service-fabric-get-started.md), kunt u de Service Fabric Explorer starten op uw lokale cluster door te navigeren naar http://localhost: 19080/Explorer.

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
>

De volgende tabel bevat de acties die beschikbaar zijn voor elke entiteit:

| **Entiteit** | **Actie** | **Beschrijving** |
| --- | --- | --- |
| Toepassingstype |Type inrichting |Hiermee verwijdert u het toepassingspakket uit het archief van de installatiekopie van het cluster. Vereist dat alle toepassingen van dat type moet eerst worden verwijderd. |
| Toepassing |Toepassing verwijderen |De toepassing, met inbegrip van alle services en hun status (indien aanwezig) verwijderen. |
| Service |Verwijderen van de Service |De service en de status verwijderd (indien aanwezig). |
| Knooppunt |Activeren |Activeren van het knooppunt. |
| Knooppunt | Deactiveren (pause) | Het knooppunt in de huidige status onderbreken. Services kunnen blijven uitvoeren maar Service Fabric wordt niet proactief verplaatst alles naar of uit deze tenzij deze is vereist om te voorkomen dat een inconsistentie onderbreking of gegevens. Deze actie wordt meestal gebruikt voor foutopsporing services op een specifiek knooppunt om ervoor te zorgen dat ze niet tijdens controle verplaatsen in te schakelen. | |
| Knooppunt | (Opnieuw opstarten) deactiveren | Veilig Verplaats alle in het geheugen services een knooppunt uit en sluit permanente services. Doorgaans gebruikt wanneer de hostprocessen of de computer moet opnieuw worden opgestart. | |
| Knooppunt | Deactiveren (gegevens verwijderen) | Alle services die worden uitgevoerd op het knooppunt na het bouwen van voldoende spare replica's sluiten. Doorgaans gebruikt wanneer een knooppunt (of ten minste bijbehorende opslag) is buiten het Commissie permanent genomen. | |
| Knooppunt | Status van knooppunt verwijderen | Kennis van replica's van een knooppunt uit het cluster verwijdert. Doorgaans gebruikt wanneer een knooppunt al onherstelbare wordt geacht. | |
| Knooppunt | Opnieuw starten | Een knooppuntfout simuleren door het knooppunt opnieuw te starten. Meer informatie [hier](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps) | |

Aangezien veel acties destructieve, wordt u mogelijk gevraagd om te bevestigen dat uw bedoeling voordat de actie is voltooid.

> [!TIP]
> Elke actie die kan worden uitgevoerd via Service Fabric Explorer kan ook worden uitgevoerd via PowerShell of REST-API om te automatiseren.
>
>

U kunt ook een Service Fabric Explorer gebruiken om toepassingsexemplaren voor een bepaalde toepassingstype en de versie te maken. Kies het toepassingstype in de structuurweergave en klik op de **app-exemplaar maken** koppeling naast de versie die u in het rechterdeelvenster dat wilt.

![Instantie van een toepassing maken in Service Fabric Explorer][sfx-create-app-instance]

> [!NOTE]
> Exemplaren van een toepassing gemaakt via Service Fabric Explorer kunnen niet op dit moment parameters worden gebruikt. Ze zijn gemaakt met behulp van de standaardwaarden voor parameters.
>
>

## <a name="connect-to-a-remote-service-fabric-cluster"></a>Verbinding maken met een externe Service Fabric-cluster
Als u eindpunt van het cluster kent en voldoende machtigingen hebt u toegang tot Service Fabric Explorer vanuit elke browser. Dit komt doordat het Service Fabric Explorer is gewoon een service die wordt uitgevoerd in het cluster.

### <a name="discover-the-service-fabric-explorer-endpoint-for-a-remote-cluster"></a>Het eindpunt van de Service Fabric Explorer voor een cluster met externe detecteren
Service Fabric Explorer voor een bepaald cluster bereiken, wijst u uw browser:

http://&lt;uw cluster-eindpunt&gt;: 19080/Explorer

De volledige URL is ook beschikbaar in het cluster essentials deelvenster van de Azure portal voor Azure-clusters.

### <a name="connect-to-a-secure-cluster"></a>Verbinding maken met een beveiligd cluster
U kunt de clienttoegang beheren met uw Service Fabric-cluster met certificaten of met behulp van Azure Active Directory (AAD).

Als u probeert verbinding maken met Service Fabric Explorer op een beveiligde cluster, vervolgens afhankelijk van de configuratie van het cluster u gevraagd een clientcertificaat of meld u aan met AAD.

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van testbaarheid](service-fabric-testability-overview.md)
* [Het beheren van uw Service Fabric-toepassingen in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
* [Implementatie van de service Fabric-toepassing met behulp van PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
