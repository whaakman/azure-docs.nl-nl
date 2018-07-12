---
title: Azure Service Fabric-patch orchestration-toepassing voor linux | Microsoft Docs
description: De toepassing voor het automatiseren van besturingssysteem toepassen van patches op een Linux Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: novino
manager: timlt
editor: ''
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/22/2018
ms.author: nachandr
ms.openlocfilehash: 00e5f5a73973a34a8611143719c91a2b1ad0c8eb
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971263"
---
# <a name="patch-the-linux-operating-system-in-your-service-fabric-cluster"></a>Patch uitvoeren voor de Linux-besturingssysteem in uw Service Fabric-cluster

> [!div class="op_single_selector"]
> * [Windows](service-fabric-patch-orchestration-application.md)
> * [Linux](service-fabric-patch-orchestration-application-linux.md)
>
>

De patch orchestration-toepassing is een Azure Service Fabric-toepassing waarmee het besturingssysteem op een Service Fabric-cluster zonder uitvaltijd patches worden geautomatiseerd.

De patch orchestration-app biedt de volgende functies:

- **Automatische update besturingssysteeminstallatie**. Besturingssysteem-updates automatisch gedownload en geïnstalleerd. Clusterknooppunten worden opnieuw opgestart als dat nodig is zonder uitvaltijd van de cluster.

- **Cluster-aware toepassen van patches en de gezondheid van integratie**. Tijdens het toepassen van updates, controleert de patch orchestration-app de status van de clusterknooppunten. Knooppunten van het cluster zijn één knooppunt op een tijd of één upgradedomein tegelijk. Als de status van het cluster uitgeschakeld vanwege het patchproces wordt, is patching gestopt om te voorkomen dat het probleem toegenomen.

## <a name="internal-details-of-the-app"></a>Interne details van de app.

De patch orchestration-app bestaat uit de volgende onderdelen:

- **Service Coordinator**: deze stateful service is verantwoordelijk voor:
    - De taak van de Update van het besturingssysteem op het hele cluster coördineren.
    - Het opslaan van het resultaat van voltooide bewerkingen van de Update van het besturingssysteem.
- **Knooppunt-agentservice**: deze stateless service wordt uitgevoerd op alle knooppunten van de Service Fabric-cluster. De service is verantwoordelijk voor:
    - Opstarten van de Knooppuntagent-daemon op Linux.
    - De daemon-service te controleren.
- **Knooppunt Agent daemon**: deze Linux-daemon-service wordt uitgevoerd op een hoger niveau van bevoegdheden (root). Daarentegen de knooppunt Agent-Service en de Coordinator-Service uitgevoerd op een lager niveau van bevoegdheden. De service is verantwoordelijk voor het uitvoeren van de volgende Update-taken op alle clusterknooppunten:
    - Het uitschakelen van automatische Update van het besturingssysteem op het knooppunt.
    - Downloaden en installeren van Update van het besturingssysteem op basis van het beleid wordt de gebruiker is opgegeven.
    - Indien nodig, opnieuw op te starten met de machine na de installatie van de Update van het besturingssysteem.
    - De resultaten van de OS-updates worden geüpload naar de Coordinator-Service.
    - Rapportage status rapporteert als een bewerking is mislukt na het toewijzen van alle nieuwe pogingen.

> [!NOTE]
> De patch orchestration-app maakt gebruik van de Service Fabric herstellen manager systeemservice uitschakelen of inschakelen van het knooppunt en statuscontroles uitvoeren. De hersteltaak die zijn gemaakt door de patch orchestration app houdt de voortgang van de Update voor elk knooppunt.

## <a name="prerequisites"></a>Vereisten

### <a name="ensure-that-your-azure-vms-are-running-ubuntu-1604"></a>Zorg ervoor dat uw virtuele Azure-machines Ubuntu 16.04 worden uitgevoerd
Op het moment van schrijven van dit document, Ubuntu 16.04 (`Xenial Xerus`) is de enige ondersteunde versie.

### <a name="ensure-that-the-service-fabric-linux-cluster-is-version-62x-and-above"></a>Zorg ervoor dat de service fabric linux-cluster versie 6.2.x en hoger

Patch orchestration app linux maakt gebruik van bepaalde functies van de runtime die alleen beschikbaar in service fabric-runtime-versie 6.2.x en hoger.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>De reparatie manager-service inschakelen (indien deze niet al actief)

De patch orchestration-app moet de service manager herstellen wordt ingeschakeld op het cluster.

#### <a name="azure-clusters"></a>Azure-clusters

Azure linux-clusters in de Silver-/ en gold duurzaamheidslaag hebben de herstel-manager-service standaard ingeschakeld. Azure-clusters in de duurzaamheidslaag brons, standaard, beschikt niet over de reparatie manager-service is ingeschakeld. Als de service al is ingeschakeld, kunt u zien dat in de sectie van de services system in de Service Fabric Explorer.

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

Zelfstandige Service Fabric Linux-clusters worden niet ondersteund op het moment van schrijven van dit document.

### <a name="disable-automatic-os-update-on-all-nodes"></a>Uitschakelen van automatische Update van het besturingssysteem op alle knooppunten

Automatische updates van het besturingssysteem kunnen leiden tot verlies van beschikbaarheid en zijn of wijzigen in het gedrag van de actieve toepassingen. De patch orchestration-app probeert standaard om uit te schakelen van de automatische Update van het besturingssysteem op elk clusterknooppunt om te voorkomen dat dergelijke scenario's.
Voor Ubuntu [zonder toezicht-upgrades](https://help.ubuntu.com/community/AutomaticSecurityUpdates) zijn uitgeschakeld door een patch orchestration-app.

## <a name="download-the-app-package"></a>Het app-pakket downloaden

Toepassing echter samen met scripts voor installatie kan worden gedownload vanaf [archief koppeling](https://go.microsoft.com/fwlink/?linkid=867984).

Toepassing in sfpkg-indeling kan worden gedownload vanaf [sfpkg koppeling](https://go.microsoft.com/fwlink/?linkid=867984&pc=sfpkg). Dit is handig voor [implementatie van toepassing op basis van Azure Resource Manager](service-fabric-application-arm-resource.md).

## <a name="configure-the-app"></a>De app configureren

Het gedrag van de patch orchestration-app kan worden geconfigureerd om te voldoen aan uw behoeften. De standaardwaarden overschrijven door te geven in de parameter van de toepassing tijdens het maken van de toepassing of update. Parameters voor de toepassing kunnen worden opgegeven door op te geven `ApplicationParameter` naar de `Start-ServiceFabricApplicationUpgrade` of `New-ServiceFabricApplication` cmdlets.

|**Parameter**        |**Type**                          | **Details**|
|:-|-|-|
|MaxResultsToCache    |Lang                              | Maximum aantal resultaten van de Update, die moet worden opgeslagen in de cache. <br>Standaardwaarde is 3000 ervan uitgaande dat de: <br> -Het aantal knooppunten is 20. <br> -Het aantal updates dat op een knooppunt per maand gebeurt is vijf. <br> -Het aantal resultaten dat per bewerking is 10. <br> -Resultaten voor de afgelopen drie maanden moeten worden opgeslagen. |
|TaskApprovalPolicy   |Enum <br> {NodeWise, UpgradeDomainWise}                          |TaskApprovalPolicy geeft aan dat het beleid dat moet worden gebruikt door de Coordinator-Service voor het installeren van updates over de Service Fabric-clusterknooppunten.<br>                         Toegestane waarden zijn: <br>                                                           <b>NodeWise</b>. Updates zijn geïnstalleerd één knooppunt tegelijk. <br>                                                           <b>UpgradeDomainWise</b>. Updates zijn geïnstalleerd één upgradedomein tegelijk. (Het maximum is bereikt, alle knooppunten die behoren tot een upgradedomein kunnen gaan voor update.)
| UpdateOperationTimeOutInMinutes | Int <br>(Standaard: 180)                   | Hiermee geeft u de time-out voor een updatebewerking (downloaden of installeren). Als de bewerking is niet voltooid binnen de opgegeven time-out, wordt het afgebroken.       |
| RescheduleCount      | Int <br> (Standaard: 5).                  | Het maximum aantal keren dat de service opnieuw gepland voor het besturingssysteem bijwerken in het geval een bewerking blijft mislukken.          |
| RescheduleTimeInMinutes  | Int <br>(Standaard: 30). | Het interval waarmee de service opnieuw gepland het besturingssysteem bijwerken als de fout zich blijft voordoen. |
| UpdateFrequency           | Door komma's gescheiden tekenreeks op (standaard: "Wekelijks, woensdag, 7:00:00")     | De frequentie voor het installeren van updates van het besturingssysteem op het cluster. De indeling en de mogelijke waarden zijn: <br>-Maand, DD uu: mm:, bijvoorbeeld, maandelijks, 5, 12:22:32. <br> -Wekelijks, dag, uu: mm:, voor bijvoorbeeld wekelijks, dinsdag, 12:22:32.  <br> -Dagelijks, uu: mm:, bijvoorbeeld dagelijks, 12:22:32.  <br> -Geen geeft aan of update al dan niet mogen worden uitgevoerd.  <br><br> Alle tijden worden in UTC.|
| UpdateClassification | Door komma's gescheiden tekenreeks op (standaard: "securityupdates") | Het type updates dat moet worden geïnstalleerd op de clusterknooppunten. Acceptabele waarden zijn securityupdates, alle. <br> -securityupdates - zou alleen beveiligingsupdates installeren <br> -alle - installeert alle beschikbare updates van apt.|
| ApprovedPatches | Door komma's gescheiden tekenreeks op (standaard: ' ") | Dit is de lijst met goedgekeurde updates die moeten worden geïnstalleerd op de clusterknooppunten. De door komma's gescheiden lijst met bevat goedgekeurde pakketten en eventueel gewenste doelversie.<br> bijvoorbeeld: "apt-utils 1.2.10ubuntu1 python3-jwt, apt-transport-https < 1.2.194, libsystemd0 = > 229 4ubuntu16 =" <br> De bovenstaande wilt installeren <br> -apt-utils met versie 1.2.10ubuntu1 als deze beschikbaar zijn in apt-cache. Als deze specifieke versie beschikbaar is, is er geen. <br> -python3-jwt-upgrades naar de meest recente beschikbare versie. Als het pakket niet aanwezig is, is geen. <br> -upgrades naar hoogste versie die is minder dan 1.2.194 apt-transport-https. Als u deze versie niet aanwezig is, is geen. <br> -upgrades naar hoogste versie die hoger is dan of gelijk aan 229 4ubuntu16 libsystemd0. Als deze een versie niet bestaat, is geen.|
| RejectedPatches | Door komma's gescheiden tekenreeks op (standaard: ' ") | Dit is de lijst met updates die niet moeten worden geïnstalleerd op de clusterknooppunten <br> bijvoorbeeld: 'bash, sudo' <br> De voorgaande gefilterd bash, sudo ontvangen van updates. |


> [!TIP]
> Als u wilt dat de Update van het besturingssysteem te vinden direct plaats, stelt u `UpdateFrequency` ten opzichte van de tijd van de implementatie van toepassing. Stel bijvoorbeeld dat u een testcluster met vijf knooppunten hebt en wilt implementeren, de app om ongeveer 5:00 uur UTC. Als u wordt ervan uitgegaan dat de upgrade van de toepassing of de implementatie van 30 minuten maximaal duurt, stelt u de UpdateFrequency als "Dagelijks, 17:30:00."

## <a name="deploy-the-app"></a>De app implementeren

1. Bereid het cluster door de vereiste stappen is voltooid.
2. De patch orchestration-app, zoals elke andere Service Fabric-app implementeren. U kunt de app implementeren met behulp van PowerShell of Azure Service Fabric-CLI. Volg de stappen in [implementeren en remove-toepassingen met behulp van PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) of [toepassing implementeren met behulp van Azure Service Fabric-CLI](https://docs.microsoft.com/azure/service-fabric/scripts/cli-deploy-application)
3. Voor het configureren van de toepassing op het moment van implementatie, geven de `ApplicationParamater` naar de `New-ServiceFabricApplication` cmdlet of de scripts opgegeven. Voor uw gemak powershell (Deploy.ps1) en (Deploy.sh) bash-scripts vindt u samen met de toepassing. Het script gebruiken:

    - Verbinding maken met een Service Fabric-cluster.
    - Voer het script implementeren. (Optioneel) de toepassing-parameter doorgeven aan het script. bijvoorbeeld:.\Deploy.ps1 - ApplicationParameter @{UpdateFrequency = "Dagelijks, 11:00:00"} of./Deploy.sh "{\"UpdateFrequency\":\"dagelijks, 11:00:00\"}" 

> [!NOTE]
> Houd het script en de toepassingsmap PatchOrchestrationApplication in dezelfde map.

## <a name="upgrade-the-app"></a>Upgrade van de app.

Als u een bestaande patch orchestration-app bijwerken, volg de stappen in [upgrade van de Service Fabric-toepassing met behulp van PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell) of [upgrade van de Service Fabric-toepassing met behulp van Azure Service Fabric-CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-upgrade)

## <a name="remove-the-app"></a>De app verwijderen

Volg de stappen in de toepassing te verwijderen, [implementeren en remove-toepassingen met behulp van PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) of [verwijderen van een toepassing met behulp van Azure Service Fabric-CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-delete)

Voor uw gemak powershell (Undeploy.ps1) en (Undeploy.sh) bash-scripts vindt u samen met de toepassing. Het script gebruiken:

  - Verbinding maken met een Service Fabric-cluster.
  - Voer het script Undeploy.ps1 of Undeploy.sh

> [!NOTE]
> Houd het script en de toepassingsmap PatchOrchestrationApplication in dezelfde map.

## <a name="view-the-update-results"></a>De Update-resultaten weergeven

De patch orchestration-app beschikbaar REST-API's om de historische resultaten voor de gebruiker weer te geven. Hieronder volgt een voorbeeld-resultaat: ```testadm@bronze000001:~$ curl -X GET http://10.0.0.5:20002/PatchOrchestrationApplication/v1/GetResults```
```json
[ 
  { 
    "NodeName": "_bronze_0", 
    "UpdateOperationResults": [ 
      { 
        "OperationResult": "succeeded", 
        "NodeName": "_bronze_0", 
        "OperationTime": "2017-11-21T12:39:29.0435917Z", 
        "UpdateDetails": [ 
          { 
            "UpdateId": "linux-cloud-tools-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-headers-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-image-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "linux-tools-azure:amd64=4.11.0.1015.15", 
            "ResultCode": "succeeded" 
          }, 
          { 
            "UpdateId": "python3-apport:amd64=2.20.1-0ubuntu2.13", 
            "ResultCode": "succeeded" 
          }, 
        ], 
        "OperationType": "installation", 
        "UpdateClassification": "securityupdates", 
        "UpdateFrequency": "Daily, 7:00:00", 
        "RebootRequired": true, 
        "ApprovedList": "", 
        "RejectedList": "" 
      } 
    ] 
  } 
] 
```

Velden van de JSON worden als volgt beschreven:

Veld | Waarden | Details
-- | -- | --
Kan operationresult niet | 0 - geslaagd<br> 1 - is voltooid met fouten<br> 2 - is mislukt<br> 3 - afgebroken<br> 4 - afgebroken met time-out | Geeft aan dat het resultaat van de algehele bewerking (meestal met betrekking tot installatie van updates voor een of meer).
ResultCode | Kan operationresult niet gelijk | Dit veld wordt resultaat van de installatiebewerking voor individuele update aangegeven.
OperationType | 1 - installatie<br> 0 - zoeken naar en downloaden.| De installatie is de enige OperationType die standaard wordt weergegeven in de resultaten.
UpdateClassification | De standaardwaarde is "securityupdates" | Type updates dat tijdens de updatebewerking is geïnstalleerd
UpdateFrequency | De standaardwaarde is "Wekelijks, woensdag, 7:00:00" | Bijwerkfrequentie geconfigureerd voor deze update.
RebootRequired | True - is opnieuw opstarten vereist<br> False - is opnieuw opstarten niet vereist | Geeft aan dat een opnieuw opstarten is vereist voor het volledige installatie van updates.
ApprovedList | De standaardwaarde is "" | Lijst met goedgekeurde patches voor deze update
RejectedList | De standaardwaarde is "" | Lijst met geweigerde patches voor deze update

Als er geen update nog is gepland, wordt het resultaat JSON is leeg.

Meld u aan bij het cluster met queryresultaten voor Update. Vervolgens de replica-adres voor de primaire van de Service Coordinator weten en klikt u op de URL van de browser: http://&lt;REPLICA-IP-&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetResults .

De REST-eindpunt voor de Coordinator-Service heeft een dynamische poort. Om te controleren of de exacte URL, raadpleegt u de Service Fabric Explorer. Bijvoorbeeld, de resultaten zijn beschikbaar op `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetResults`.

![Afbeelding van REST-eindpunt](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)

## <a name="diagnosticshealth-events"></a>Diagnostische gegevens/statusgebeurtenissen

### <a name="diagnostic-logs"></a>Diagnostische logboeken

Patch orchestration app-logboeken worden verzameld als onderdeel van de logboeken van de Service Fabric-runtime.

Als u vastleggen van Logboeken via diagnostische hulpprogramma/pijplijn van uw keuze wilt. Patch orchestration-toepassing gebruikt de volgende vaste provider-id's om aan te melden van gebeurtenissen via [eventsource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netstandard-2.0)

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Statusrapporten

De patch orchestration-app publiceert ook statusrapporten tegen de Coordinator-Service of de Agent-Service van het knooppunt in de volgende gevallen:

#### <a name="an-update-operation-failed"></a>Een updatebewerking is mislukt

Als een updatebewerking is mislukt op een knooppunt, wordt een statusrapport gegenereerd op basis van de Agent-Service van het knooppunt. Details van het statusrapport bevat de naam voor het problematische.

Na het toepassen van patches is voltooid op de problematische knooppunt, wordt het rapport automatisch gewist.

#### <a name="the-node-agent-daemon-service-is-down"></a>Het knooppunt Agent Daemon-Service is niet actief

Als het knooppunt Agent Daemon-service niet actief op een knooppunt is, wordt een statusrapport op waarschuwingsniveau gegenereerd op basis van de Agent-Service van het knooppunt.

#### <a name="the-repair-manager-service-is-not-enabled"></a>De reparatie manager-service is niet ingeschakeld

Een statusrapport op waarschuwingsniveau wordt gegenereerd voor de Service Coordinator als herstel manager-service niet in het cluster gevonden is.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

Q. **Waarom zie ik mijn cluster in een foutstatus wanneer de patch orchestration-app wordt uitgevoerd?**

A. Tijdens de installatie, wordt de patch orchestration-app wordt uitgeschakeld of knooppunten opnieuw wordt opgestart. Met deze bewerking kan tijdelijk resulteren in de status van het cluster uitvalt.

Op basis van het beleid voor de toepassing, ofwel een knooppunt kan uitvallen tijdens een patch-bewerking *of* een hele upgradedomein tegelijkertijd kan uitvallen.

Aan het einde van de installatie van de knooppunten zijn ingeschakeld boeken opnieuw opstarten.

In het volgende voorbeeld wordt het cluster is een fout op een foutstatus tijdelijk omdat twee knooppunten niet actief was en het beleid MaxPercentageUnhealthyNodes is geschonden. De fout is tijdelijk totdat de patch-bewerking momenteel uitgevoerd wordt.

![Afbeelding van het cluster niet in orde](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Als het probleem zich blijft voordoen, raadpleegt u de sectie over probleemoplossing.

Q. **Patch orchestration-app is in de waarschuwingsstatus heeft**

A. Controleer of een statusrapport geplaatst op basis van de toepassing de hoofdoorzaak is. De waarschuwing bevat meestal, details van het probleem. Als het probleem tijdelijk is, wordt automatisch herstellen van deze status van de toepassing verwacht.

Q. **Wat moet ik doen als mijn cluster niet in orde is en ik wil een urgent besturingssysteemupdate doen?**

A. De patch orchestration-app heeft geen updates installeren, terwijl het cluster niet in orde is. Als u wilt deblokkeren van de patch orchestration-app-werkstroom, breng uw cluster een goede status.

Q. **Waarom patches in meerdere computerclusters duurt zo lang om uit te voeren?**

A. De tijd die nodig zijn voor de patch orchestration-app is voornamelijk afhankelijk van de volgende factoren:

- Het beleid van de Coordinator-Service. 
  - Het standaardbeleid `NodeWise`, resulteert in het patchen van slechts één knooppunt tegelijk. Met name als er een grotere cluster, raden wij aan dat u de `UpgradeDomainWise` beleid voor een snellere patches voor het volledige cluster.
- Het aantal updates beschikbaar voor downloaden en installeren. 
- De gemiddelde tijd die nodig zijn om te downloaden en installeren van een update die mag niet meer dan een paar uur.
- De prestaties van de virtuele machine en netwerkbandbreedte.

Q. **Hoe kiest, wordt er patch orchestration app beslist welke updates zijn beveiligingsupdates.**

A. Patch orchestration-app maakt gebruik van distributie-specifieke logica om te bepalen welke updates tussen de beschikbare updates zijn beveiligingsupdates. Bijvoorbeeld: In de app wordt gezocht naar updates van archieven $RELEASE ubuntu-beveiliging, $RELEASE-updates ($RELEASE = xenial of de linux standard base release-versie). 

 
Q. **Hoe kan ik u aan bij een specifieke versie van pakket vergrendelen?**

A. Gebruik de instellingen ApprovedPatches uw pakketten op een bepaalde versie vergrendelen. 


Q. **Wat gebeurt er met automatische updates in Ubuntu ingeschakeld?**

A. Als u patch orchestration-app op uw cluster installeren, wordt zonder toezicht-upgrades in uw clusterknooppunt uitgeschakeld. Alle de werkstroom voor periodieke updates zou worden aangestuurd door patch orchestration-app.
Als u consistentie van de omgeving voor het volledige cluster, wordt u aangeraden de updates via patch orchestration alleen app te installeren. 
 
Q. **Upgrade boeken vullen van de orchestration-app kan het opschonen van niet-gebruikte pakketten?**

A. Ja, het opruimen gebeurt er als onderdeel van de stappen na de installatie. 

Q. **Kan Patch Orchestration-app worden gebruikt voor het vullen van mijn dev-cluster (cluster met één knooppunt)?**

A. Nee, Patch orchestration-app kan niet worden gebruikt om patch cluster met één knooppunt. Deze beperking is standaard, als [service fabric-systeemservices](https://docs.microsoft.com/azure/service-fabric/service-fabric-technical-overview#system-services) of alle apps van de klant wordt krijgen met downtime en kan daarom elke taak herstellen voor het patchen van zouden nooit ophalen goedgekeurd door reparatiemanager.

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

De patch orchestration app probeert te installeren van een update op basis van het nieuwe beleid. De service probeert te herstellen van het knooppunt en overslaan van de update op basis van het toepassingenbeleid.

In dat geval wordt een waarschuwing het niveau statusrapport gegenereerd op basis van de Agent-Service van het knooppunt. Het resultaat voor de update bevat ook de mogelijke oorzaak van het probleem.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a>De status van het cluster gaat u naar de fout terwijl de update is geïnstalleerd

Een defecte update kan de status van een toepassing of het cluster op een bepaald knooppunt of upgradedomein uitvallen. De patch orchestration-app stoppen alle volgende update-bewerkingen totdat het cluster weer in orde is.

Een beheerder moet waarbij en te bepalen waarom de toepassing of het cluster is geworden vanwege een eerder geïnstalleerde update niet in orde.

## <a name="disclaimer"></a>Vrijwaring

De patch orchestration app verzamelt telemetrie om gebruik en prestaties te houden. Van de toepassing telemetrie volgt de instelling van de Service Fabric-runtime telemetrie (dit is standaard ingeschakeld).

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="version-010"></a>Versie 0.1.0
- Private preview-versie

### <a name="version-200"></a>Versie 2.0.0
- Openbare versie

### <a name="version-201-latest"></a>Versie 2.0.1 (recentste)
- De app met de meest recente Service Fabric SDK gecompileerd
