---
title: Een zelfstandig Azure Service Fabric-cluster instellen | Microsoft Docs
description: Maak een zelfstandig ontwikkelingscluster met drie knooppunten die op dezelfde computer worden uitgevoerd. Na deze instelling kunt u een cluster voor meerdere computers gaan maken.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/06/2017
ms.author: dekapur
ms.openlocfilehash: 5c8f4c784eed7b64810a3dd1c36c043d22a66936
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-service-fabric-standalone-cluster"></a>Uw eerste zelfstandige Service Fabric-cluster maken
U kunt een zelfstandig Service Fabric-cluster maken op elke virtuele machine of computer met Windows Server 2012 R2 of Windows Server 2016, lokaal of in de cloud. Deze Quick Start helpt u om in enkele minuten een zelfstandig ontwikkelingscluster te maken.  Wanneer u klaar bent, hebt u een cluster met drie knooppunten die worden uitgevoerd op één computer. Hierop kunt u vervolgens apps implementeren.

## <a name="before-you-begin"></a>Voordat u begint
Service Fabric biedt een installatiepakket voor het maken van zelfstandige Service Fabric-clusters.  [Download het installatiepakket](http://go.microsoft.com/fwlink/?LinkId=730690).  Pak het installatiepakket uit en zet dit in een map op de computer of virtuele machine waar u het ontwikkelingscluster instelt.  De inhoud van het installatiepakket wordt [hier](service-fabric-cluster-standalone-package-contents.md) uitgebreid beschreven.

De clusterbeheerder die het cluster implementeert en configureert, moet administratorbevoegdheden hebben op de computer. U kunt Service Fabric niet installeren op een domeincontroller.

## <a name="validate-the-environment"></a>De omgeving valideren
Het script *TestConfiguration.ps1* in het zelfstandige pakket wordt gebruikt als Best Practices Analyzer om te valideren of een cluster in een bepaalde omgeving kan worden geïmplementeerd. In de [Implementatievoorbereiding](service-fabric-cluster-standalone-deployment-preparation.md) vindt u de implementatie- en omgevingsvereisten. Voer het script uit om te controleren of u het ontwikkelingscluster kunt maken:

```powershell
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
```
## <a name="create-the-cluster"></a>Het cluster maken
Met het installatiepakket worden diverse voorbeelden van clusterconfiguraties geïnstalleerd. *ClusterConfig.Unsecure.DevCluster.json* is de eenvoudigste clusterconfiguratie: een onbeveiligd cluster met drie knooppunten die worden uitgevoerd op één computer.  Andere configuratiebestanden beschrijven clusters voor één of meerdere machines die zijn beveiligd met een x.509-certificaat of met Windows-beveiliging.  U hoeft de standaardconfiguratie-instellingen voor deze zelfstudie niet te wijzigen, maar neem wel het configuratiebestand door om vertrouwd te raken met de instellingen.  De sectie **nodes** beschrijft de drie knooppunten in het cluster: naam, IP-adres, [knooppunttype, foutdomein en upgradedomein](service-fabric-cluster-manifest.md#nodes-on-the-cluster).  De sectie **properties** definieert [de beveiliging, het betrouwbaarheidsniveau, de diagnostische verzameling en de typen knooppunten](service-fabric-cluster-manifest.md#cluster-properties) voor het cluster.

Dit cluster is onveilig.  Iedereen kan anoniem verbinding maken en beheerbewerkingen uitvoeren. Productieclusters moeten dus altijd worden beveiligd met X.509-certificaten of Windows-beveiliging.  Beveiliging kan alleen worden geconfigureerd tijdens het maken van het cluster. Het is niet mogelijk beveiliging in te schakelen nadat het cluster is gemaakt.  Lees [Een cluster beveiligen](service-fabric-cluster-security.md) voor meer informatie over de beveiliging van Service Fabric-clusters.  

Als u het ontwikkelingscluster met drie knooppunten wilt maken, voert u het script *CreateServiceFabricCluster.ps1* uit vanuit een PowerShell-sessie met administratorbevoegdheden:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

Het Service Fabric-runtimepakket wordt automatisch gedownload en geïnstalleerd op het moment dat het cluster wordt gemaakt.

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster
Het ontwikkelingscluster met drie knooppunten wordt nu uitgevoerd. De Service Fabric PowerShell-module wordt met de runtime geïnstalleerd.  Vanaf dezelfde computer of vanaf een externe computer met de Service Fabric-runtime kunt u controleren of het cluster wordt uitgevoerd.  De cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) brengt een verbinding met het cluster tot stand.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
Zie [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md) (Verbinding maken met een beveiligd cluster) voor meer voorbeelden van clusterverbindingen. Nadat u verbinding hebt gemaakt met het cluster, gebruikt u de cmdlet [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) om een lijst weer te geven van de knooppunten in het cluster, evenals statusinformatie voor elk knooppunt. **HealthState** moet *OK* zijn voor elk knooppunt.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- -------- --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     vm2      localhost       NodeType2 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm1      localhost       NodeType1 5.6.220.9494 0                     Up 00:03:38   00:00:00              OK
                     vm0      localhost       NodeType0 5.6.220.9494 0                     Up 00:02:43   00:00:00              OK
```

## <a name="visualize-the-cluster-using-service-fabric-explorer"></a>Het cluster visualiseren met Service Fabric Explorer
[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is een goed hulpmiddel om een cluster te visualiseren en toepassingen te beheren.  Service Fabric Explorer is een service die in het cluster wordt uitgevoerd. U opent deze door in een browser te navigeren naar [http://localhost:19080/Explorer](http://localhost:19080/Explorer). 

Het clusterdashboard bevat een overzicht van het cluster, inclusief een overzicht van de toepassings- en knooppuntstatus. In de knooppuntweergave ziet u de fysieke indeling van het cluster. Voor elk knooppunt kunt u controleren voor welke toepassingen er op het knooppunt code is geïmplementeerd.

![Service Fabric Explorer][service-fabric-explorer]

## <a name="remove-the-cluster"></a>Het cluster verwijderen
Als u een cluster wilt verwijderen, voert u het PowerShell-script *RemoveServiceFabricCluster.ps1* uit vanuit de pakketmap en geeft u daarbij het pad op naar het JSON-configuratiebestand. Optioneel kunt u een locatie voor het verwijderingslogboek opgeven.

```powershell
# Removes Service Fabric cluster nodes from each computer in the configuration file.
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -Force
```

Als u de Service Fabric-runtime van de computer wilt verwijderen, voert u het volgende PowerShell-script uit vanuit de pakketmap.

```powershell
# Removes Service Fabric from the current computer.
.\CleanFabric.ps1
```

## <a name="next-steps"></a>Volgende stappen
Nu u een zelfstandig ontwikkelingscluster hebt ingesteld, kunt u de volgende artikelen proberen:
* [Een zelfstandig cluster met meerdere machines instellen](service-fabric-cluster-creation-for-windows-server.md) en beveiliging inschakelen.
* [Apps implementeren met behulp van Powershell](service-fabric-deploy-remove-applications.md)

[service-fabric-explorer]: ./media/service-fabric-get-started-standalone-cluster/sfx.png
