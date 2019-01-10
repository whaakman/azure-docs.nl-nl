---
title: Een zelfstandig Azure Service Fabric-cluster maken | Microsoft Docs
description: Een Azure Service Fabric-cluster maken op elke computer (fysiek of virtueel) uitvoeren van Windows Server, ongeacht of deze zich on-premises of in elke cloud.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: 31349169-de19-4be6-8742-ca20ac41eb9e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: dekapur
ms.openlocfilehash: 321a69768935a9cb220bf5c2ae96c30274dc590d
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54159449"
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a>Een zelfstandige cluster uitgevoerd op Windows Server maken
Azure Service Fabric kunt u Service Fabric-clusters maken op elke virtuele machine of computers met Windows Server. Dit betekent dat u kunt implementeren en Service Fabric-toepassingen uitvoeren in een omgeving die een set met elkaar verbonden computers met Windows Server bevat, worden deze on-premises of met elke andere cloudprovider. Service Fabric biedt een installatiepakket voor het maken van het zelfstandige pakket voor Windows Server met de naam van Service Fabric-clusters.

In dit artikel leidt u door de stappen voor het maken van een zelfstandige Service Fabric-cluster.

> [!NOTE]
> Deze zelfstandige Windows Server-pakket is commercieel verkrijgbaar en kan worden gebruikt voor productie-implementaties. Dit pakket bevat nieuwe Service Fabric-functies die zich in 'Preview'. Schuif omlaag naar '[Preview-functies opgenomen in dit pakket](#previewfeatures_anchor). " sectie voor een lijst van de preview-functies. U kunt [download een exemplaar van de gebruiksrechtovereenkomst](https://go.microsoft.com/fwlink/?LinkID=733084) nu.
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-the-service-fabric-for-windows-server-package"></a>Profiteer van ondersteuning voor het Service Fabric voor Windows Server-pakket
* Vraag het de community over het zelfstandige pakket met Service Fabric voor Windows Server in de [forum van Azure Service Fabric](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).
* Open een ticket voor [professionele ondersteuning voor Service Fabric](https://support.microsoft.com/oas/default.aspx?prid=16146).  Meer informatie over professionele ondersteuning van Microsoft [hier](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
* U kunt ook ondersteuning voor dit pakket als onderdeel van [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).
* Zie voor meer informatie, [ondersteuningsopties voor Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).
* Uitvoeren voor het verzamelen van Logboeken voor ondersteuning van toepassing, de [logboekverzamelaar voor Service Fabric-zelfstandige](service-fabric-cluster-standalone-package-contents.md).

<a id="downloadpackage"></a>

## <a name="download-the-service-fabric-for-windows-server-package"></a>Het pakket Service Fabric voor Windows Server downloaden
Voor het maken van het cluster, gebruikt u de Service Fabric voor Windows Server-pakket (Windows Server 2012 R2 en hoger) is hier beschikbaar: <br>
[Link - pakket met Service Fabric zelfstandige - Windows-Server downloaden](https://go.microsoft.com/fwlink/?LinkId=730690)

Lees informatie over de inhoud van het pakket [hier](service-fabric-cluster-standalone-package-contents.md).

Het Service Fabric-runtimepakket wordt automatisch gedownload tijdens het maken van clusters. Als een implementatie van een virtuele machine niet is verbonden met internet, download u de runtime-pakket buiten-band hier: <br>
[Link - Service Fabric-Runtime - Windows-Server downloaden](https://go.microsoft.com/fwlink/?linkid=839354)

Voorbeelden op zelfstandige clusterconfiguratie zoeken: <br>
[Voorbeelden van configuraties voor zelfstandige Cluster](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

<a id="createcluster"></a>

## <a name="create-the-cluster"></a>Het cluster maken
Met het installatiepakket worden diverse voorbeelden van clusterconfiguraties geïnstalleerd. *ClusterConfig.Unsecure.DevCluster.json* is de eenvoudigste clusterconfiguratie: een onbeveiligd cluster met drie knooppunten die worden uitgevoerd op één computer.  Andere configuratiebestanden beschrijven clusters voor één of meerdere machines die zijn beveiligd met een x.509-certificaat of met Windows-beveiliging.  U hoeft de standaardconfiguratie-instellingen voor deze zelfstudie niet te wijzigen, maar neem wel het configuratiebestand door om vertrouwd te raken met de instellingen.  De sectie **nodes** beschrijft de drie knooppunten in het cluster: naam, IP-adres, [knooppunttype, foutdomein en upgradedomein](service-fabric-cluster-manifest.md#nodes-on-the-cluster).  De sectie **properties** definieert [de beveiliging, het betrouwbaarheidsniveau, de diagnostische verzameling en de typen knooppunten](service-fabric-cluster-manifest.md#cluster-properties) voor het cluster.

Het cluster hebt gemaakt in dit artikel is onveilig.  Iedereen kan anoniem verbinding maken en beheerbewerkingen uitvoeren. Productieclusters moeten dus altijd worden beveiligd met X.509-certificaten of Windows-beveiliging.  Beveiliging kan alleen worden geconfigureerd tijdens het maken van het cluster. Het is niet mogelijk beveiliging in te schakelen nadat het cluster is gemaakt. Update het configuratiebestand inschakelen [beveiliging van het certificaat](service-fabric-windows-cluster-x509-security.md) of [Windows security](service-fabric-windows-cluster-windows-security.md). Lees [Een cluster beveiligen](service-fabric-cluster-security.md) voor meer informatie over de beveiliging van Service Fabric-clusters.

### <a name="step-1a-create-an-unsecured-local-development-cluster"></a>Stap 1A: Maak een niet-beveiligde lokaal ontwikkelcluster
Service Fabric kunnen worden geïmplementeerd op een ontwikkelingscluster met een machine met behulp van de *ClusterConfig.Unsecure.DevCluster.json* -bestand in [voorbeelden](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

Pak het zelfstandige pakket naar uw computer, de config-voorbeeldbestand kopiëren naar de lokale computer en voer vervolgens de *CreateServiceFabricCluster.ps1* script via een PowerShell-sessie, uit het zelfstandige pakketmap .

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

Zie de sectie omgeving instellen op [plannen en voorbereiden van de implementatie van uw cluster](service-fabric-cluster-standalone-deployment-preparation.md) voor probleemoplossing.

Als u actieve ontwikkelscenario's hebt voltooid, kunt u de Service Fabric-cluster van de computer verwijderen door te verwijzen naar de stappen in de sectie '[verwijderen van een cluster](#removecluster_anchor)'. 

### <a name="step-1b-create-a-multi-machine-cluster"></a>Stap 1B: Maken van een cluster met meerdere machines
Nadat u hebt doorlopen de planning en voorbereidingsstappen beschreven aan [plannen en voorbereiden van de implementatie van uw cluster](service-fabric-cluster-standalone-deployment-preparation.md), bent u klaar om uw productiecluster met behulp van het configuratiebestand van uw cluster te maken.

De clusterbeheerder die het cluster implementeert en configureert, moet administratorbevoegdheden hebben op de computer. U kunt Service Fabric niet installeren op een domeincontroller.

1. Het script *TestConfiguration.ps1* in het zelfstandige pakket wordt gebruikt als Best Practices Analyzer om te valideren of een cluster in een bepaalde omgeving kan worden geïmplementeerd. In de [Implementatievoorbereiding](service-fabric-cluster-standalone-deployment-preparation.md) vindt u de implementatie- en omgevingsvereisten. Voer het script uit om te controleren of u het ontwikkelingscluster kunt maken:  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    De uitvoer ziet er als volgt uit. Het veld onder is 'Goedgekeurd' wordt geretourneerd als 'True', bevestigingen zijn geslaagd en het cluster ziet er uit om te worden geïmplementeerd op basis van de configuratie van de invoer.

    ```powershell
    Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
    Running Best Practices Analyzer...
    Best Practices Analyzer completed successfully.
    
    LocalAdminPrivilege        : True
    IsJsonValid                : True
    IsCabValid                 : True
    RequiredPortsOpen          : True
    RemoteRegistryAvailable    : True
    FirewallAvailable          : True
    RpcCheckPassed             : True
    NoConflictingInstallations : True
    FabricInstallable          : True
    Passed                     : True
    ```

2. Maken van het cluster:  Voer de *CreateServiceFabricCluster.ps1* script voor het implementeren van de Service Fabric-cluster in elke computer in de configuratie. 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> Er worden implementatietraceringen weggeschreven naar de VM/computer waarop u het PowerShell-script CreateServiceFabricCluster.ps1 hebt uitgevoerd. U vindt deze traceringen in de submap DeploymentTraces, in de map van waaruit het script is uitgevoerd. U weet dat Service Fabric goed is geïmplementeerd op een computer als de geïnstalleerde bestanden in de map FabricDataRoot staan, zoals beschreven in de sectie FabricSettings van het configuratiebestand voor het cluster (standaard c:\ProgramData\SF). Daarnaast moeten de processen FabricHost.exe en Fabric.exe actief zijn in Taakbeheer.
> 
> 

### <a name="step-1c-create-an-offline-internet-disconnected-cluster"></a>Stap 1C: Een offline (internet verbroken)-cluster maken
Het Service Fabric-runtimepakket wordt automatisch gedownload bij het maken van clusters. Wanneer u een cluster implementeert op computers die niet zijn verbonden met internet, moet u het Service Fabric-runtimepakket afzonderlijk te downloaden en geef het pad naar het bij het maken van clusters.
De runtime-pakket kan afzonderlijk worden gedownload vanaf een andere computer is verbonden met internet, op [koppeling downloaden - Service Fabric-Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354). Kopieer de runtime-pakket naar waar u de offline cluster op basis van implementeert en maken van het cluster door uit te voeren `CreateServiceFabricCluster.ps1` met de `-FabricRuntimePackagePath` parameter opgenomen, zoals wordt weergegeven in dit voorbeeld: 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```

*.\ClusterConfig.JSON* en *.\MicrosoftAzureServiceFabric.cab* zijn respectievelijk de paden naar de configuratie van het cluster en het runtime CAB-bestand.

### <a name="step-2-connect-to-the-cluster"></a>Stap 2: Verbinding maken met het cluster
Verbinding maken met het cluster om te controleren of dat het cluster wordt uitgevoerd en beschikbaar. De Service Fabric PowerShell-module wordt met de runtime geïnstalleerd.  U kunt verbinding maken met het cluster uit een van de clusterknooppunten of vanaf een externe computer met de Service Fabric-runtime.  De cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) brengt een verbinding met het cluster tot stand.

Voor verbinding met een onbeveiligd cluster, moet u de volgende PowerShell-opdracht uitvoeren:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```

Bijvoorbeeld:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
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

### <a name="step-3-visualize-the-cluster-using-service-fabric-explorer"></a>Stap 3: Het cluster visualiseren met Service Fabric Explorer
[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is een goed hulpmiddel om een cluster te visualiseren en toepassingen te beheren.  Service Fabric Explorer is een service die wordt uitgevoerd in het cluster, die u via een browser openen door te navigeren naar [ http://localhost:19080/Explorer ](http://localhost:19080/Explorer).

Het clusterdashboard bevat een overzicht van het cluster, inclusief een overzicht van de toepassings- en knooppuntstatus. In de knooppuntweergave ziet u de fysieke indeling van het cluster. Voor elk knooppunt kunt u controleren voor welke toepassingen er op het knooppunt code is geïmplementeerd.

![Service Fabric Explorer][service-fabric-explorer]

## <a name="add-and-remove-nodes"></a>Knooppunten toevoegen en verwijderen
U kunt knooppunten toevoegen aan of verwijderen uit uw zelfstandige Service Fabric-cluster wanneer de bedrijfsbehoeften veranderen. Zie [Knooppunten toevoegen aan of verwijderen uit een zelfstandig Service Fabric-cluster](service-fabric-cluster-windows-server-add-remove-nodes.md) voor gedetailleerde stappen.

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a>Een cluster verwijderen
Als u een cluster wilt verwijderen, voert u het PowerShell-script *RemoveServiceFabricCluster.ps1* uit vanuit de pakketmap en geeft u daarbij het pad op naar het JSON-configuratiebestand. Optioneel kunt u een locatie voor het verwijderingslogboek opgeven.

Met dit script kan worden uitgevoerd op elke computer die de beheerderstoegang heeft tot alle machines die worden weergegeven als knooppunten in het configuratiebestand van het cluster. De machine die met dit script wordt uitgevoerd op geen deel uitmaken van het cluster.

```powershell
# Removes Service Fabric from each machine in the configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```powershell
# Removes Service Fabric from the current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Telemetrische gegevens die worden verzameld en hoe u deze uitschakelen
Als een standaard verzamelt het product telemetrie op het gebruik van de Service Fabric voor het verbeteren van het product. De analyse van aanbevolen procedures die wordt uitgevoerd als een onderdeel van de controles voor connectiviteit met [ https://vortex.data.microsoft.com/collect/v1 ](https://vortex.data.microsoft.com/collect/v1). Als deze niet bereikbaar is, mislukt de installatie, tenzij u afmelden voor telemetrie.

1. De pijplijn telemetrie probeert te uploaden van de volgende gegevens op [ https://vortex.data.microsoft.com/collect/v1 ](https://vortex.data.microsoft.com/collect/v1) één keer voor elke dag. Het uploaden van een best-effort is en heeft geen invloed op de clusterfunctionaliteit. De telemetrie wordt alleen verzonden vanaf het knooppunt waarop de failover manager primaire. Er zijn geen andere knooppunten verzenden telemetrie.
2. De telemetrie bestaat uit het volgende:

* Aantal services
* Aantal ServiceTypes
* Aantal toepassingen
* Aantal ApplicationUpgrades
* Aantal failover-eenheden
* Aantal InBuildFailoverUnits
* Aantal UnhealthyFailoverUnits
* Aantal replica's
* Aantal InBuildReplicas
* Aantal StandByReplicas
* Aantal OfflineReplicas
* CommonQueueLength
* QueryQueueLength
* FailoverUnitQueueLength
* CommitQueueLength
* Aantal knooppunten
* IsContextComplete: Waar/onwaar
* ClusterId: Dit is een willekeurig gegenereerd voor elk cluster GUID
* ServiceFabricVersion
* IP-adres van de virtuele machine of computer van waaruit de telemetrie is geüpload

Als u wilt uitschakelen telemetrie, het volgende toevoegen aan *eigenschappen* in uw cluster-configuratie: *enableTelemetry: false*.

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a>Preview-functies opgenomen in dit pakket
Geen.


> [!NOTE]
> Beginnen met de nieuwe [GA-versie van de zelfstandige-cluster voor Windows Server (versie 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), kunt u uw cluster upgraden naar toekomstige releases handmatig of automatisch. Raadpleeg [upgraden van een zelfstandige Service Fabric-clusterversie](service-fabric-cluster-upgrade-windows-server.md) document voor meer informatie.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Implementeren en verwijderen van toepassingen met behulp van PowerShell](service-fabric-deploy-remove-applications.md)
* [Configuratie-instellingen voor zelfstandige Windows-cluster](service-fabric-cluster-manifest.md)
* [Toevoegen of verwijderen van knooppunten in een zelfstandige Service Fabric-cluster](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Een zelfstandige Service Fabric-clusterversie bijwerken](service-fabric-cluster-upgrade-windows-server.md)
* [Een zelfstandige Service Fabric-cluster maken met virtuele Azure-machines waarop Windows wordt uitgevoerd](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [Een zelfstandige cluster beveiligen op Windows met behulp van Windows-beveiliging](service-fabric-windows-cluster-windows-security.md)
* [Een zelfstandige cluster beveiligen op Windows met behulp van X509 certificaten](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
[service-fabric-explorer]: ./media/service-fabric-cluster-creation-for-windows-server/sfx.png