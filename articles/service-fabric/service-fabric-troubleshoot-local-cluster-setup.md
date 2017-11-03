---
title: Problemen met uw lokale installatie van de Service Fabric-cluster | Microsoft Docs
description: In dit artikel wordt een reeks suggesties voor het oplossen van uw lokaal ontwikkelcluster behandeld.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 97f4feaa-bba0-47af-8fdd-07f811fe2202
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: aa393f884b564cee81fcf75cc2eff895efea9471
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a>Installatie van uw lokale ontwikkeling oplossen
Wanneer u een probleem tijdens interactie met uw lokale cluster van de Azure Service Fabric-ontwikkeling, controleert u de volgende suggesties voor mogelijke oplossingen.

## <a name="cluster-setup-failures"></a>Setup van clusters
### <a name="cannot-clean-up-service-fabric-logs"></a>Kan niet opschonen van Service Fabric-Logboeken
#### <a name="problem"></a>Probleem
Tijdens het uitvoeren van het script DevClusterSetup, ziet u een fout als volgt:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Oplossing
Het huidige PowerShell-venster sluiten en open een nieuw PowerShell-venster als beheerder. U moet nu mogelijk is het script wilt uitvoeren.

## <a name="cluster-connection-failures"></a>Verbindingsfouten cluster
### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Service Fabric PowerShell-cmdlets worden niet herkend in Azure PowerShell
#### <a name="problem"></a>Probleem
Als u probeert uit te voeren van de Service Fabric PowerShell-cmdlets, zoals `Connect-ServiceFabricCluster` in een Azure PowerShell-venster het mislukt, dat de cmdlet wordt niet herkend. De reden hiervoor is dat gebruikmaakt van Azure PowerShell de 32-bits versie van Windows PowerShell (zelfs op 64-bits OS-versies), terwijl de Service Fabric-cmdlets alleen in 64-bits-omgevingen werken.

#### <a name="solution"></a>Oplossing
Service Fabric-cmdlets worden altijd uitgevoerd rechtstreeks vanuit Windows PowerShell.

> [!NOTE]
> De meest recente versie van Azure PowerShell maakt geen een speciale snelkoppeling zodat dit niet langer moet gebeuren.
> 
> 

### <a name="type-initialization-exception"></a>Type initialisatie van de uitzondering
#### <a name="problem"></a>Probleem
Als u verbinding met het cluster in PowerShell maakt, ziet u de fout TypeInitializationException voor System.Fabric.Common.AppTrace.

#### <a name="solution"></a>Oplossing
De variabele path is niet correct ingesteld tijdens de installatie. Afmelden bij Windows en meld u opnieuw aan. Dit vernieuwt pad naar uw.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Cluster-verbinding is mislukt met '-Object is gesloten'
#### <a name="problem"></a>Probleem
Een aanroep van Connect-ServiceFabricCluster mislukt met een fout als volgt:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Oplossing
Het huidige PowerShell-venster sluiten en open een nieuw PowerShell-venster als beheerder. U moet nu mogelijk om verbinding te maken.

### <a name="fabric-connection-denied-exception"></a>Verbinding geweigerd fabric-uitzondering
#### <a name="problem"></a>Probleem
Als u foutopsporing van Visual Studio, krijgt u een FabricConnectionDeniedException-fout.

#### <a name="solution"></a>Oplossing
Deze fout treedt meestal op wanneer u probeert een hostproces van de service handmatig te starten in plaats van de Service Fabric-runtime start deze voor u.

Zorg ervoor dat er geen serviceprojecten die zijn ingesteld als opstartprojecten in uw oplossing. Alleen de projecten van Service Fabric-toepassing moeten worden ingesteld als opstartprojecten.

> [!TIP]
> Als na de installatie, uw lokale cluster abnormaal gedrag begint, kunt u het opnieuw met behulp van de lokale cluster manager systeemtoepassing lade instellen. Hiermee verwijdert u het bestaande cluster en het instellen van een nieuwe. Houd er rekening mee dat alle toepassingen geïmplementeerde en bijbehorende gegevens worden verwijderd.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Begrijpen en oplossen van het cluster met systeemstatusrapporten](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Een cluster visualiseren met Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)

