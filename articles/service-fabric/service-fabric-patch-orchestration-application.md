---
title: Azure Service Fabric-patch orchestration-toepassing | Microsoft Docs
description: De toepassing voor het automatiseren van besturingssysteem toepassen van patches op een Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: khandelwalbrijeshiitr
manager: timlt
editor: ''
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/01/2019
ms.author: brkhande
ms.openlocfilehash: d5d7f45b4833bb535e98542ee513e9ea8bf0f9e5
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57432988"
---
# <a name="patch-the-windows-operating-system-in-your-service-fabric-cluster"></a>Patch uitvoeren voor het Windows-besturingssysteem in uw Service Fabric-cluster

> [!div class="op_single_selector"]
> * [Windows](service-fabric-patch-orchestration-application.md)
> * [Linux](service-fabric-patch-orchestration-application-linux.md)
>
>

[Afbeelding van automatische besturingssysteemupgrades schaalset van virtuele machine van Azure](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) is de aanbevolen procedure voor uw besturingssystemen gevuld in Azure bewaren en de Patch Orchestration-toepassing (POA) is een wrapper rond Service Fabrics RepairManager systemen service waarmee configuratie op basis van OS patch plannen voor niet-Azure gehoste clusters. POA is niet vereist voor niet-Azure gehoste clusters, maar het patchinstallatie door de domeinen bijwerken plannen is vereist voor het vullen van Service Fabric-clusters hosts zonder uitvaltijd.

POA is een Azure Service Fabric-toepassing waarmee het besturingssysteem op een Service Fabric-cluster zonder uitvaltijd patches worden geautomatiseerd.

De patch orchestration-app biedt de volgende functies:

- **Automatische update besturingssysteeminstallatie**. Besturingssysteem-updates automatisch gedownload en geïnstalleerd. Clusterknooppunten worden opnieuw opgestart als dat nodig is zonder uitvaltijd van de cluster.

- **Cluster-aware toepassen van patches en de gezondheid van integratie**. Tijdens het toepassen van updates, controleert de patch orchestration-app de status van de clusterknooppunten. Knooppunten van het cluster zijn één knooppunt op een tijd of één upgradedomein tegelijk. Als de status van het cluster uitgeschakeld vanwege het patchproces wordt, is patching gestopt om te voorkomen dat het probleem toegenomen.

## <a name="internal-details-of-the-app"></a>Interne details van de app.

De patch orchestration-app bestaat uit de volgende onderdelen:

- **Service Coordinator**: Deze stateful service is verantwoordelijk voor:
    - De Windows Update-taak op het hele cluster coördineren.
    - Het opslaan van het resultaat van voltooide Windows Update-bewerkingen.
- **Knooppunt-agentservice**: Deze stateless service wordt uitgevoerd op alle knooppunten van de Service Fabric-cluster. De service is verantwoordelijk voor:
    - De Agent knooppunt NTService opstarten.
    - Bewaking van de Agent knooppunt NTService.
- **Knooppunt Agent NTService**: Deze Windows NT-service wordt uitgevoerd op een hoger niveau van bevoegdheden (systeem). Daarentegen de knooppunt Agent-Service en de Coordinator-Service uitgevoerd op een lager niveau van bevoegdheden (Netwerkservice). De service is verantwoordelijk voor het uitvoeren van de volgende taken in de Windows Update op alle clusterknooppunten:
    - Het uitschakelen van automatische Update van Windows op het knooppunt.
    - Downloaden en installeren van Windows Update op basis van het beleid wordt de gebruiker is opgegeven.
    - Opnieuw starten van de computer na installatie van Windows Update.
    - De resultaten van Windows-updates worden geüpload naar de Coordinator-Service.
    - Rapportage status rapporteert als een bewerking is mislukt na het toewijzen van alle nieuwe pogingen.

> [!NOTE]
> De patch orchestration-app maakt gebruik van de Service Fabric herstellen manager systeemservice uitschakelen of inschakelen van het knooppunt en statuscontroles uitvoeren. De hersteltaak die zijn gemaakt door de patch orchestration app houdt de voortgang van de Windows Update voor elk knooppunt.

## <a name="prerequisites"></a>Vereisten

> [!NOTE]
> Minimum .NET framework-versie vereist is 4.6.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>De reparatie manager-service inschakelen (indien deze niet al actief)

De patch orchestration-app moet de service manager herstellen wordt ingeschakeld op het cluster.

#### <a name="azure-clusters"></a>Azure-clusters

Azure-clusters in de silver duurzaamheidslaag hebben de herstel-manager-service standaard ingeschakeld. Azure-clusters in de duurzaamheidslaag goud mogelijk of beschikt niet over de reparatie manager-service is ingeschakeld, afhankelijk van wanneer deze clusters zijn gemaakt. Azure-clusters in de duurzaamheidslaag brons, standaard, beschikt niet over de reparatie manager-service is ingeschakeld. Als de service al is ingeschakeld, kunt u zien dat in de sectie van de services system in de Service Fabric Explorer.

##### <a name="azure-portal"></a>Azure Portal
Reparatiemanager vanuit Azure portal kunt u op het moment van het instellen van het cluster. Selecteer **Reparatiemanager opnemen** onder de optie **functies van invoegtoepassingen** op het moment van de configuratie van het cluster.
![Afbeelding van inschakelen van herstel-Manager van Azure portal](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="azure-resource-manager-deployment-model"></a>Azure Resource Manager-implementatiemodel
U kunt ook kunt u de [Azure Resource Manager-implementatiemodel](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) om in te schakelen van de manager-service voor herstel op nieuwe en bestaande Service Fabric-clusters. De sjabloon ophalen voor het cluster dat u wilt implementeren. U kunt de voorbeeldsjablonen gebruiken of een aangepaste sjabloon van Azure Resource Manager deployment model maken. 

Om in te schakelen de reparatie manager service met behulp van [Azure Resource Manager deployment model sjabloon](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):

1. Controleer eerst of de `apiversion` is ingesteld op `2017-07-01-preview` voor de `Microsoft.ServiceFabric/clusters` resource. Als deze verschilt, dan moet u bijwerken de `apiVersion` op de waarde `2017-07-01-preview` of hoger:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Nu de reparatie manager-service inschakelen door toe te voegen van de volgende `addonFeatures` sectie na de `fabricSettings` sectie:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Nadat u de clustersjabloon voor het hebt bijgewerkt met deze wijzigingen, past deze toe en kunt u de upgrade voltooien. U ziet nu de reparatie manager systeemservice uitgevoerd in het cluster. Dit heet `fabric:/System/RepairManagerService` in de sectie van de services system in de Service Fabric Explorer. 

### <a name="standalone-on-premises-clusters"></a>Clusters van on-premises zelfstandige

U kunt de [configuratie-instellingen voor zelfstandige Windows cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) om in te schakelen van de manager-service voor herstel op nieuwe en bestaande Service Fabric-cluster.

De reparatie manager-service inschakelen:

1. Controleer eerst of de `apiversion` in [algemene clusterconfiguraties](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) is ingesteld op `04-2017` of hoger:

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. Nu herstellen manager-service inschakelen door toe te voegen van de volgende `addonFeatures` sectie na de `fabricSettings` sectie zoals hieronder wordt weergegeven:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Het clustermanifest van uw bijwerken met deze wijzigingen, met behulp van de bijgewerkte clustermanifest [Maak een nieuw cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) of [upgrade van de clusterconfiguratie](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server). Zodra het cluster wordt uitgevoerd met bijgewerkte clustermanifest, kunt u nu de reparatie manager systeemservice uitgevoerd in het cluster, met de naam zien `fabric:/System/RepairManagerService`onder system services-punt in de Service Fabric explorer.

### <a name="configure-windows-updates-for-all-nodes"></a>Windows-Updates configureren voor alle knooppunten

Automatische Updates voor Windows kan leiden tot verlies van beschikbaarheid, omdat meerdere clusterknooppunten opnieuw op hetzelfde moment starten kunnen. De patch orchestration-app probeert standaard om uit te schakelen van de automatische Update van Windows op elk clusterknooppunt. Als de instellingen worden beheerd door een beheerder of Groepsbeleid, is het echter raadzaam te expliciet instellen van de Windows Update-beleid om u te 'Op de hoogte stellen voordat downloaden'.

## <a name="download-the-app-package"></a>Het app-pakket downloaden

Toepassing echter samen met scripts voor installatie kan worden gedownload vanaf [archief koppeling](https://go.microsoft.com/fwlink/?linkid=869566).

Toepassing in sfpkg-indeling kan worden gedownload vanaf [sfpkg koppeling](https://aka.ms/POA/POA.sfpkg). Dit is handig voor [implementatie van toepassing op basis van Azure Resource Manager](service-fabric-application-arm-resource.md).

## <a name="configure-the-app"></a>De app configureren

Het gedrag van de patch orchestration-app kan worden geconfigureerd om te voldoen aan uw behoeften. De standaardwaarden overschrijven door te geven in de parameter van de toepassing tijdens het maken van de toepassing of update. Parameters voor de toepassing kunnen worden opgegeven door op te geven `ApplicationParameter` naar de `Start-ServiceFabricApplicationUpgrade` of `New-ServiceFabricApplication` cmdlets.

|**Parameter**        |**Type**                          | **Details**|
|:-|-|-|
|MaxResultsToCache    |Lang                              | Maximum aantal resultaten van de Windows Update, die moet worden opgeslagen in de cache. <br>Standaardwaarde is 3000 ervan uitgaande dat de: <br> -Het aantal knooppunten is 20. <br> -Het aantal updates dat op een knooppunt per maand gebeurt is vijf. <br> -Het aantal resultaten dat per bewerking is 10. <br> -Resultaten voor de afgelopen drie maanden moeten worden opgeslagen. |
|TaskApprovalPolicy   |Enum <br> { NodeWise, UpgradeDomainWise }                          |TaskApprovalPolicy geeft aan dat het beleid dat moet worden gebruikt door de Coordinator-Service op Windows-updates installeren via de Service Fabric-clusterknooppunten.<br>                         Toegestane waarden zijn: <br>                                                           <b>NodeWise</b>. Windows Update is geïnstalleerd één knooppunt tegelijk. <br>                                                           <b>UpgradeDomainWise</b>. Windows Update is geïnstalleerd één upgradedomein tegelijk. (Het maximum is bereikt, alle knooppunten die behoren tot een upgradedomein kunnen gaan voor Windows Update.)<br> Raadpleeg [Veelgestelde vragen over](#frequently-asked-questions) sectie over het bepalen welke het beste geschikt beleid voor uw cluster.
|LogsDiskQuotaInMB   |Lang  <br> (Standaard: 1024)               |Maximale grootte van de patch orchestration app registreert in MB, hetgeen kan lokaal worden opgeslagen op de knooppunten.
| WUQuery               | string<br>(Standaard: "IsInstalled=0")                | Query voor het ophalen van Windows-updates. Zie voor meer informatie, [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)
| InstallWindowsOSOnlyUpdates | Booleaans <br> (standaard: false)                 | Gebruik deze eigenschap om te bepalen welke updates moeten worden gedownload en geïnstalleerd. Volgende waarden zijn toegestaan <br>True - installeert alleen de updates van Windows-besturingssysteem.<br>ONWAAR: installeert alle beschikbare updates op de machine.          |
| WUOperationTimeOutInMinutes | Int <br>(Standaard: 90)                   | Hiermee geeft u de time-out voor een Windows Update-bewerking (zoeken of downloaden of installeren). Als de bewerking is niet voltooid binnen de opgegeven time-out, wordt het afgebroken.       |
| WURescheduleCount     | Int <br> (Standaard: 5)                  | Het maximum aantal keren dat de service opnieuw gepland voor de Windows update als een bewerking blijft mislukken.          |
| WURescheduleTimeInMinutes | Int <br>(Standaard: 30) | Het interval waarmee de service wordt automatisch opnieuw gepland met de Windows update als fout zich blijft voordoen. |
| WUFrequency           | Door komma's gescheiden tekenreeks op (standaard: "Weekly, Wednesday, 7:00:00")     | De frequentie voor het installeren van Windows Update. De indeling en de mogelijke waarden zijn: <br>-Maand, DD uu: mm:, bijvoorbeeld, maandelijks, 5, 12: 22:32.<br>Toegestane waarden voor veld DD (dag) zijn getallen tussen het bereik 1-28 en 'last'. <br> -Wekelijks, dag, uu: mm:, voor bijvoorbeeld wekelijks, dinsdag, 12:22:32.  <br> -Dagelijks, uu: mm:, bijvoorbeeld dagelijks, 12:22:32.  <br> -Geen geeft aan dat Windows Update al dan niet mogen worden uitgevoerd.  <br><br> Houd er rekening mee dat de tijden in UTC zijn.|
| AcceptWindowsUpdateEula | Booleaans <br>(Standaard: true) | Met deze markering instellen, accepteert de toepassing de eindgebruiker-licentie voor Windows Update namens de eigenaar van de machine.              |

> [!TIP]
> Als u wilt dat Windows Update onmiddellijk plaats, stelt u `WUFrequency` ten opzichte van de tijd van de implementatie van toepassing. Stel bijvoorbeeld dat u een testcluster met vijf knooppunten hebt en wilt implementeren, de app om ongeveer 5:00 uur UTC. Als u wordt ervan uitgegaan dat de upgrade van de toepassing of de implementatie van 30 minuten maximaal duurt, stelt u de WUFrequency als "Dagelijks, 17:30:00"

## <a name="deploy-the-app"></a>De app implementeren

1. Voltooi de vereiste stappen voor het voorbereiden van het cluster.
2. De patch orchestration-app, zoals elke andere Service Fabric-app implementeren. U kunt de app implementeren met behulp van PowerShell. Volg de stappen in [implementeren en remove-toepassingen met behulp van PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).
3. Voor het configureren van de toepassing op het moment van implementatie, geven de `ApplicationParameter` naar de `New-ServiceFabricApplication` cmdlet. Voor uw gemak hebben we het script Deploy.ps1 samen met de toepassing opgegeven. Het script gebruiken:

    - Verbinding maken met een Service Fabric-cluster met behulp van `Connect-ServiceFabricCluster`.
    - Voer de PowerShell-script Deploy.ps1 met de juiste `ApplicationParameter` waarde.

> [!NOTE]
> Houd het script en de toepassingsmap PatchOrchestrationApplication in dezelfde map.

## <a name="upgrade-the-app"></a>Upgrade van de app.

Als u een bestaande patch orchestration-app bijwerken met behulp van PowerShell, volg de stappen in [upgrade van de Service Fabric-toepassing met behulp van PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).

## <a name="remove-the-app"></a>De app verwijderen

Volg de stappen in de toepassing te verwijderen, [implementeren en remove-toepassingen met behulp van PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).

Voor uw gemak hebben we het script Undeploy.ps1 samen met de toepassing opgegeven. Het script gebruiken:

  - Verbinding maken met een Service Fabric-cluster met behulp van ```Connect-ServiceFabricCluster```.

  - Voer het PowerShell-script Undeploy.ps1.

> [!NOTE]
> Houd het script en de toepassingsmap PatchOrchestrationApplication in dezelfde map.

## <a name="view-the-windows-update-results"></a>De Windows Update-resultaten weergeven

De patch orchestration-app beschikbaar REST-API's om de historische resultaten voor de gebruiker weer te geven. Een voorbeeld van het JSON-resultaat:
```json
[
  {
    "NodeName": "_stg1vm_1",
    "WindowsUpdateOperationResults": [
      {
        "OperationResult": 0,
        "NodeName": "_stg1vm_1",
        "OperationTime": "2017-05-21T11:46:52.1953713Z",
        "UpdateDetails": [
          {
            "UpdateId": "7392acaf-6a85-427c-8a8d-058c25beb0d6",
            "Title": "Cumulative Security Update for Internet Explorer 11 for Windows Server 2012 R2 (KB3185319)",
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of the issues that are included in this update, see the associated Microsoft Knowledge Base article. After you install this update, you may have to restart your system.",
            "ResultCode": 0
          }
        ],
        "OperationType": 1,
        "WindowsUpdateQuery": "IsInstalled=0",
        "WindowsUpdateFrequency": "Daily,10:00:00",
        "RebootRequired": false
      }
    ]
  },
  ...
]
```

Velden van de JSON worden hieronder beschreven.

Veld | Waarden | Details
-- | -- | --
OperationResult | 0 - Succeeded<br> 1 - is voltooid met fouten<br> 2 - is mislukt<br> 3 - afgebroken<br> 4 - afgebroken met time-out | Geeft aan dat het resultaat van de algehele bewerking (meestal met betrekking tot installatie van updates voor een of meer).
ResultCode | Kan operationresult niet gelijk | Dit veld wordt resultaat van de installatiebewerking voor individuele update aangegeven.
OperationType | 1 - installatie<br> 0 - zoeken naar en downloaden.| De installatie is de enige OperationType die standaard wordt weergegeven in de resultaten.
WindowsUpdateQuery | De standaardwaarde is "IsInstalled = 0" |Windows update-query die is gebruikt om te zoeken naar updates. Zie voor meer informatie, [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)
RebootRequired | True - is opnieuw opstarten vereist<br> False - is opnieuw opstarten niet vereist | Hiermee wordt aangegeven als opnieuw opstarten vereist voor het volledige installatie van updates is.

Als er geen update nog is gepland, wordt het resultaat JSON is leeg.

Meld u aan het cluster op Windows Update-query resultaten. Vervolgens de replica-adres voor de primaire van de Service Coordinator weten en klikt u op de URL van de browser: http://&lt;REPLICA-IP-&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1 / GetWindowsUpdateResults.

De REST-eindpunt voor de Coordinator-Service heeft een dynamische poort. Om te controleren of de exacte URL, raadpleegt u de Service Fabric Explorer. Bijvoorbeeld, de resultaten zijn beschikbaar op `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.

![Afbeelding van REST-eindpunt](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


Als de omgekeerde proxy is ingeschakeld op het cluster, kunt u de URL van buiten het cluster ook openen.
Het eindpunt dat moet worden bereikt is http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.

Als u wilt inschakelen in de omgekeerde proxy op het cluster, volg de stappen in [omgekeerde proxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy). 

> 
> [!WARNING]
> Nadat de omgekeerde proxy is geconfigureerd, kunnen alle microservices in het cluster die beschikbaar maken van een HTTP-eindpunt worden opgevraagd van buiten het cluster.

## <a name="diagnosticshealth-events"></a>Diagnostische gegevens/statusgebeurtenissen

### <a name="diagnostic-logs"></a>Diagnostische logboeken

Patch orchestration app-logboeken worden verzameld als onderdeel van de logboeken van de Service Fabric-runtime.

Als u vastleggen van Logboeken via diagnostische hulpprogramma/pijplijn van uw keuze wilt. Patch orchestration-toepassing gebruikt hieronder vaste provider-id's om aan te melden van gebeurtenissen via [eventsource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netframework-4.5.1)

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Statusrapporten

De patch orchestration-app publiceert ook statusrapporten tegen de Coordinator-Service of de Agent-Service van het knooppunt in de volgende gevallen:

#### <a name="a-windows-update-operation-failed"></a>Een Windows Update-bewerking is mislukt

Als een Windows Update-bewerking is mislukt op een knooppunt, wordt een statusrapport gegenereerd op basis van de Agent-Service van het knooppunt. Details van het statusrapport bevat de naam voor het problematische.

Na het toepassen van patches is voltooid op de problematische knooppunt, wordt het rapport automatisch gewist.

#### <a name="the-node-agent-ntservice-is-down"></a>De Agent knooppunt NTService is niet actief

Als de Agent knooppunt NTService niet actief op een knooppunt is, wordt een statusrapport op waarschuwingsniveau gegenereerd op basis van de Agent-Service van het knooppunt.

#### <a name="the-repair-manager-service-is-not-enabled"></a>De reparatie manager-service is niet ingeschakeld

Als de reparatie manager-service niet in het cluster gevonden is, wordt er een statusrapport op waarschuwingsniveau gegenereerd voor de Coordinator-Service.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

V. **Waarom zie ik mijn cluster in een foutstatus wanneer de patch orchestration-app wordt uitgevoerd?**

A. Tijdens het installatieproces de patch orchestration-app wordt uitgeschakeld of opnieuw wordt opgestart knooppunten die tijdelijk kunnen resulteren in de status van het cluster uitvalt.

Op basis van het beleid voor de toepassing, ofwel een knooppunt kan uitvallen tijdens een patch-bewerking *of* een hele upgradedomein tegelijkertijd kan uitvallen.

Aan het einde van de Windows Update-installatie, de knooppunten zijn ingeschakeld boeken opnieuw opstarten.

In het volgende voorbeeld wordt het cluster is een fout op een foutstatus tijdelijk omdat twee knooppunten niet actief was en het beleid MaxPercentageUnhealthyNodes is geschonden. De fout is tijdelijk totdat de patch-bewerking momenteel uitgevoerd wordt.

![Afbeelding van het cluster niet in orde](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Als het probleem zich blijft voordoen, raadpleegt u de sectie over probleemoplossing.

V. **Patch orchestration-app is in de waarschuwingsstatus heeft**

A. Controleer of een statusrapport geplaatst op basis van de toepassing de hoofdoorzaak is. De waarschuwing bevat meestal, details van het probleem. Als het probleem tijdelijk is, wordt automatisch herstellen van deze status van de toepassing verwacht.

V. **Wat moet ik doen als mijn cluster niet in orde is en ik wil een urgent besturingssysteemupdate doen?**

A. De patch orchestration-app heeft geen updates installeren, terwijl het cluster niet in orde is. Probeer uw cluster naar een goede status om de blokkering van de patch orchestration-app-werkstroom te brengen.

V. **Moet ik TaskApprovalPolicy als 'NodeWise' of 'UpgradeDomainWise' instellen voor mijn cluster?**

A. 'UpgradeDomainWise' maakt de algehele cluster patchen sneller patches voor alle knooppunten die behoren tot een upgradedomein tegelijkertijd. Dit betekent dat knooppunten die behoren tot een volledige upgradedomein niet beschikbaar zijn zou (in [uitgeschakelde](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabled) staat) tijdens het toepassen van patches.

Daarentegen 'NodeWise' beleid slechts één knooppunt tegelijk patches, dit betekent dat het algehele cluster patchen zou langere tijd in beslag nemen. Op de limiet is bereikt, slechts één knooppunt zou zijn echter niet beschikbaar (in [uitgeschakelde](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabled) staat) tijdens het toepassen van patches.

Als uw cluster uitgevoerd op het aantal upgradedomeinen N-1 tolereren kan tijdens het toepassen van patches cyclus (waarbij N staat voor het totale aantal upgradedomeinen in uw cluster), kunt u het beleid als 'UpgradeDomainWise' instellen, anders u dit instellen op 'NodeWise'.

V. **Hoeveel tijd doet het allemaal voor toets maken voor het vullen van een knooppunt?**

A. Patch toepassen op een knooppunt kan duren (bijvoorbeeld: [Definitie-updates voor Windows Defender](https://www.microsoft.com/wdsi/definitions)) uur (bijvoorbeeld: [Windows cumulatieve updates](https://www.catalog.update.microsoft.com/Search.aspx?q=windows%20server%20cumulative%20update)). Tijd die nodig is voor het vullen van een knooppunt is afhankelijk van voornamelijk op 
 - De grootte van updates
 - Aantal updates die moeten worden toegepast in een tijdvenster
 - De tijd die nodig is de updates worden geïnstalleerd, het knooppunt (indien nodig) opnieuw opstarten en stappen na opnieuw opstarten-installatie te voltooien.
 - Prestaties van virtuele machine/machine en netwerkomstandigheden.

V. **Hoe lang duurt het voor het vullen van een geheel cluster?**

A. De tijd die nodig is voor het vullen van een geheel cluster is afhankelijk van de volgende factoren:

- De tijd die nodig is voor het vullen van een knooppunt.
- Het beleid van de Coordinator-Service. -Het standaardbeleid `NodeWise`, resulteert in het patchen van slechts één knooppunt tegelijk, die langzamer dan zijn `UpgradeDomainWise`. Bijvoorbeeld: Als een knooppunt ~ 1 uur moet worden gevuld, als u wilt een 20 patch knooppunt (hetzelfde type knooppunten) van het cluster met 5 upgradedomeinen, elk met 4 knooppunten.
    - Het duurt ongeveer 20 uur voor het vullen van het hele cluster als het beleid is `NodeWise`
    - Het duurt ongeveer 5 uur als beleid is `UpgradeDomainWise`
- Belasting van de cluster - elke patch-bewerking moet de workload van een klant verplaatsen naar andere beschikbare knooppunten in het cluster. Knooppunt die een patch op zou zijn [Disabling](https://docs.microsoft.com/dotnet/api/system.fabric.query.nodestatus?view=azure-dotnet#System_Fabric_Query_NodeStatus_Disabling) status gedurende deze tijd. Als het cluster wordt uitgevoerd in de buurt van piekbelasting, zou de uitschakelen proces langer duren. Daarom lijkt algehele patchproces langzaam in dergelijke extreme omstandigheden.
- Elk cluster-statusfouten tijdens het toepassen van patches - [degradatie](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate?view=azure-dotnet#System_Fabric_Health_HealthState_Error) in [status van het cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-health-introduction) het patchproces onderbroken. Dit zou toevoegen aan de totale tijd die nodig is voor het vullen van het hele cluster.

V. **Waarom zie ik bepaalde updates in de resultaten van de Windows Update is verkregen via de REST API, maar niet in de geschiedenis van Windows Update op de machine?**

A. Sommige productupdates wordt alleen weergegeven in de geschiedenis van hun respectieve updates/patching. Bijvoorbeeld updates voor Windows Defender kunnen of mogelijk niet weergegeven in de geschiedenis van Windows Update op Windows Server 2016.

V. **Kan Patch Orchestration-app worden gebruikt voor het vullen van mijn dev-cluster (cluster met één knooppunt)?**

A. Nee, Patch orchestration-app kan niet worden gebruikt om patch cluster met één knooppunt. Deze beperking is standaard, als [service fabric-systeemservices](https://docs.microsoft.com/azure/service-fabric/service-fabric-technical-overview#system-services) of alle apps van de klant wordt krijgen met downtime en kan daarom elke taak herstellen voor het patchen van zouden nooit ophalen goedgekeurd door reparatiemanager.

## <a name="disclaimers"></a>Disclaimers

- De patch orchestration app accepteert de gebruiksrechtovereenkomst-licentie van Windows Update namens de gebruiker. In de configuratie van de toepassing kan eventueel de instelling worden uitgeschakeld.

- De patch orchestration app verzamelt telemetrie om gebruik en prestaties te houden. Van de toepassing telemetrie volgt de instelling van de Service Fabric-runtime telemetrie (dit is standaard ingeschakeld).

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="a-node-is-not-coming-back-to-up-state"></a>Een knooppunt komt niet back-up maken van status

**Het knooppunt kan blijven steken in een status uitschakelen omdat**:

Een selectievakje veiligheid is in behandeling. Zorg ervoor dat voldoende knooppunten beschikbaar in orde zijn als u wilt deze situatie oplossen.

**Het knooppunt kan een uitgeschakelde status zijn vastgelopen omdat**:

- Het knooppunt is handmatig uitgeschakeld.
- Het knooppunt is uitgeschakeld vanwege een lopende Azure-infrastructuur-taak.
- Het knooppunt is tijdelijk uitgeschakeld door de patch orchestration-app voor het vullen van het knooppunt.

**Het knooppunt kan blijven steken in een status down omdat**:

- Het knooppunt is geplaatst in een status down handmatig.
- Het knooppunt ondergaat een opnieuw opstarten (dit kan worden geactiveerd door de patch orchestration-app).
- Het knooppunt is niet beschikbaar vanwege een onjuiste VM of verbindingsproblemen machine of het netwerk.

### <a name="updates-were-skipped-on-some-nodes"></a>Updates zijn op een aantal knooppunten overgeslagen

De patch orchestration app probeert te installeren van een Windows-update op basis van het nieuwe beleid. De service probeert te herstellen van het knooppunt en overslaan van de update op basis van het toepassingenbeleid.

In dat geval wordt een waarschuwing het niveau statusrapport gegenereerd op basis van de Agent-Service van het knooppunt. Het resultaat voor Windows Update bevat ook de mogelijke oorzaak van het probleem.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a>De status van het cluster gaat u naar de fout terwijl de update is geïnstalleerd

Een defecte Windows update kan uitvallen van de status van een toepassing of het cluster op een bepaald knooppunt of een upgradedomein. De patch orchestration-app stoppen eventuele latere Windows Update-bewerking totdat het cluster weer in orde is.

Een beheerder moet waarbij en te bepalen waarom de toepassing of het cluster is geworden vanwege de Windows Update niet in orde.

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="version-110"></a>Versie 1.1.0
- Openbare versie

### <a name="version-111"></a>Versie 1.1.1
- Een bug in SetupEntryPoint van NodeAgentService waardoor de installatie van NodeAgentNTService niet kan worden opgelost.

### <a name="version-120"></a>Versie 1.2.0

- Werkstroom van oplossingen voor problemen rond systeem opnieuw starten.
- Opgelost probleem bij het maken van RM taken vanwege welke status selectievakje tijdens het voorbereiden van herstellen taken is niet gebeurt, zoals verwacht.
- De opstartmodus gewijzigd voor windows service POANodeSvc van automatische vertraagd automatisch.

### <a name="version-121"></a>Versie 1.2.1

- Opgelost probleem in cluster omlaag schalen werkstroom. Garbagecollection verzameling logica voor POA herstellen taken die behoren tot niet-bestaande knooppunten geïntroduceerd.

### <a name="version-122"></a>Versie 1.2.2

- Diverse oplossingen voor problemen.
- Binaire bestanden zijn nu aangemeld.
- Sfpkg-koppeling voor de toepassing hebt toegevoegd.

### <a name="version-130"></a>Versie 1.3.0

- InstallWindowsOSOnlyUpdates nu instellen op false, worden alle beschikbare updates geïnstalleerd.
- De logica van het uitschakelen van automatische updates is gewijzigd. Hiermee een bug opgelost wanneer Automatische updates niet ophalen van uitgeschakeld op Server 2016 en hoger.
- Plaatsing beperking voor zowel de microservices van POA voor geavanceerde usecases als parameters gebruikt.

### <a name="version-131"></a>Versie 1.3.1
- Regressie oplossen waar POA 1.3.0 werkt niet op Windows Server 2012 R2 of lager vanwege een fout bij het uitschakelen van automatische updates. 
- Bug opgelost waarbij InstallWindowsOSOnlyUpdates configuratie altijd wordt opgehaald als de waarde True.
- De standaardwaarde van InstallWindowsOSOnlyUpdates wijzigen in False.

### <a name="version-132"></a>Versie 1.3.2
- Een probleem dat het toepassen van patches leven cyle op een knooppunt heeft in het geval er knooppunten zijn met de naam subset van de naam van het huidige knooppunt is opgelost. Voor dergelijke knooppunten, de mogelijke patches ontbreekt of opnieuw opstarten in behandeling is. 
