---
title: Oplossen van uw lokale Azure Service Fabric-cluster instellen | Microsoft Docs
description: In dit artikel bevat een reeks suggesties voor het oplossen van het lokale ontwikkelingscluster wijzigen
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: chackdan
editor: ''
ms.assetid: 97f4feaa-bba0-47af-8fdd-07f811fe2202
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: 8bb32b2bded061bd19bcd7cfda4ef259a75b0626
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662802"
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a>Oplossen van uw installatie van lokale ontwikkeling
Als u een probleem ondervindt tijdens interactie met uw lokale ontwikkelcluster van Azure Service Fabric, raadpleegt u de volgende suggesties voor mogelijke oplossingen.

## <a name="cluster-setup-failures"></a>Clusterinstallatie
### <a name="cannot-clean-up-service-fabric-logs"></a>Kan geen opschonen van Logboeken van de Service Fabric
#### <a name="problem"></a>Probleem
Tijdens het uitvoeren van het script DevClusterSetup, ziet u de volgende fout:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Oplossing
De huidige PowerShell-venster sluiten en open een nieuwe PowerShell-venster als beheerder. U kunt nu het script met succes uitvoeren.

## <a name="cluster-connection-failures"></a>Fouten bij het verbinden van cluster

### <a name="type-initialization-exception"></a>Type initialisatie uitzondering
#### <a name="problem"></a>Probleem
Wanneer u verbinding met het cluster in PowerShell maakt, ziet u de fout TypeInitializationException voor System.Fabric.Common.AppTrace.

#### <a name="solution"></a>Oplossing
De variabele path is niet correct ingesteld tijdens de installatie. Afmelden bij Windows en meld u opnieuw aan. Het pad wordt vernieuwd.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Clusterverbinding is mislukt met "-Object is gesloten"
#### <a name="problem"></a>Probleem
Een aanroep naar Connect-ServiceFabricCluster mislukt met een fout als volgt:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Oplossing
De huidige PowerShell-venster sluiten en open een nieuwe PowerShell-venster als beheerder.

### <a name="fabric-connection-denied-exception"></a>Uitzondering voor fabric verbinding geweigerd
#### <a name="problem"></a>Probleem
Als u foutopsporing via Visual Studio, krijgt u een FabricConnectionDeniedException-fout.

#### <a name="solution"></a>Oplossing
Deze fout treedt meestal op wanneer u probeert een hostproces van de service om handmatig te starten.

Zorg ervoor dat u geen serviceprojecten die zijn ingesteld als opstartprojecten in uw oplossing. Alleen de projecten van Service Fabric-toepassing moeten worden ingesteld als opstartprojecten.

> [!TIP]
> Als u, na de installatie, uw lokale cluster begint zich abnormaal gedraagt, kunt u herstellen met behulp van het systeemvak van de lokale cluster manager. Hiermee verwijdert u het bestaande cluster en een nieuw wachtwoord instellen. Houd er rekening mee dat alle toepassingen geïmplementeerde en bijbehorende gegevens worden verwijderd.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Begrijpen en oplossen van uw cluster met systeemstatusrapporten](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Een cluster visualiseren met Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)

