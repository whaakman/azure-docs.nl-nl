---
title: Problemen met uw lokale installatie van de Azure Service Fabric-cluster | Microsoft Docs
description: In dit artikel wordt een reeks suggesties voor het oplossen van uw lokaal ontwikkelcluster behandeld.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: ''
ms.assetid: 97f4feaa-bba0-47af-8fdd-07f811fe2202
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: 6879a24df434d5bf69c9ba14aa00cdc9cd67df57
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a>Installatie van uw lokale ontwikkeling oplossen
Wanneer u een probleem tijdens interactie met uw lokale cluster van de Azure Service Fabric-ontwikkeling, controleert u de volgende suggesties voor mogelijke oplossingen.

## <a name="cluster-setup-failures"></a>Setup van clusters
### <a name="cannot-clean-up-service-fabric-logs"></a>Kan niet opschonen van Service Fabric-Logboeken
#### <a name="problem"></a>Probleem
Tijdens het uitvoeren van het script DevClusterSetup, ziet u de volgende fout:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Oplossing
Het huidige PowerShell-venster sluiten en open een nieuw PowerShell-venster als beheerder. U kunt nu het script met succes uitvoeren.

## <a name="cluster-connection-failures"></a>Verbindingsfouten cluster

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
Het huidige PowerShell-venster sluiten en open een nieuw PowerShell-venster als beheerder.

### <a name="fabric-connection-denied-exception"></a>Verbinding geweigerd fabric-uitzondering
#### <a name="problem"></a>Probleem
Als u foutopsporing van Visual Studio, krijgt u een FabricConnectionDeniedException-fout.

#### <a name="solution"></a>Oplossing
Deze fout treedt meestal op wanneer u probeert een hostproces van de service handmatig te starten.

Zorg ervoor dat er geen serviceprojecten die zijn ingesteld als opstartprojecten in uw oplossing. Alleen de projecten van Service Fabric-toepassing moeten worden ingesteld als opstartprojecten.

> [!TIP]
> Als na de installatie, uw lokale cluster abnormaal gedrag begint, kunt u het opnieuw met behulp van de lokale cluster manager systeemtoepassing lade instellen. Hiermee verwijdert u het bestaande cluster en het instellen van een nieuwe. Houd er rekening mee dat alle toepassingen geïmplementeerde en bijbehorende gegevens worden verwijderd.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Begrijpen en oplossen van het cluster met systeemstatusrapporten](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Een cluster visualiseren met Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)

