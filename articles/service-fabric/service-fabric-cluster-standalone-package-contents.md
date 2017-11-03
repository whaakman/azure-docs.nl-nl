---
title: Azure Service Fabric zelfstandige-pakket voor WindowsServer | Microsoft Docs
description: Beschrijving en de inhoud van het pakket Azure Service Fabric Standalone voor Windows Server.
services: service-fabric
documentationcenter: .net
author: maburlik
manager: timlt
editor: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: chackdan;maburlik
ms.openlocfilehash: 6a6bacedde04419449a061554d5ce9ad37259a2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="contents-of-service-fabric-standalone-package-for-windows-server"></a>Inhoud van de zelfstandige versie van Service Fabric-pakket voor Windows Server
In de [gedownload](http://go.microsoft.com/fwlink/?LinkId=730690) zelfstandige voor Service Fabric-pakket, vindt u de volgende bestanden:

| **Bestandsnaam** | **Korte beschrijving** |
| --- | --- |
| CreateServiceFabricCluster.ps1 |Een PowerShell-script dat wordt gemaakt van het cluster met de instellingen in ClusterConfig.json. |
| RemoveServiceFabricCluster.ps1 |Een PowerShell-script dat wordt verwijderd van een cluster met de instellingen in ClusterConfig.json. |
| AddNode.ps1 |Een PowerShell-script voor het toevoegen van een knooppunt aan een bestaande implementatie van cluster op de huidige computer. |
| RemoveNode.ps1 |Een PowerShell-script voor het verwijderen van een knooppunt van een bestaand cluster met de huidige computer geïmplementeerd. |
| CleanFabric.ps1 |Een PowerShell-script voor het reinigen van een zelfstandige Service Fabric-installatie buiten de huidige computer. Vorige MSI-installaties moeten worden verwijderd met behulp van hun eigen uninstallers gekoppeld. |
| TestConfiguration.ps1 |Een PowerShell-script voor het analyseren van de infrastructuur zoals opgegeven in de Cluster.json. |
| DownloadServiceFabricRuntimePackage.ps1 |Een PowerShell-script gebruikt voor het downloaden van de meest recente pakket runtime buiten band zijn, voor scenario's waarin de implementatie machine niet is verbonden met internet. |
| DeploymentComponentsAutoextractor.exe |Zichzelf uitpakkend archief met onderdelen voor implementatie door de zelfstandige pakket scripts gebruikt. |
| EULA_ENU.txt |De licentievoorwaarden voor het gebruik van Microsoft Azure Service Fabric zelfstandig pakket met Windows Server. U kunt [download een exemplaar van de gebruiksrechtovereenkomst](http://go.microsoft.com/fwlink/?LinkID=733084) nu. |
| Leesmij |Een koppeling naar de release-opmerkingen en eenvoudige installatie-instructies. Dit is een subset van de instructies in dit document. |
| ThirdPartyNotice.rtf |De aankondiging van de software van derden die zich in het pakket. |
| Tools\Microsoft.Azure.ServiceFabric.WindowsServer.SupportPackage.zip |StandaloneLogCollector.exe die wordt uitgevoerd op aanvraag voor het verzamelen en traceerlogboeken uploaden naar Microsoft voor ondersteuning doel. |
| Tools\ServiceFabricUpdateService.zip |Een hulpprogramma waarmee automatisch code upgrade inschakelen voor clusters waarvoor geen toegang tot internet. Meer informatie vindt u [hier](service-fabric-cluster-upgrade-windows-server.md)|

**Sjablonen** 
| **Bestandsnaam** | **Korte beschrijving** |
| --- | --- |
| ClusterConfig.Unsecure.DevCluster.json |Een cluster voorbeeld configuratiebestand met de instellingen voor een onbeveiligde, drie-knooppunt, single-(of virtuele machine) ontwikkeling cluster, inclusief de gegevens voor elk knooppunt in het cluster. |
| ClusterConfig.Unsecure.MultiMachine.json |Een cluster voorbeeld configuratiebestand met de instellingen voor een niet-beveiligde, meerdere (of virtuele machine)-cluster, inclusief de gegevens voor elke computer in het cluster. |
| ClusterConfig.Windows.DevCluster.json |Een cluster voorbeeld configuratiebestand met de instellingen voor een veilige, drie-knooppunt, single-(of virtuele machine) ontwikkelcluster, inclusief de gegevens voor elk knooppunt in het cluster. Het cluster is beveiligd met behulp van [Windows identiteiten](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.Windows.MultiMachine.json |Een cluster voorbeeld configuratiebestand met de instellingen voor een veilige, cluster met meerdere (of virtuele machine) met behulp van Windows-beveiliging, met inbegrip van de gegevens voor elke machine die in het beveiligde cluster. Het cluster is beveiligd met behulp van [Windows identiteiten](https://msdn.microsoft.com/library/ff649396.aspx). |
| ClusterConfig.x509.DevCluster.json |Een cluster voorbeeld configuratiebestand met de instellingen voor een veilige, drie-knooppunt, single-(of virtuele machine) ontwikkelcluster, inclusief de gegevens voor elk knooppunt in het cluster. Het cluster is beveiligd met x509 certificaten. |
| ClusterConfig.x509.MultiMachine.json |Een cluster voorbeeld configuratiebestand met de instellingen voor het cluster veilige, meerdere (of virtuele machine), met inbegrip van de gegevens voor elk knooppunt in het beveiligde cluster. Het cluster is beveiligd met x509 certificaten. |
| ClusterConfig.gMSA.Windows.MultiMachine.json |Een cluster voorbeeld configuratiebestand met de instellingen voor het cluster veilige, meerdere (of virtuele machine), met inbegrip van de gegevens voor elk knooppunt in het beveiligde cluster. Het cluster is beveiligd met [Group Managed Service Accounts](https://technet.microsoft.com/en-us/library/jj128431(v=ws.11).aspx). |

## <a name="cluster-configuration-samples"></a>Voorbeelden van de cluster-configuratie
Meest recente versies van de configuratiesjablonen cluster kunnen worden gevonden op de GitHub-pagina: [zelfstandige Cluster configuratie voorbeelden](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

## <a name="independent-runtime-package"></a>Onafhankelijke Runtime-pakket
De meest recente runtime-pakket wordt automatisch gedownload tijdens de implementatie van het cluster uit [- Service Fabric-Runtime - koppeling Download Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).

## <a name="related"></a>Verwant
* [Een zelfstandige Azure Service Fabric-cluster maken](service-fabric-cluster-creation-for-windows-server.md)
* [Scenario's voor beveiliging van service Fabric-cluster](service-fabric-windows-cluster-windows-security.md)
