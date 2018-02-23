---
title: Azure Service Fabric patch orchestration-toepassing voor linux | Microsoft Docs
description: De toepassing te automatiseren besturingssysteem patchen op een Linux-Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: novino
manager: timlt
editor: 
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/22/2018
ms.author: nachandr
ms.openlocfilehash: dac8068705e284b04d84d128eb1ce62c459d44ff
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="patch-the-linux-operating-system-in-your-service-fabric-cluster"></a>Patch voor het Linux-besturingssysteem in uw Service Fabric-cluster

> [!div class="op_single_selector"]
> * [Windows](service-fabric-patch-orchestration-application.md)
> * [Linux](service-fabric-patch-orchestration-application-linux.md)
>
>

De patch orchestration-toepassing is een Azure Service Fabric-toepassing die op een Service Fabric-cluster zonder uitvaltijd patchen besturingssysteem automatiseert.

De patch orchestration-app biedt de volgende functies:

- **Automatische update besturingssysteeminstallatie**. Besturingssysteem-updates automatisch gedownload en geïnstalleerd. Clusterknooppunten worden opgestart naar behoefte zonder uitvaltijd van de cluster.

- **Clusterbewust patchen en health integratie**. Tijdens het toepassen van updates controleert de patch orchestration app de status van de clusterknooppunten. Clusterknooppunten worden één knooppunt of één upgradedomein tegelijk. Als de status van het cluster als gevolg van de patch-proces uitvalt, is patchen om te voorkomen dat het probleem toegenomen gestopt.

## <a name="internal-details-of-the-app"></a>Interne details van de app.

De patch orchestration app bestaat uit de volgende onderdelen:

- **Service Coordinator**: deze stateful service is verantwoordelijk voor:
    - De OS-Update-taak op het hele cluster coördineren.
    - Het opslaan van het resultaat van de voltooide OS-Update-bewerkingen.
- **Knooppunt-agentservice**: deze staatloze service wordt uitgevoerd op alle clusterknooppunten van de Service Fabric. De service is verantwoordelijk voor:
    - Uitvoeren van de bootstrap knooppunt Agent-daemon op Linux.
    - De daemonservice te controleren.
- **Knooppunt Agent daemon**: deze Linux-daemon-service wordt uitgevoerd op een hoger niveau van bevoegdheden (root). Daarentegen de knooppunt Agent-Service en de coördinator-Service worden uitgevoerd op een lager niveau van bevoegdheden. De service is verantwoordelijk voor het uitvoeren van de volgende Update-taken op alle clusterknooppunten:
    - Het uitschakelen van automatische OS-updates op het knooppunt.
    - Downloaden en installeren van de OS-Update volgens het beleid voor is de gebruiker opgegeven.
    - Opnieuw starten van de machine na installatie van de OS-Update indien nodig.
    - De resultaten van het besturingssysteem kan worden bijgewerkt naar de coördinator-Service geüpload.
    - Reporting systeemstatusrapporten voor het geval is mislukt na het toewijzen van alle nieuwe pogingen.

> [!NOTE]
> De patch orchestration-app gebruikmaakt van de Service Fabric reparatie manager service uitschakelen of het inschakelen van het knooppunt en het uitvoeren van statuscontroles. De hersteltaak gemaakt door de patch orchestration app houdt de voortgang van de Update voor elk knooppunt.

## <a name="prerequisites"></a>Vereisten

### <a name="ensure-that-your-azure-vms-are-running-ubuntu-1604"></a>Zorg ervoor dat uw Azure VM's Ubuntu 16.04 worden uitgevoerd
Op het moment van schrijven van dit document, Ubuntu 16.04 (`Xenial Xerus`) is de enige ondersteunde versie.

### <a name="ensure-that-the-service-fabric-linux-cluster-is-version-61x-and-above"></a>Zorg ervoor dat de service fabric-cluster linux versie 6.1.x en hoger

Patch orchestration app linux maakt gebruik van bepaalde functies van de runtime die alleen beschikbaar in service fabric-runtimeversie zijn 6.1.x en hoger.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>De reparatie manager-service inschakelen (indien deze niet al actief)

De patch orchestration-app is vereist voor de service manager herstellen moet zijn ingeschakeld op het cluster.

#### <a name="azure-clusters"></a>Azure-clusters

Azure linux-clusters in de zilver en laag goud duurzaamheid hebben de herstel-service manager standaard ingeschakeld. Azure-clusters in de laag Brons duurzaamheid standaard beschikt niet over de reparatie manager service is ingeschakeld. Als de service al is ingeschakeld, ziet u in het gedeelte van de services system op de Service Fabric Explorer draait.

##### <a name="azure-portal"></a>Azure Portal
U kunt herstel manager vanuit Azure-portal inschakelen op het moment van het instellen van het cluster. Selecteer **omvatten herstel Manager** onder de optie **extra functies** op het moment van de configuratie van het cluster.
![Afbeelding van inschakelen van herstel-Manager vanuit Azure-portal](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)

##### <a name="azure-resource-manager-deployment-model"></a>Azure Resource Manager-implementatiemodel
U kunt ook de [Azure Resource Manager-implementatiemodel](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) zodat de manager-service voor herstel op nieuwe en bestaande Service Fabric-cluster. Haal de sjabloon voor het cluster dat u wilt implementeren. U kunt de voorbeeldsjablonen gebruiken of een aangepaste sjabloon van Azure Resource Manager deployment model maken. 

Om in te schakelen reparatie manager service gebruikmaakt [Azure Resource Manager deployment model sjabloon](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):

1. Controleer eerst of de `apiversion` is ingesteld op `2017-07-01-preview` voor de `Microsoft.ServiceFabric/clusters` resource. Als dit afwijkt, moet u bijwerken de `apiVersion` op de waarde `2017-07-01-preview` of hoger:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Nu de reparatie manager-service inschakelen door het volgende toe te voegen `addonFeatures` sectie na de `fabricSettings` sectie:

    ```json
    "fabricSettings": [
        ...      
    ],
    "addonFeatures": [
        "RepairManager"
    ],
    ```

3. Nadat u de sjabloon voor het cluster hebt bijgewerkt met deze wijzigingen, worden ze toepassen en kunt u de upgrade voltooien. U ziet nu de reparatie manager systeemservice in uw cluster wordt uitgevoerd. Wordt aangeroepen `fabric:/System/RepairManagerService` in het gedeelte van de services system op de Service Fabric Explorer. 

### <a name="standalone-on-premises-clusters"></a>Zelfstandige lokale clusters

Zelfstandige Service Fabric Linux-clusters worden niet ondersteund op het moment van schrijven in dit document.

### <a name="disable-automatic-os-update-on-all-nodes"></a>Automatische Update van het besturingssysteem op alle knooppunten uitschakelen

Automatische updates voor het besturingssysteem kunnen leiden tot verlies van beschikbaarheid en zijn of wijzigen van het gedrag van de actieve toepassingen. De patch orchestration-app, probeert standaard, u de automatische OS Update op elk clusterknooppunt om te voorkomen dat dergelijke scenario's uitschakelen.
Voor Ubuntu [zonder toezicht upgrades](https://help.ubuntu.com/community/AutomaticSecurityUpdates) door patch orchestration app zijn uitgeschakeld.

## <a name="download-the-app-package"></a>Download het apppakket

Downloaden van de toepassing van de [downloadkoppeling](https://go.microsoft.com/fwlink/?linkid=867984).

## <a name="configure-the-app"></a>De app configureren

Het gedrag van de patch orchestration-app kan worden geconfigureerd om te voldoen aan uw behoeften. De standaardwaarden overschrijven door door te geven in de parameter van de toepassing tijdens het maken van de toepassing of update. Parameters voor de toepassing kunnen worden opgegeven door te geven `ApplicationParameter` naar de `Start-ServiceFabricApplicationUpgrade` of `New-ServiceFabricApplication` cmdlets.

|**Parameter**        |**Type**                          | **Details**|
|:-|-|-|
|MaxResultsToCache    |Lang                              | Maximum aantal resultaten van de Update, die in de cache moet worden opgeslagen. <br>Standaardwaarde is 3000 ervan uitgaande dat de: <br> -Het aantal knooppunten is 20. <br> -Het aantal updates dat op een knooppunt per maand gebeurt is vijf. <br> -Het aantal resultaten per bewerking is 10. <br> -Resultaten voor de afgelopen drie maanden moeten worden opgeslagen. |
|TaskApprovalPolicy   |Enum <br> {NodeWise, UpgradeDomainWise}                          |TaskApprovalPolicy geeft aan het beleid dat moet worden gebruikt door de coördinator-Service om updates te installeren op de knooppunten Service Fabric-cluster.<br>                         Toegestane waarden zijn: <br>                                                           <b>NodeWise</b>. Updates zijn geïnstalleerd, één knooppunt tegelijk. <br>                                                           <b>UpgradeDomainWise</b>. Updates zijn geïnstalleerd één upgradedomein tegelijk. (Het maximum is bereikt, alle knooppunten van een upgradedomein gaan voor update.)
| UpdateOperationTimeOutInMinutes | Int <br>(Standaard: 180)                   | Hiermee geeft u de time-out voor een updatebewerking (downloaden of installeren). Als de bewerking is niet voltooid binnen de opgegeven time-out, wordt het afgebroken.       |
| RescheduleCount      | Int <br> (Standaard: 5).                  | Het maximum aantal keren dat de service opnieuw gepland voor het besturingssysteem bijwerken als een bewerking blijft mislukken.          |
| RescheduleTimeInMinutes  | Int <br>(Standaard: 30). | Het interval waarmee de service wordt automatisch opnieuw gepland het besturingssysteem bijwerken als de fout zich blijft voordoen. |
| UpdateFrequency           | Door komma's gescheiden tekenreeks (standaard: "Wekelijks, woensdag, 7:00:00")     | De frequentie voor het installeren van updates voor het besturingssysteem op het cluster. De indeling en de mogelijke waarden zijn: <br>-Maandelijks, DD: mm: ss, bijvoorbeeld, maandelijks, 5, 12:22:32. <br> -Per week, dag,: mm: ss, voor bijvoorbeeld wekelijks, dinsdag, 12:22:32.  <br> -Dagelijks: mm: ss, bijvoorbeeld dagelijks, 12:22:32.  <br> -Geen geeft aan dat de update mag niet worden uitgevoerd.  <br><br> Alle tijden zijn in UTC.|
| UpdateClassification | Door komma's gescheiden tekenreeks (standaard: 'securityupdates') | Type van de updates die moeten worden geïnstalleerd op de clusterknooppunten. Acceptabele waarden zijn securityupdates, alle. <br> -securityupdates - zouden alleen beveiligingsupdates installeren <br> -all - installeert alle beschikbare updates van enz.|
| ApprovedPatches | Door komma's gescheiden tekenreeks (standaard: ' ") | Dit is de lijst met goedgekeurde updates die moeten worden geïnstalleerd op de clusterknooppunten. De door komma's gescheiden lijst bevat goedgekeurde pakketten en eventueel gewenste doelversie.<br> bijvoorbeeld: ' apt utils 1.2.10ubuntu1, python3 jwt, apt-transport-https < 1.2.194, libsystemd0 = > 229 4ubuntu16 = " <br> De bovenstaande zou installeren <br> -apt utils met versie 1.2.10ubuntu1 indien ze beschikbaar zijn in apt-cache. Als dat bepaalde versie niet beschikbaar is, is geen. <br> -python3 jwt-upgrades naar de meest recente versie. Als het pakket niet aanwezig is is, is geen. <br> -apt-transport-https-upgrades naar de hoogste versie die minder dan 1.2.194 is. Als u deze versie niet aanwezig is, is geen. <br> -libsystemd0 upgrades naar de hoogste versie die groter is dan of gelijk aan 229 4ubuntu16. Als deze versie niet bestaat nog, is er geen.|
| RejectedPatches | Door komma's gescheiden tekenreeks (standaard: ' ") | Dit is de lijst met updates die niet mag worden geïnstalleerd op de clusterknooppunten <br> bijvoorbeeld: 'bash, sudo' <br> De voorgaande gefilterd bash, sudo ontvangen van updates. |


> [!TIP]
> Als u OS updates direct plaatsvindt wilt, stelt u `UpdateFrequency` ten opzichte van de tijd van de implementatie van toepassing. Stel bijvoorbeeld dat u hebt een testcluster met vijf knooppunten en plan de implementatie van de app op ongeveer 5:00 uur UTC. Als u wordt ervan uitgegaan dat de upgrade van de toepassing of implementatie 30 minuten maximaal duurt, ingesteld op de UpdateFrequency "Dagelijks, 17:30:00."

## <a name="deploy-the-app"></a>De app implementeren

1. Het cluster voorbereiden door de vereiste stappen is voltooid.
2. Implementeer de patch orchestration-app als elke andere Service Fabric-app. U kunt de app implementeren met behulp van PowerShell of CLI van Azure Service Fabric. Volg de stappen in [implementeren en verwijderen van toepassingen via PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) of [met CLI van Azure Service Fabric-toepassing implementeren](https://docs.microsoft.com/azure/service-fabric/scripts/cli-deploy-application)
3. De toepassing configureren op het moment van implementatie, de `ApplicationParamater` naar de `New-ServiceFabricApplication` cmdlet of de scripts opgegeven. Powershell (Deploy.ps1) en (Deploy.sh) bash-scripts worden voor uw gemak geleverd samen met de toepassing. Het script gebruiken:

    - Verbinding maken met een Service Fabric-cluster.
    - Voer het script implementeren. Optioneel de toepassing parameter doorgeven aan het script. bijvoorbeeld:.\Deploy.ps1 - ApplicationParameter @{UpdateFrequency = "Dagelijks, 11:00:00"} OR./Deploy.sh ' {\"UpdateFrequency\":\"dagelijks, 11:00:00\"} " 

> [!NOTE]
> Houd het script en de map application PatchOrchestrationApplication in dezelfde map.

## <a name="upgrade-the-app"></a>De app bijwerken

Upgrade van een bestaande patch orchestration app moet u de stappen in [upgrade van de Service Fabric-toepassing met behulp van PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell) of [upgrade van de Service Fabric-toepassing met Azure Service Fabric CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-upgrade)

## <a name="remove-the-app"></a>De app verwijderen

Volg de stappen in de toepassing te verwijderen, [implementeren en verwijderen van toepassingen via PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) of [een toepassing met Azure Service Fabric CLI verwijderen](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-delete)

Powershell (Undeploy.ps1) en (Undeploy.sh) bash-scripts worden voor uw gemak geleverd samen met de toepassing. Het script gebruiken:

  - Verbinding maken met een Service Fabric-cluster.
  - Het script Undeploy.ps1 of Undeploy.sh uitvoeren

> [!NOTE]
> Houd het script en de map application PatchOrchestrationApplication in dezelfde map.

## <a name="view-the-update-results"></a>Bekijk de resultaten van de Update

De patch orchestration app beschrijft de REST-API's om de historische resultaten voor de gebruiker weer te geven. Hier volgt een voorbeeld-resultaat: ```testadm@bronze000001:~$ curl -X GET http://10.0.0.5:20002/PatchOrchestrationApplication/v1/GetResults```
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
OperationResult | 0 - geslaagd<br> 1 - is voltooid met fouten<br> 2 - is mislukt<br> 3 - afgebroken<br> 4 - afgebroken met time-out | Geeft het resultaat van de algehele bewerking (meestal met betrekking tot installatie van een of meer updates).
ResultCode | Zelfde als OperationResult | Dit veld wordt het resultaat van de installatiebewerking voor individuele update aangegeven.
OperationType | 1 - installatie<br> 0 - te zoeken en downloaden.| De installatie is de enige OperationType die standaard wordt weergegeven in de resultaten.
UpdateClassification | De standaardwaarde is "securityupdates" | Type-updates die tijdens de updatebewerking is geïnstalleerd
UpdateFrequency | De standaardwaarde is "Wekelijks, woensdag, 7:00:00" | Werk de synchronisatiefrequentie gebruikt die is geconfigureerd voor deze update.
RebootRequired | waar - is opnieuw opstarten vereist<br> ONWAAR - is opnieuw opstarten niet vereist | Hiermee wordt aangegeven op dat een herstart is vereist voor volledige installatie van updates.
ApprovedList | Standaardwaarde is "" | Lijst met goedgekeurde patches voor deze update
RejectedList | Standaardwaarde is "" | Lijst met geweigerde patches voor deze update

Als er geen update nog is gepland, is het resultaat JSON is leeg.

Aanmelden bij het cluster om de resultaten van de query-Update. Vervolgens weten het adres van de replica voor de primaire van de Service Coordinator en klik op de URL van de browser: http://&lt;REPLICA-IP-&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetResults .

De REST-eindpunt voor de coördinator-Service heeft een dynamische poort. Als u wilt de exacte URL controleren, raadpleegt u de Service Fabric Explorer. Bijvoorbeeld, de resultaten zijn beschikbaar op `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetResults`.

![Afbeelding van REST-eindpunt](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)

## <a name="diagnosticshealth-events"></a>Diagnostische gegevens/health-gebeurtenissen

### <a name="diagnostic-logs"></a>Diagnostische logboeken

Patch orchestration app logboeken worden bijgehouden als onderdeel van Service Fabric-runtime-Logboeken.

Als u vastleggen logboeken via diagnostische hulpprogramma/pipeline van uw keuze wilt. Patch orchestration toepassing gebruikt na vaste provider id's gebeurtenissen via [eventsource](https://docs.microsoft.com/dotnet/api/system.diagnostics.tracing.eventsource?view=netstandard-2.0)

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

### <a name="health-reports"></a>Statusrapporten

De patch orchestration-app publiceert ook statusrapporten op basis van de coördinator-Service of de Agent-Service van het knooppunt in de volgende gevallen:

#### <a name="an-update-operation-failed"></a>Update-bewerking is mislukt

Als een updatebewerking is mislukt op een knooppunt, wordt een statusrapport gegenereerd op basis van de Agent-Service van het knooppunt. Details van het statusrapport bevatten problematisch knooppuntnaam.

Nadat patching is voltooid op het knooppunt problematisch, wordt het rapport automatisch gewist.

#### <a name="the-node-agent-daemon-service-is-down"></a>Het knooppunt Agent-Daemon is niet actief

Als de Daemon van knooppunt Agent-service niet actief op een knooppunt is, wordt een waarschuwing het niveau statusrapport gegenereerd op basis van de Agent-Service van het knooppunt.

#### <a name="the-repair-manager-service-is-not-enabled"></a>De reparatie manager-service is niet ingeschakeld

Een statusrapport van het niveau van de waarschuwing wordt gegenereerd voor de coördinator-Service als herstel manager-service niet op het cluster gevonden is.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

Q. **Waarom zie ik mijn cluster in een foutstatus wanneer de patch orchestration-app wordt uitgevoerd?**

A. Tijdens het installatieproces de patch orchestration-app wordt uitgeschakeld of knooppunten opnieuw wordt opgestart. Deze bewerking kan tijdelijk resulteren in de status van het cluster tijdelijk niet beschikbaar.

Op basis van het beleid voor de toepassing, ofwel een knooppunt kan uitvallen tijdens een bewerking van de toepassing van patches *of* een hele upgradedomein tegelijkertijd kan uitvallen.

Aan het einde van de installatie van de knooppunten zijn ingeschakeld boeken opnieuw opstarten.

In het volgende voorbeeld wordt het cluster is verzonden naar een foutstatus tijdelijk omdat twee knooppunten zijn en het beleid MaxPercentageUnhealthyNodes is geschonden. De fout is tijdelijk totdat de toepassing van patches bewerking uitgevoerd wordt.

![Afbeelding van slecht cluster](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Als het probleem zich blijft voordoen, raadpleegt u de sectie probleemoplossing.

Q. **Patch orchestration app heeft de waarschuwingsstatus**

A. Controleer of een statusrapport geplaatst voor de toepassing de hoofdoorzaak is. De waarschuwing bevat meestal, details van het probleem. Als het probleem tijdelijke is, wordt de toepassing automatisch herstellen van deze status verwacht.

Q. **Wat moet ik doen als mijn cluster beschadigd is en moet ik doen urgente besturingssysteem bijwerken?**

A. De patch orchestration app terwijl het cluster niet in orde is niet geïnstalleerd met updates. Als u wilt deblokkeren van de patch orchestration app werkstroom, zorg ervoor dat uw cluster status in orde.

Q. **Waarom patchen tussen verschillende clusters duurt te lang om uit te voeren?**

A. De tijd die nodig is door de patch orchestration-app is voornamelijk afhankelijk van de volgende factoren:

- Het beleid van de coördinator-Service. 
  - Het standaardbeleid `NodeWise`, resulteert in het patchen slechts één knooppunt tegelijk. Met name als er een groter cluster, het is raadzaam dat u de `UpgradeDomainWise` beleid sneller patchen hele cluster.
- Het aantal beschikbare updates voor het downloaden en installeren. 
- De gemiddelde tijd die nodig zijn voor een update downloaden en installeren die mag niet meer dan een paar uur.
- De prestaties van de virtuele machine en de netwerkbandbreedte.

Q. **Hoe biedt patch orchestration app besluit welke updates zijn beveiligingsupdates.**

A. Patch orchestration app gebruikmaakt van distro-specifieke logica om te bepalen welke updates tussen de beschikbare updates beveiligingsupdates. Bijvoorbeeld: In de app zoekt naar updates van archieven $RELEASE ubuntu-beveiliging, $RELEASE-updates ($RELEASE = xenial of de linux-versie standaard base release). 

 
Q. **Hoe kan ik vergrendelen doorsturen naar een specifieke versie van pakket?**

A. Gebruik de instellingen ApprovedPatches uw pakketten op een bepaalde versie vergrendelen. 


Q. **Wat gebeurt er met automatische updates is ingeschakeld in Ubuntu?**

A. Zodra u patch orchestration app op uw cluster installeren, zou zonder toezicht upgrades op uw clusterknooppunt worden uitgeschakeld. Alle periodieke update-werkstroom zou worden aangestuurd door patch orchestration app.
Als u wilt dat de consistentie van de omgeving hele cluster, wordt u aangeraden de updates via alleen patch orchestration-app te installeren. 
 
Q. **Upgrade post orchestration app wilt opruimen van niet-gebruikte pakketten van de patch?**

A. Ja, opschonen wordt uitgevoerd als onderdeel van de stappen na de installatie. 

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="a-node-is-not-coming-back-to-up-state"></a>Een knooppunt afkomstig niet is back-up maken van status

**Het knooppunt kan blijven steken in een status uitschakelen omdat**:

Er is een veiligheidscontrole in behandeling. U lost deze situatie, zorg ervoor dat er voldoende knooppunten beschikbaar in een foutloze toestand bevindt zijn.

**Het knooppunt kan blijven steken uitgeschakeld omdat**:

- Het knooppunt is handmatig uitgeschakeld.
- Het knooppunt is uitgeschakeld vanwege een lopende Azure-infrastructuur.
- Het knooppunt is tijdelijk uitgeschakeld door de app patch orchestration patch van het knooppunt.

**Het knooppunt kan blijven steken in een status Omlaag omdat**:

- Het knooppunt is geplaatst in een status Omlaag handmatig.
- Het knooppunt is momenteel door een herstart (die kan worden geactiveerd door de patch orchestration app).
- Het knooppunt is niet beschikbaar vanwege een defecte VM of verbindingsproblemen machine of het netwerk.

### <a name="updates-were-skipped-on-some-nodes"></a>Updates zijn op sommige knooppunten overgeslagen

De patch orchestration app probeert te installeren van een update op basis van het nieuwe beleid. De service probeert te herstellen van het knooppunt en overslaan van de update volgens het toepassingenbeleid.

In dat geval wordt een waarschuwing het niveau statusrapport gegenereerd op basis van de Agent-Service van het knooppunt. Het resultaat voor de update bevat ook de mogelijke reden voor de fout.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a>De status van het cluster overschakelt naar de fout tijdens de update is geïnstalleerd

Een defecte update kunt u de status van een toepassing of het cluster op een bepaald knooppunt of upgradedomein zetten. De patch orchestration app stoppen alle daaropvolgende updatebewerkingen totdat het cluster weer in orde is.

Een beheerder moet ingrijpen en bepalen waarom de toepassing of het cluster beschadigd vanwege een eerder geïnstalleerde update is.

## <a name="disclaimer"></a>Vrijwaring

De patch orchestration app verzamelt telemetrie om informatie over het gebruik en prestaties te houden. De toepassing telemetrie volgt de instelling van de Service Fabric-runtime telemetrie (die standaard is ingeschakeld).

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="version-010"></a>Versie 0.1.0
- Private preview-versie

### <a name="version-200-latest"></a>Versie 2.0.0 (laatste)
- Openbare versie
