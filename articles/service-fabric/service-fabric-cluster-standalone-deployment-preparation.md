---
title: Azure Service Fabric zelfstandige Cluster implementatie voorbereiden | Microsoft Docs
description: Documentatie die betrekking hebben op de omgeving voorbereidt en de configuratie van het cluster worden overwogen voordat u een cluster dat is bedoeld voor het verwerken van een werkbelasting productie implementeert.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/12/2017
ms.author: dekapur;maburlik;chackdan
ms.openlocfilehash: b1190ec5a3ff70a368b29465699f9082d2b989bf
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
<a id="preparemachines"></a>

# <a name="plan-and-prepare-your-service-fabric-standalone-cluster-deployment"></a>Plannen en voorbereiden van uw implementatie van Service Fabric zelfstandige cluster
De volgende stappen uitvoeren voordat u uw cluster hebt gemaakt.

## <a name="plan-your-cluster-infrastructure"></a>Uw clusterinfrastructuur plannen
U gaat maken van een Service Fabric-cluster op computers die u 'eigenaar', zodat u welke soorten fouten u wilt dat het cluster beslissen kunt te blijven werken. Bijvoorbeeld, u verschillende nodig power lijnen of Internet-verbindingen die zijn doorgegeven aan deze machines? Overweeg daarnaast de fysieke beveiliging van deze computers. Waarbij de machines zich bevinden en die behoefte hebben toegang tot deze? Nadat u deze beslissingen kunt u de machines logisch toewijzen aan verschillende domeinen met fouten (Zie de volgende stap). Plannen voor productieclusters van de infrastructuur is meer betrokken dan voor testclusters.

## <a name="determine-the-number-of-fault-domains-and-upgrade-domains"></a>Het aantal foutdomeinen bepalen en domeinen upgraden
Een [ *foutdomein* (FD)](service-fabric-cluster-resource-manager-cluster-description.md) is een fysieke eenheid van de fout en is direct gerelateerd aan de fysieke infrastructuur in de datacenters. Een foutdomein bestaat uit de hardwareonderdelen (computers, switches, netwerken en meer) die delen van een potentieel risico. Hoewel er geen 1:1-toewijzing tussen domeinen met fouten en rekken, kan los spreken elk rack worden overwogen een domein met fouten.

Wanneer u FDs in ClusterConfig.json opgeeft, kunt u de naam op voor elke FD. Service Fabric ondersteunt hiërarchische FDs zodat u kunt de infrastructuurtopologie van uw erin spiegelen.  Bijvoorbeeld zijn de volgende FDs geldig:

* 'faultDomain': ' fd: / Rack1-Room1/Machine1 '
* 'faultDomain': ' fd: / FD1 '
* 'faultDomain': ' fd: / Room1/Rack1/PDU1/M1 '

Een *upgradedomein* (UD) is een logische eenheid van knooppunten. Tijdens upgrades van de Service Fabric gedirigeerd (een upgrade van de toepassing of het upgraden van een cluster), worden alle knooppunten in een UD offline gezet om uit te voeren van de upgrade terwijl knooppunten in andere UDs beschikbaar blijven voor aanvragen. De firmware-upgrades u op uw computers uitvoeren komen niet inwilligen UDs, dus moet u ze een doen machines tegelijk.

De eenvoudigste manier om na te denken over deze concepten is FDs rekening moet houden als de eenheid van niet-geplande storingen en UDs als eenheid voor gepland onderhoud.

Wanneer u UDs in ClusterConfig.json opgeeft, kunt u de naam op voor elke UD. De volgende namen zijn bijvoorbeeld geldig:

* 'upgradeDomain': 'UD0'
* 'upgradeDomain': 'UD1A'
* 'upgradeDomain': 'DomainRed'
* "upgradeDomain": "Blue"

Zie voor meer informatie over FDs en UDs, [met een beschrijving van een Service Fabric-cluster](service-fabric-cluster-resource-manager-cluster-description.md).

Een cluster in productie moet ten minste drie FDs span om te worden ondersteund in een productieomgeving, hebt u volledige controle over het onderhoud en beheer van de knooppunten, dus u bent zelf verantwoordelijk voor het bijwerken en machines vervangen. Voor clusters die zijn uitgevoerd in een omgeving (dat wil zeggen Amazon Web Services VM-instanties) waar u hebt geen volledige controle over de machines, moet u ten minste vijf FDs hebt in uw cluster. Elke FD kan een of meer knooppunten hebben. Zo wordt voorkomen dat problemen veroorzaakt door machine upgrades en updates die, afhankelijk van de timing, kunnen het uitvoeren van toepassingen en services in clusters verstoren.

## <a name="determine-the-initial-cluster-size"></a>De eerste clustergrootte bepalen

Over het algemeen is het aantal knooppunten in het cluster bepaald op basis van de behoeften van uw bedrijf, dat wil zeggen, hoeveel services en containers wordt uitgevoerd op het cluster en het aantal resources moet u uw werkbelastingen tolereren. Voor productieclusters, wordt aangeraden ten minste 5 knooppunten in het cluster met 5 FDs-spanning. Echter, als hierboven beschreven, als u volledige controle over uw knooppunten hebt en drie FDs kan omspannen, vervolgens drie knooppunten moeten ook doen de taak.

Testclusters met stateful werkbelastingen moeten drie knooppunten hebben dat testclusters alleen actieve alleen staatloze werkbelastingen één knooppunt moeten. Ook moet worden opgemerkt dat voor ontwikkelingsdoeleinden, kunnen er meer dan één knooppunt op een bepaalde computer. In een productieomgeving echter Service Fabric ondersteunt slechts één knooppunt per fysieke of virtuele machine.

## <a name="prepare-the-machines-that-will-serve-as-nodes"></a>Voorbereiden van de machines die als knooppunten fungeert

Hier volgen enkele aanbevolen specificaties voor elke computer die u wilt toevoegen aan het cluster:

* Minimaal 16 GB RAM-geheugen
* Een minimum van 40 GB beschikbare schijfruimte
* Een core 4 of hoger CPU
* De verbinding met een beveiligd netwerk of netwerken voor alle machines
* Windows Server 2012 R2 of WindowsServer 2016
* [.NET framework 4.5.1 of hoger](https://www.microsoft.com/download/details.aspx?id=40773), volledige installatie
* [Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell)
* De [RemoteRegistry service](https://technet.microsoft.com/library/cc754820) moet worden uitgevoerd op alle computers

De Clusterbeheerder implementeren en configureren van het cluster moet hebben [administrator-bevoegdheden](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) op elk van de machines. U kunt Service Fabric niet installeren op een domeincontroller.

## <a name="download-the-service-fabric-standalone-package-for-windows-server"></a>Downloaden van de zelfstandige Service Fabric-pakket voor Windows Server
[Windows-Server - Service Fabric zelfstandige Package - koppeling downloaden](http://go.microsoft.com/fwlink/?LinkId=730690) en pak het pakket, met een implementatie-machine die geen deel uitmaakt van het cluster of op een van de machines die een deel van uw cluster zal zijn.

## <a name="modify-cluster-configuration"></a>Configuratie van het cluster wijzigen
U moet een zelfstandige cluster ClusterConfig.json configuratiebestand maken, waarin de specificatie van het cluster wordt beschreven om een zelfstandige cluster te maken. U kunt het configuratiebestand van de sjablonen die zijn gevonden op basis van de onderstaande koppeling. <br>
[Zelfstandige clusterconfiguraties](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)

Zie voor meer informatie over de secties in dit bestand [configuratie-instellingen voor Windows-cluster zelfstandige](service-fabric-cluster-manifest.md).

Open een van de bestanden ClusterConfig.json van het pakket dat u hebt gedownload en de volgende instellingen wijzigen:
| **Configuratie-instelling** | **Beschrijving** |
| --- | --- |
| **NodeTypes** |Knooppunttypen kunnen u uw clusterknooppunten verdelen in verschillende groepen. Een cluster moet ten minste één NodeType hebben. Alle knooppunten in een groep hebben de volgende algemene kenmerken: <br> **Naam** -dit is de naam van het knooppunt. <br>**Eindpuntpoorten** - deze heten verschillende eindpunten (poorten) die gekoppeld aan dit knooppunttype zijn. U kunt een ander poortnummer die u wenst, zolang ze geen conflict met iets anders in dit manifest veroorzaken en zijn niet al in gebruik door een andere toepassing die wordt uitgevoerd op de virtuele machine/machine gebruiken. <br> **Plaatsingseigenschappen** -deze beschrijving van eigenschappen voor dit knooppunttype dat u als plaatsingsbeperkingen voor de systeemservices of uw services gebruikt. Deze eigenschappen zijn de gebruiker gedefinieerde sleutel/waarde-paren die extra meta-gegevens voor een bepaald knooppunt bieden. Voorbeelden van eigenschappen van het knooppunt is of het knooppunt een harde schijf of grafische kaart, het aantal aandrijfassen in de harde schijf, kernen en andere fysieke eigenschappen heeft. <br> **Capaciteitswaarden** -knooppunt capaciteit de naam en een bepaalde bron hoeveelheid definiëren dat een bepaald knooppunt beschikbaar voor verbruik is. Een knooppunt kan bijvoorbeeld dat er capaciteit voor een metriek 'MemoryInMb' genoemd en 2048 MB beschikbare schijfruimte heeft standaard definiëren. Deze capaciteit worden gebruikt tijdens runtime om ervoor te zorgen dat de services waarvoor bepaalde hoeveelheden resources worden geplaatst op de knooppunten die deze bronnen beschikbaar zijn in de vereiste bedragen hebben.<br>**IsPrimary** - als er meer dan één NodeType gedefinieerd dat slechts één is ingesteld op primaire met de waarde *true*, namelijk waarbij het systeem services uitvoeren. Alle andere knooppunttypen moeten worden ingesteld op de waarde *false* |
| **Knooppunten** |Dit zijn de gegevens voor elk van de knooppunten die deel van het cluster (knooppunttype, naam van het knooppunt IP-adres, foutdomein en upgradedomein van het knooppunt uitmaken). De machines u wilt dat het cluster op hoeft te worden hier vermeld met hun IP-adressen moeten worden gemaakt. <br> Als u hetzelfde IP-adres voor alle knooppunten gebruikt, klikt u vervolgens een 1-box-cluster is gemaakt, die u gebruiken kunt voor testdoeleinden. Gebruik geen 1-box-clusters voor het implementeren van productie-workloads. |

Nadat de configuratie van het cluster alle instellingen die zijn geconfigureerd voor de omgeving heeft, kan het worden getest op basis van de clusteromgeving (stap 7).

<a id="environmentsetup"></a>

## <a name="environment-setup"></a>Omgeving instellen

Wanneer een Clusterbeheerder zelfstandige Service Fabric-cluster configureert, moet de omgeving moet worden ingesteld met de volgende criteria: <br>
1. De gebruiker maakt het cluster moet beheerdersniveau machtigingen op alle machines die worden vermeld als knooppunten in het configuratiebestand van de cluster zijn.
2. Computer van waaruit het cluster is gemaakt, evenals elke machine cluster-knooppunt moet:
* Service Fabric SDK verwijderd hebben
* Service Fabric-runtime verwijderd hebben 
* Hebben de service Windows Firewall (mpsvc) ingeschakeld
* Hebben de Remote Registry-Service (remoteregistry) ingeschakeld
* Bestand die delen (SMB) ingeschakeld
* Hebt u nodig poorten die worden geopend, op basis van poorten voor cluster-configuratie
* Nodig poorten zijn geopend voor Windows SMB en Remote Registry-service: 135, 137, 138, 139 en 445
* Netwerk zijn verbonden met elkaar
3. Geen van de cluster-knooppunt machines moet een domeincontroller.
4. Als het cluster worden geïmplementeerd, een beveiligde cluster is, controleert u de noodzakelijke beveiliging vereiste onderdelen zijn geplaatst en correct zijn geconfigureerd op basis van de configuratie.
5. Als de clustermachines niet toegankelijk is via het internet zijn, moet u het volgende instellen in de configuratie van het cluster:
* Telemetrie uitschakelen: onder *eigenschappen* ingesteld *'enableTelemetry': false*
* Uitschakelen van automatische meldingen de huidige versie van de cluster nadert ondersteuning eindigt & downloaden van de Fabric-versie: onder *eigenschappen* ingesteld *'fabricClusterAutoupgradeEnabled': false*
* U kunt ook als internet toegang tot het netwerk beperkt tot domeinen wit vermeld is, de domeinen die hieronder zijn vereist voor automatische upgrade: go.microsoft.com download.microsoft.com

6. Toepasselijke antivirussoftware Service Fabric-uitsluitingen instellen:

| **Antivirussoftware uitgesloten mappen** |
| --- |
| Program Files\Microsoft Service Fabric |
| FabricDataRoot (van de configuratie van het cluster) |
| FabricLogRoot (van de configuratie van het cluster) |

| **Antivirussoftware uitgesloten processen** |
| --- |
| Fabric.exe |
| FabricHost.exe |
| FabricInstallerService.exe |
| FabricSetup.exe |
| FabricDeployer.exe |
| ImageBuilder.exe |
| FabricGateway.exe |
| FabricDCA.exe |
| FabricFAS.exe |
| FabricUOS.exe |
| FabricRM.exe |
| FileStoreService.exe |

## <a name="validate-environment-using-testconfiguration-script"></a>Omgeving met TestConfiguration script valideren
Het script TestConfiguration.ps1 vindt u in het zelfstandige pakket. Deze wordt gebruikt als een Best Practices Analyzer voor enkele van de bovenstaande criteria valideren en moet worden gebruikt als een controle wilt controleren of een cluster kan worden geïmplementeerd op een bepaalde omgeving. Als er een storing, verwijzen naar de lijst onder [omgeving in te stellen](service-fabric-cluster-standalone-deployment-preparation.md) voor het oplossen van problemen. 

Dit script kan worden uitgevoerd op een machine met beheerdersrechten op alle machines die worden vermeld als knooppunten in het configuratiebestand van het cluster. Dit script wordt uitgevoerd op de machine geen deel uitmaken van het cluster.

```powershell
PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
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

Deze configuratie testen module worden de beveiligingsconfiguratie momenteel niet gevalideerd, zodat dit heeft onafhankelijk worden uitgevoerd.  

> [!NOTE]
> We zijn voortdurend er verbeteringen aangebracht in deze module maken die krachtiger, dus als er een aanvraag defect of ontbrekend die u van mening bent is niet op dit moment zijn opgepikt door TestConfiguration, laat ons weten via onze [ondersteunen kanalen](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).   
> 
> 

## <a name="next-steps"></a>Volgende stappen
* [Een zelfstandige cluster met Windows Server maken](service-fabric-cluster-creation-for-windows-server.md)
