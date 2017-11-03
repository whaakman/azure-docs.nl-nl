---
title: Automatisch schalen HPC Pack clusterknooppunten | Microsoft Docs
description: Automatisch vergroten of verkleinen het aantal HPC Pack cluster-rekenknooppunten in Azure
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: 
editor: tysonn
ms.assetid: 38762cd1-f917-464c-ae5d-b02b1eb21e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/08/2016
ms.author: danlep
ms.openlocfilehash: 0c8a5aacd19d83b26cfeb3750d57dd783687f1c4
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Automatisch vergroten of verkleinen van de clusterbronnen HPC Pack in Azure volgens de clusterwerkbelasting worden opgeslagen
Als u Azure 'burst' knooppunten in het cluster HPC Pack implementeert, of u een cluster HPC Pack in Azure VM's maakt, kunt u een manier om automatisch vergroten of verkleinen van de clusterresources zoals knooppunten of kernen volgens de werkbelasting op het cluster. Schalen van de clusterresources op deze manier kunt u efficiënter gebruik van uw Azure-resources en hun kosten te verifiëren.

Dit artikel ziet u twee manieren waarop HPC Pack biedt te schalen bronnen berekenen:

* De eigenschap van de cluster HPC Pack **AutoGrowShrink**

* De **AzureAutoGrowShrink.ps1** HPC PowerShell-script

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Momenteel kunt u alleen automatisch vergroten of verkleinen van HPC Pack rekenknooppunten waarop een Windows-serverbesturingssysteem.


## <a name="set-the-autogrowshrink-cluster-property"></a>Stel de eigenschap AutoGrowShrink-cluster
### <a name="prerequisites"></a>Vereisten

* **HPC Pack 2012 R2 Update 2 of hoger cluster** -het hoofdknooppunt van het cluster kan worden geïmplementeerd on-premises of in een Azure VM. Zie [een hybride-cluster met HPC Pack instellen](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) aan de slag met het hoofdknooppunt van een lokale en Azure 'burst' knooppunten. Zie de [HPC Pack IaaS-implementatiescript](hpcpack-cluster-powershell-script.md) snel implementeren een HPC Pack-cluster in Azure VM's.

* **Voor een cluster met een hoofdknooppunt in Azure (Resource Manager-implementatiemodel)** - vanaf HPC Pack 2016, certificaatverificatie in een Azure Active Directory-toepassing wordt gebruikt voor het automatisch groeiende en comprimeren cluster virtuele machines geïmplementeerd met Azure Resource Manager. Een certificaat als volgt configureren:

  1. Na implementatie van het cluster, verbinding maken met extern bureaublad met één hoofdknooppunt.

  2. Upload het certificaat (PFX-indeling met persoonlijke sleutel) naar elke hoofdknooppunt en Cert: \LocalMachine\My en Cert: \LocalMachine\Root installeren.

  3. Open Azure PowerShell als beheerder en voer de volgende opdrachten op één hoofdknooppunt:

    ```powershell
        cd $env:CCP_HOME\bin

        Login-AzureRmAccount
    ```
        
    Als uw account meer dan één Azure Active Directory-tenant of Azure-abonnement is, kunt u de volgende opdracht de juiste tenant en het abonnement selecteren uitvoeren:
  
    ```powershell
        Login-AzureRMAccount -TenantId <TenantId> -SubscriptionId <subscriptionId>
    ```     
       
    Voer de volgende opdracht om de geselecteerde tenant en het abonnement weer te geven:
    
    ```powershell
        Get-AzureRMContext
    ```

  4. Voer het volgende script

    ```powershell
        .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -CertificateThumbprint "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" -TenantId xxxxxxxx-xxxxx-xxxxx-xxxxx-xxxxxxxxxxxx
    ```

    waar

    **DisplayName** -weergavenaam van de Azure Active-toepassing. Als de toepassing niet bestaat, wordt deze gemaakt in Azure Active Directory.

    **Startpagina** -de startpagina van de toepassing. U kunt een dummy-URL, zoals in het voorgaande voorbeeld configureren.

    **IdentifierUri** -id van de toepassing. U kunt een dummy-URL, zoals in het voorgaande voorbeeld configureren.

    **CertificateThumbprint** -vingerafdruk van het certificaat dat u op het hoofdknooppunt in stap 1 hebt geïnstalleerd.

    **TenantId** -Tenant-ID van uw Azure Active Directory. Kunt u de Tenant-ID ophalen van de Azure Active Directory-beheerportal **eigenschappen** pagina.

    Voor meer informatie over **ConfigARMAutoGrowShrinkCert.ps1**, voert `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.


* **Voor een cluster met een hoofdknooppunt in Azure (klassieke implementatiemodel)** : als u het implementatiescript HPC Pack IaaS gebruiken om te maken van het cluster in het klassieke implementatiemodel, schakelt u de **AutoGrowShrink** eigenschap door de optie AutoGrowShrink in het configuratiebestand van de cluster-cluster. Zie voor meer informatie de documentatie bij de [script downloaden](https://www.microsoft.com/download/details.aspx?id=44949).

    U kunt ook inschakelen de **AutoGrowShrink** eigenschap cluster nadat u het cluster implementeren met behulp van HPC PowerShell-opdrachten in de volgende sectie beschreven. Als u wilt voorbereiden voor deze, moet u eerst de volgende stappen uitvoeren:

  1. Een Azure-beheercertificaat configureren op het hoofdknooppunt en in het Azure-abonnement. U kunt voor een proef-implementatie, gebruiken de standaard Microsoft HPC Azure zelf-ondertekend certificaat die HPC Pack op het hoofdknooppunt installeert en vervolgens dat certificaat uploaden naar uw Azure-abonnement. Zie voor opties en werk de [TechNet-bibliotheek richtlijnen](https://technet.microsoft.com/library/gg481759.aspx).

  2. Voer **regedit** op het hoofdknooppunt gaat u naar HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo en voeg een string-waarde. De naam van de waarde voor 'Vingerafdruk', en de waarde van gegevens naar de vingerafdruk van het certificaat in stap 1.

### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>Stel de eigenschap AutoGrowShrink HPC PowerShell-opdrachten
Hieronder vindt u voorbeeld HPC PowerShell-opdrachten om in te stellen **AutoGrowShrink** en af te stemmen met extra parameters in het gedrag ervan. Zie [AutoGrowShrink parameters](#AutoGrowShrink-parameters) verderop in dit artikel voor de volledige lijst met instellingen.

Als u wilt deze opdrachten uitgevoerd, start u HPC PowerShell hoofdknooppunt van het cluster als beheerder.

**De eigenschap AutoGrowShrink inschakelen**

```powershell
Set-HpcClusterProperty –EnableGrowShrink 1
```

**De eigenschap AutoGrowShrink uitschakelen**

```powershell
Set-HpcClusterProperty –EnableGrowShrink 0
```

**Voor het wijzigen van het formaat interval in minuten**

```powershell
Set-HpcClusterProperty –GrowInterval <interval>
```

**Voor het wijzigen van het verkleinen interval in minuten**

```powershell
Set-HpcClusterProperty –ShrinkInterval <interval>
```

**De huidige configuratie van AutoGrowShrink weergeven**

```powershell
Get-HpcClusterProperty –AutoGrowShrink
```

**Knooppuntgroepen uitsluiten van AutoGrowShrink**

```powershell
Set-HpcClusterProperty –ExcludeNodeGroups <group1,group2,group3>
```

>[!NOTE] 
> Deze parameter wordt ondersteund vanaf HPC Pack 2016
>

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink parameters
Hieronder vindt u AutoGrowShrink parameters die u aanpassen met behulp van kunt de **Set HpcClusterProperty** opdracht.

* **EnableGrowShrink** -Switch-of uitschakelen de **AutoGrowShrink** eigenschap.
* **ParamSweepTasksPerCore** -aantal taken dat parametrische één kern groeien. De standaardwaarde is één kern per taak groeien.

  > [!NOTE]
  > HPC Pack QFE KB3134307 wijzigingen **ParamSweepTasksPerCore** naar **TasksPerResourceUnit**. Deze is gebaseerd op het resourcetype en kan dit knooppunt, socket of core.
  >
  >
* **GrowThreshold** -drempelwaarde van taken in de wachtrij voor het activeren van automatische groei. De standaardwaarde is 1, wat dat betekent als er 1 of meer taken in de wachtrij staat, worden automatisch vergroot knooppunten.
* **GrowInterval** -Interval in minuten voor het activeren van automatische groei. Het standaardinterval is 5 minuten.
* **ShrinkInterval** -Interval in minuten voor het activeren van automatische verkleinen. Het interval is standaard 5 minuten. |
* **ShrinkIdleTimes** -continue controles verkleinen om aan te geven van de knooppunten niet actief zijn. De standaardwaarde is 3 maal. Bijvoorbeeld, als de **ShrinkInterval** is 5 minuten, HPC Pack controleert of het knooppunt niet actief is om de 5 minuten. Als de knooppunten zich in de niet-actieve status nadat 3 continue gecontroleerd (15 minuten), verkleint HPC Pack dat knooppunt.
* **ExtraNodesGrowRatio** -extra percentage van de knooppunten voor Message Passing Interface (MPI) taken groeien. De standaardwaarde is 1, wat betekent dat HPC Pack knooppunten 1% voor MPI-taken groeit.
* **GrowByMin** -Switch om aan te geven of het beleid autogrow is gebaseerd op de minimaal benodigde resources voor de taak. De standaardwaarde is ONWAAR, wat betekent dat HPC Pack knooppunten voor taken die op basis van het maximum aantal resources die vereist zijn voor de taken groeit.
* **SoaJobGrowThreshold** -drempelwaarde van inkomende SOA-aanvragen voor het activeren van het automatisch vergroten proces. De standaardwaarde is 50000.

  > [!NOTE]
  > Deze parameter wordt ondersteund vanaf HPC Pack 2012 R2 Update 3.
  >
  >
* **SoaRequestsPerCore** -aantal binnenkomende SOA-aanvragen voor uitbreiding van één kern. De standaardwaarde is 20000.

  > [!NOTE]
  > Deze parameter wordt ondersteund vanaf HPC Pack 2012 R2 Update 3.
  >
* **ExcludeNodeGroups** – knooppunten in het opgegeven knooppuntgroepen niet automatisch vergroten of verkleinen.
  
  > [!NOTE]
  > Deze parameter wordt ondersteund vanaf HPC Pack 2016.
  >

### <a name="mpi-example"></a>MPI-voorbeeld
Standaard HPC Pack 1% groeit extra knooppunten voor MPI-jobs (**ExtraNodesGrowRatio** is ingesteld op 1). De reden is dat MPI mogelijk meerdere knooppunten en de taak kan alleen worden uitgevoerd als alle knooppunten klaar bent. Wanneer Azure knooppunten start, soms één knooppunt mogelijk meer tijd nodig dan andere dat andere knooppunten actief wordt tijdens het wachten op dat knooppunt aan het gebruiksklaar starten. Extra knooppunten groeiende HPC Pack vermindert de wachttijd voor deze resource en bespaart u mogelijk kosten. Voor een verhoging van het percentage van extra knooppunten voor MPI-taken (bijvoorbeeld tot 10%), een vergelijkbaar met opdracht uitvoeren

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>SOA-voorbeeld
Standaard **SoaJobGrowThreshold** is ingesteld op 50000 en **SoaRequestsPerCore** is ingesteld op 20000. Als u één SOA-taak met 70000 aanvragen indient, er is een taak die in de wachtrij en inkomende aanvragen zijn 70000. In dit geval HPC Pack 1 core voor de taak in de wachtrij, en voor binnenkomende aanvragen groeit, groeit (70000 50000) / 20000 = 1 core daarom in totaal 2 kernen voor deze taak SOA groeit.

## <a name="run-the-azureautogrowshrinkps1-script"></a>Voer het script AzureAutoGrowShrink.ps1
### <a name="prerequisites"></a>Vereisten

* **HPC Pack 2012 R2 Update 1 of hoger cluster** : de **AzureAutoGrowShrink.ps1** script in de map % CCP_HOME % bin is geïnstalleerd. Het hoofdknooppunt van het cluster kan worden geïmplementeerd on-premises of in een Azure VM. Zie [een hybride-cluster met HPC Pack instellen](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) aan de slag met het hoofdknooppunt van een lokale en Azure 'burst' knooppunten. Zie de [HPC Pack IaaS-implementatiescript](hpcpack-cluster-powershell-script.md) om snel een HPC Pack cluster in Azure VM's implementeren, of gebruik een [snelstartsjabloon met de Azure](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).
* **Azure PowerShell 1.4.0** -het script op dat moment is afhankelijk van deze specifieke versie van Azure PowerShell.
* **Voor een cluster met Azure burst knooppunten** -het script uitvoert op een clientcomputer waarop HPC Pack is geïnstalleerd of op het hoofdknooppunt. Als uitgevoerd op een clientcomputer, controleert u of u de variabele $env ingesteld: CCP_SCHEDULER om te verwijzen naar het hoofdknooppunt. De Azure 'burst'-knooppunten moeten worden toegevoegd aan het cluster, maar kunnen ze zich in de status niet geïmplementeerd.
* **Voor een cluster dat is geïmplementeerd in Azure Virtual machines (Resource Manager-implementatiemodel)** -voor een cluster met virtuele Azure-machines geïmplementeerd in het Resource Manager-implementatiemodel, het script ondersteunt twee methoden voor verificatie op Azure: het script uitvoeren elke keer aanmelden bij uw Azure-account (door het uitvoeren van `Login-AzureRmAccount`, of een service-principal voor verificatie met een certificaat configureren. HPC Pack levert het script **ConfigARMAutoGrowShrinkCert.ps** een service-principal maken met certificaat. Het script wordt een Azure Active Directory (Azure AD)-toepassing en een service-principal gemaakt en wordt de rol Inzender toegewezen aan de service-principal. Het script wordt uitgevoerd, start u Azure PowerShell als administrator en voer de volgende opdrachten:

    ```powershell
    cd $env:CCP_HOME\bin

    Login-AzureRmAccount

    .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -PfxFile "d:\yourcertificate.pfx"
    ```

    Voor meer informatie over **ConfigARMAutoGrowShrinkCert.ps1**, voert `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,

* **Voor een cluster dat is geïmplementeerd in Azure Virtual machines (klassieke implementatiemodel)** -het script uitvoeren op het hoofdknooppunt VM, omdat deze afhankelijk van is de **Start HpcIaaSNode.ps1** en **Stop HpcIaaSNode.ps1** scripts die zijn geïnstalleerd. Deze scripts Bovendien vereisen een Azure-beheercertificaat of bestand publicatie-instellingen (Zie [rekenknooppunten beheren in een Pack HPC-cluster in Azure](hpcpack-cluster-node-manage.md)). Controleer of alle het rekenknooppunt u moet virtuele machines zijn al toegevoegd aan het cluster. Ze mogelijk de status gestopt.



### <a name="syntax"></a>Syntaxis
```powershell
AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfActiveQueuedTasksPerNodeToGrow <Single> [-NumOfActiveQueuedTasksToGrowThreshold <Int32>]
    [-NumOfInitialNodesToGrow <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>]
    [-ShrinkCheckIdleTimes <Int32>] [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>]
    [<CommonParameters>]

AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfQueuedJobsPerNodeToGrow <Single> [-NumOfQueuedJobsToGrowThreshold <Int32>] [-NumOfInitialNodesToGrow
    <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>] [-ShrinkCheckIdleTimes <Int32>]
    [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]

AzureAutoGrowShrink.ps1 -UseLastConfigurations [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]
```
### <a name="parameters"></a>Parameters
* **NodeTemplates** -namen van het knooppunt sjablonen voor het definiëren van het bereik voor de knooppunten om te vergroten of verkleinen. Als niet wordt opgegeven (de standaardwaarde is @()), alle knooppunten in de **AzureNodes** knooppunt groep zijn wanneer in het bereik **NodeType** een waarde heeft van AzureNodes en alle knooppunten in de **ComputeNodes** knooppunt groep zijn wanneer het in bereik **NodeType** een waarde heeft van ComputeNodes.
* **JobTemplates** -namen van de taaksjablonen voor het definiëren van het bereik voor de knooppunten groeien.
* **NodeType** -het type knooppunt te vergroten of verkleinen. Ondersteunde waarden zijn:

  * **AzureNodes** : voor Azure PaaS (burst) knooppunten in een on-premises of Azure IaaS-cluster.
  * **ComputeNodes** : voor het rekenknooppunt VM's alleen in een Azure IaaS-cluster.

* **NumOfQueuedJobsPerNodeToGrow** -aantal taken in de wachtrij vereist voor uitbreiding van één knooppunt.
* **NumOfQueuedJobsToGrowThreshold** -de drempelwaarde voor aantal wachtrijtaken om het proces van het formaat te starten.
* **NumOfActiveQueuedTasksPerNodeToGrow** - het getal van actief in de wachtrij vereiste taken voor het vergroten van één knooppunt. Als **NumOfQueuedJobsPerNodeToGrow** wordt opgegeven met een waarde groter dan 0, worden deze parameter wordt genegeerd.
* **NumOfActiveQueuedTasksToGrowThreshold** -de drempelwaarde voor aantal actieve taken in de wachtrij om de groei-proces te starten.
* **NumOfInitialNodesToGrow** -de initiële minimum aantal knooppunten toe als alle knooppunten in het bereik zijn **niet geïmplementeerd** of **gestopt (Deallocated)**.
* **GrowCheckIntervalMins** -het interval in minuten tussen controles groeien.
* **ShrinkCheckIntervalMins** -het interval in minuten tussen controles verkleinen.
* **ShrinkCheckIdleTimes** -het aantal continue verkleinen controles (gescheiden door **ShrinkCheckIntervalMins**) om aan te geven van de knooppunten niet actief zijn.
* **UseLastConfigurations** -vorige configuraties in het argument-bestand opgeslagen.
* **ArgFile**-de naam van het argument-bestand gebruikt voor het opslaan en bijwerken van de configuraties als het script wilt uitvoeren.
* **LogFilePrefix** -voorvoegsel van de naam van het logboekbestand. U kunt een pad opgeven. Standaard wordt het logboek geschreven naar de huidige werkmap.

### <a name="example-1"></a>Voorbeeld 1
Het volgende voorbeeld configureert u de Azure burst knooppunten met de standaardsjabloon AzureNode te vergroten of verkleinen automatisch geïmplementeerd. Als alle knooppunten in eerste instantie in de **niet geïmplementeerd** staat, ten minste 3 knooppunten worden gestart. Als het aantal taken in de wachtrij groter is dan 8, knooppunten in het script wordt gestart totdat hun aantal is groter dan de ratio van opdrachten in de wachtrij **NumOfQueuedJobsPerNodeToGrow**. Als een knooppunt wordt gevonden 3 opeenvolgende niet-actieve tijd actief is, wordt het gestopt.

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Voorbeeld 2
Het volgende voorbeeld wordt het rekenknooppunt Azure virtuele machines die worden geïmplementeerd met de standaardsjabloon ComputeNode te vergroten of verkleinen automatisch geconfigureerd.
De taken die zijn geconfigureerd door de standaardsjabloon voor de taak wordt het bereik van de werkbelasting definiëren op het cluster. Als alle knooppunten in eerste instantie zijn gestopt, worden ten minste 5 knooppunten gestart. Als het aantal actieve taken in de wachtrij groter is dan 15, knooppunten in het script wordt gestart totdat hun aantal is groter dan de ratio van actieve taken in de wachtrij voor **NumOfActiveQueuedTasksPerNodeToGrow**. Als een knooppunt wordt gevonden in 10 opeenvolgende niet-actieve keer actief is, wordt het gestopt.

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
