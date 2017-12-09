---
title: Een zelfstandige Azure Service Fabric-cluster maken | Microsoft Docs
description: Een Azure Service Fabric-cluster maken op elke computer (fysiek of virtueel) Windows Server, of het lokale of uitgevoerd in een cloud.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 31349169-de19-4be6-8742-ca20ac41eb9e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: chackdan;maburlik;dekapur
ms.openlocfilehash: 4b3b1e8a368419fe4014802f7b2a35b7ae3fba3c
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a>Een zelfstandige cluster met Windows Server maken
U kunt Azure Service Fabric gebruiken voor het maken van Service Fabric-clusters op alle virtuele machines of de computers waarop Windows Server wordt uitgevoerd. Dit betekent dat u kunt implementeren en Service Fabric-toepassingen uitvoeren in een omgeving die een set met elkaar verbonden computers met Windows Server bevat, worden deze on-premises of bij een cloudprovider. Service Fabric bevat een installatiepakket voor het maken van Service Fabric-clusters die het zelfstandige pakket met Windows Server genoemd.

Dit artikel begeleidt u bij de stappen voor het maken van een zelfstandige Service Fabric-cluster.

> [!NOTE]
> Dit zelfstandige Windows Server-pakket is commercieel beschikbaar en kan worden gebruikt voor productie-implementaties. Dit pakket bevat nieuwe Service Fabric-functies die 'in Preview' zijn. Schuif omlaag naar '[Preview-functies die zijn opgenomen in dit pakket](#previewfeatures_anchor). " de sectie voor een lijst van de preview-functies. U kunt [download een exemplaar van de gebruiksrechtovereenkomst](http://go.microsoft.com/fwlink/?LinkID=733084) nu.
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-the-service-fabric-for-windows-server-package"></a>Ondersteuning voor het Service Fabric voor Windows Server-pakket
* De community vragen over de zelfstandige Service Fabric-pakket voor Windows-Server in de [Azure Service Fabric-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).
* Open een serviceticket voor [Professional-ondersteuning voor Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).  Meer informatie over ondersteuning van Microsoft-professionals [hier](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
* U kunt ook ondersteuning voor dit pakket als onderdeel van [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).
* Zie voor meer informatie [opties voor ondersteuning van Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).
* Uitvoeren voor het verzamelen van Logboeken ter ondersteuning van de [Service Fabric zelfstandige logboekverzamelaar](service-fabric-cluster-standalone-package-contents.md).

<a id="downloadpackage"></a>

## <a name="download-the-service-fabric-for-windows-server-package"></a>De Service Fabric voor Windows Server downloaden
Voor het maken van het cluster gebruikt het Service Fabric voor Windows Server-pakket (Windows Server 2012 R2 en nieuwer) hier vinden: <br>
[Link - Service Fabric zelfstandige Package - Windows-Server downloaden](http://go.microsoft.com/fwlink/?LinkId=730690)

Meer informatie vinden van inhoud van het pakket [hier](service-fabric-cluster-standalone-package-contents.md).

Het Service Fabric-runtime-pakket is automatisch gedownload tijdens het maken van het cluster. Als het implementeren van een virtuele machine niet is verbonden met internet, download u het pakket runtime buiten-band van hier: <br>
[Link - Service Fabric-Runtime - Windows-Server downloaden](https://go.microsoft.com/fwlink/?linkid=839354)

Voorbeelden van de zelfstandige configuratie van het Cluster op zoeken: <br>
[Voorbeelden van zelfstandige clusterconfiguratie](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

<a id="createcluster"></a>

## <a name="create-the-cluster"></a>Het cluster maken
Service Fabric kan worden geïmplementeerd naar een cluster met één machine ontwikkeling met behulp van de *ClusterConfig.Unsecure.DevCluster.json* bestand in [voorbeelden](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

Uitpakken van het pakket zelfstandige naar uw computer, het voorbeeldbestand config kopiëren naar de lokale computer en voer vervolgens de *CreateServiceFabricCluster.ps1* script via een PowerShell-sessie van de beheerder van de pakketmap zelfstandige:
### <a name="step-1a-create-an-unsecured-local-development-cluster"></a>Stap 1A: een onbeveiligde lokaal ontwikkelcluster maken
```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

Zie de sectie omgeving in te stellen op [plannen en voorbereiden van uw implementatie van het cluster](service-fabric-cluster-standalone-deployment-preparation.md) voor de details voor probleemoplossing.

Als u actieve ontwikkelscenario's hebt voltooid, kunt u de Service Fabric-cluster verwijderen van de machine met een verwijzing naar de stappen in de sectie '[verwijderen van een cluster](#removecluster_anchor)'. 

### <a name="step-1b-create-a-multi-machine-cluster"></a>Stap 1B: een cluster met meerdere machines maken
Nadat u hebt doorlopen de planning en de voorbereidende stappen op gedetailleerde op de onderstaande koppeling, bent u klaar om uw productiecluster met behulp van het configuratiebestand van de cluster te maken. <br>
[Plannen en voorbereiden van uw implementatie van het cluster](service-fabric-cluster-standalone-deployment-preparation.md)

1. Valideren van het configuratiebestand dat u hebt geschreven door het uitvoeren van de *TestConfiguration.ps1* script van de pakketmap zelfstandige:  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    Ziet u uitvoer zoals hieronder. Het veld onder is 'Doorgegeven' geretourneerd als 'True', zijn geslaagd voor bevestigingen en het cluster gecontroleerd worden geïmplementeerd op basis van de configuratie van de invoer.

    ```
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

2. Maken van het cluster: Voer de *CreateServiceFabricCluster.ps1* script voor het implementeren van de Service Fabric-cluster op elke computer in de configuratie. 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> Implementatie traceringen worden geschreven naar de virtuele machine/machine waarop u het CreateServiceFabricCluster.ps1 PowerShell-script is uitgevoerd. U vindt deze in de submap DeploymentTraces, op basis van de map van waaruit het script wordt uitgevoerd. Als u wilt zien als Service Fabric correct is geïmplementeerd op een computer, de geïnstalleerde bestanden niet vinden in de map FabricDataRoot, zoals beschreven in het configuratiebestand van het cluster FabricSettings sectie (met standaard c:\ProgramData\SF). FabricHost.exe en Fabric.exe processen kunnen ook worden gezien uitgevoerd in Taakbeheer.
> 
> 

### <a name="step-1c-create-an-offline-internet-disconnected-cluster"></a>Stap 1C: een offline (op internet verbroken)-cluster maken
Het Service Fabric-runtime-pakket wordt automatisch bij het maken van het cluster gedownload. Wanneer u een cluster implementeert op computers die niet is verbonden met internet, moet u het Service Fabric-runtime-pakket afzonderlijk downloaden en geef het pad naar het bij het maken van het cluster.
Het pakket runtime kan afzonderlijk worden gedownload vanaf een andere computer is verbonden met internet, op [- Service Fabric-Runtime - koppeling Download Windows Server](https://go.microsoft.com/fwlink/?linkid=839354). De runtime-pakket kopiëren naar waar u implementeert het offline cluster uit en het cluster maken door het uitvoeren van `CreateServiceFabricCluster.ps1` met de `-FabricRuntimePackagePath` parameter opgenomen, zoals hieronder wordt weergegeven: 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```
waar `.\ClusterConfig.json` en `.\MicrosoftAzureServiceFabric.cab` respectievelijk de paden naar de clusterconfiguratie en het runtime CAB-bestand zijn.


### <a name="step-2-connect-to-the-cluster"></a>Stap 2: Verbinding maken met het cluster
Zie voor verbinding met een beveiligde cluster, [Service fabric verbinding maken met veilige cluster](service-fabric-connect-to-secure-cluster.md).

Voor verbinding met een niet-beveiligde cluster, moet u de volgende PowerShell-opdracht uitvoeren:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```
Voorbeeld:
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```
### <a name="step-3-bring-up-service-fabric-explorer"></a>Stap 3: Online zetten van de Service Fabric Explorer
Nu u verbinding met het cluster met Service Fabric Explorer vanuit een van de machines http://localhost:19080/Explorer/index.html of op afstand met http://&lt maken kunt;*IPAddressofaMachine*>: 19080/Explorer/index.html.

## <a name="add-and-remove-nodes"></a>Toevoegen en verwijderen van knooppunten
U kunt toevoegen of knooppunten aan uw zelfstandige Service Fabric-cluster verwijderen als uw bedrijf. Zie [toevoegen of verwijderen van knooppunten aan een zelfstandige Service Fabric-cluster](service-fabric-cluster-windows-server-add-remove-nodes.md) voor gedetailleerde stappen.

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a>Verwijderen van een cluster
Als u een cluster wilt verwijderen, voert u het PowerShell-script *RemoveServiceFabricCluster.ps1* uit vanuit de pakketmap en geeft u daarbij het pad op naar het JSON-configuratiebestand. Optioneel kunt u een locatie voor het verwijderingslogboek opgeven.

Dit script kan worden uitgevoerd op een machine met beheerdersrechten op alle machines die worden vermeld als knooppunten in het configuratiebestand van het cluster. Dit script wordt uitgevoerd op de machine geen deel uitmaken van het cluster.

```
# Removes Service Fabric from each machine in the configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```
# Removes Service Fabric from the current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Telemetrische gegevens verzameld en hoe deze afmelden
Standaard verzamelt het product telemetrie van de Service Fabric-gebruik voor het verbeteren van het product. De Best Practices Analyzer die wordt uitgevoerd als onderdeel van het installatieprogramma controleert voor connectiviteit met [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1). Als dit niet bereikbaar is, wordt de installatie mislukt, tenzij u telemetrie afmelden.

1. De pijplijn telemetrie probeert te uploaden van de volgende gegevens op [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) eenmaal voor elke dag. Het uploaden van een best effort is en heeft geen invloed op de clusterfunctionaliteit. De telemetrie alleen wordt verzonden via het knooppunt waarop de failover manager primaire. Er zijn geen andere knooppunten verzenden telemetrie.
2. De telemetrie bestaat uit het volgende:

* Aantal services
* Aantal ServiceTypes
* Aantal toepassingen
* Aantal ApplicationUpgrades
* Aantal FailoverUnits
* Aantal InBuildFailoverUnits
* Aantal UnhealthyFailoverUnits
* Aantal replica 's
* Aantal InBuildReplicas
* Aantal StandByReplicas
* Aantal OfflineReplicas
* CommonQueueLength
* QueryQueueLength
* FailoverUnitQueueLength
* CommitQueueLength
* Het aantal knooppunten
* IsContextComplete: True/False
* ClusterId: Dit is een willekeurig gegenereerd voor elk cluster GUID
* ServiceFabricVersion
* IP-adres van de virtuele machine of de computer van waaruit de telemetrie is geüpload

Als u wilt uitschakelen telemetrie, het volgende toevoegen aan *eigenschappen* in uw cluster-config: *enableTelemetry: false*.

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a>Preview-functies die zijn opgenomen in dit pakket
Geen.


> [!NOTE]
> Beginnen met de nieuwe [GA-versie van het cluster zelfstandige voor Windows Server (versie 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), kunt u uw cluster upgraden naar toekomstige releases handmatig of automatisch. Raadpleeg [upgraden van een zelfstandige Service Fabric-cluster versie](service-fabric-cluster-upgrade-windows-server.md) document voor meer informatie.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Implementeren en verwijderen van toepassingen met behulp van PowerShell](service-fabric-deploy-remove-applications.md)
* [Configuratie-instellingen voor zelfstandige Windows-cluster](service-fabric-cluster-manifest.md)
* [Toevoegen of verwijderen van knooppunten aan een zelfstandige Service Fabric-cluster](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Een zelfstandige Service Fabric-cluster versie upgraden](service-fabric-cluster-upgrade-windows-server.md)
* [Een zelfstandige Service Fabric-cluster maken met Azure Virtual machines waarop Windows wordt uitgevoerd](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [Beveiligen van een zelfstandige cluster op Windows met behulp van Windows-beveiliging](service-fabric-windows-cluster-windows-security.md)
* [Beveiligen van een zelfstandige cluster op Windows met behulp van X509 certificaten](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
