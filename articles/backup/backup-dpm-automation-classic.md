---
title: 'Azure back-up: Gebruik PowerShell back-up DPM workloads | Microsoft Docs'
description: Meer informatie over het implementeren en beheren van Azure Backup voor Data Protection Manager (DPM) met behulp van PowerShell
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
ms.assetid: bcbcef79-9d33-4e84-a558-9866614f2cae
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;trinadhk;anuragm;markgal
ms.openlocfilehash: 943a12dcba49a114d206b9dab968da332ea99926
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a>Met behulp van PowerShell back-ups implementeren en beheren in Azure voor Data Protection Manager (DPM)-servers
> [!div class="op_single_selector"]
> * [ARM](backup-dpm-automation.md)
> * [Klassiek](backup-dpm-automation-classic.md)
>
>

In dit artikel wordt uitgelegd hoe PowerShell gebruiken voor back-up en DPM-gegevens herstellen vanaf een back-upkluis. Microsoft raadt u aan met behulp van de Recovery Services-kluizen voor alle nieuwe implementaties. Als u een nieuwe Azure Backup-gebruiker bent, gebruikt u het artikel [implementeren en beheren van Data Protection Manager-gegevens naar Azure met behulp van PowerShell](backup-dpm-automation.md), zodat u uw gegevens in een Recovery Services-kluis opslaat.

> [!IMPORTANT]
> U kunt uw back-upkluizen nu upgraden naar Recovery Services-kluizen. Zie voor meer informatie het artikel [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Een back-upkluis upgraden naar een Recovery Services-kluis). Microsoft adviseert om uw back-upkluizen te upgraden naar Recovery Services-kluizen. Na 15 oktober 2017 kunt u PowerShell niet meer gebruiken voor het maken van back-upkluizen. **Per 1 november 2017**:
>- Alle resterende back-upkluizen worden automatisch omgezet in Recovery Services-kluizen.
>- Het is niet mogelijk om via de klassieke portal toegang te krijgen tot uw back-upgegevens. In plaats daarvan gebruikt u Azure Portal voor toegang tot uw back-upgegevens in Recovery Services-kluizen.
>

## <a name="setting-up-the-powershell-environment"></a>De PowerShell-omgeving instellen
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Voordat u PowerShell gebruiken kunt voor het beheren van back-ups van Data Protection Manager naar Azure, moet u de juiste omgeving hebben in PowerShell. Zorg ervoor dat u de volgende opdracht voor het importeren van de juiste modules en kunt u correct verwijzen naar de DPM-cmdlets uitvoert aan het begin van de PowerShell-sessie:

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome to the DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a>De installatie en registratie
Om te beginnen met:

1. [Download de meest recente PowerShell](https://github.com/Azure/azure-powershell/releases) (minimaal vereiste versie is: 1.0.0)
2. De Azure Backup-commandlets inschakelen door het overschakelen naar *AzureResourceManager* modus met behulp van de **Switch AzureMode** commandlet:

```
PS C:\> Switch-AzureMode AzureResourceManager
```

De volgende taken voor de installatie en registratie kunnen worden geautomatiseerd met PowerShell:

* Een back-upkluis maken
* De Azure Backup agent installeren
* Registreren bij de Azure Backup-service
* Netwerkinstellingen
* Versleutelingsinstellingen

### <a name="create-a-backup-vault"></a>Een back-upkluis maken
> [!WARNING]
> Voor klanten met Azure Backup voor de eerste keer, moet u de Azure Backup-provider die moet worden gebruikt met uw abonnement te registreren. Dit kan worden gedaan door de volgende opdracht uit te voeren: Register AzureProvider - ProviderNamespace 'Microsoft.Backup'
>
>

U kunt maken met een nieuwe back-upkluis met de **nieuw AzureRMBackupVault** commandlet. De back-upkluis is een ARM-bron, dus u moet deze binnen een resourcegroep te plaatsen. Voer de volgende opdrachten in een verhoogde Azure PowerShell-console:

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

U krijgt een lijst met alle back-upkluizen in een bepaald abonnement met de **Get-AzureRMBackupVault** commandlet.

### <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a>De Azure Backup agent installeren op een DPM-Server
Voordat u de Azure Backup agent installeert, moet u het installatieprogramma gedownload en aanwezig is op de Windows Server hebben. U kunt de nieuwste versie van het installatieprogramma van de [Microsoft Download Center](http://aka.ms/azurebackup_agent) of van de pagina Dashboard van de back-upkluis. Het installatieprogramma opslaan op een toegankelijke locatie zoals * C:\Downloads\*.

Als u wilt de agent installeert, kunt u de volgende opdracht uitvoeren in een PowerShell-console met verhoogde bevoegdheid **op de DPM-server**:

```
PS C:\> MARSAgentInstaller.exe /q
```

Hiermee installeert u de agent met de standaardopties. De installatie duurt een paar minuten op de achtergrond. Als u niet geeft de */nu* optie de **Windows Update** venster wordt geopend aan het einde van de installatie om te controleren op updates.

De agent wordt weergegeven in de lijst met geïnstalleerde programma's. De lijst met geïnstalleerde programma's wilt bekijken, gaat u naar **Configuratiescherm** > **programma's** > **programma's en onderdelen**.

![Agent is geïnstalleerd](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a>Opties voor de installatie
Overzicht van alle opties die beschikbaar is via de opdrachtregel de volgende opdracht gebruiken:

```
PS C:\> MARSAgentInstaller.exe /?
```

De beschikbare opties zijn onder andere:

| Optie | Details | Standaard |
| --- | --- | --- |
| /q |Stille installatie |- |
| / p: 'locatie' |Pad naar de installatiemap voor de Azure Backup agent. |C:\Program Files\Microsoft Azure Recovery Services-Agent |
| / s: 'locatie' |Pad naar de cachemap voor de Azure Backup agent. |C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch |
| /m |Aanmelden voor Microsoft Update |- |
| /nu |Niet controleren op updates nadat de installatie is voltooid |- |
| /d |Hiermee verwijdert u Microsoft Azure Recovery Services-Agent |- |
| /pH |Host-proxyadres |- |
| /PO |Proxy-Host-poortnummer |- |
| /Pu |De Proxygebruikersnaam voor de Host |- |
| /PW |Proxy-wachtwoord |- |

### <a name="registering-with-the-azure-backup-service"></a>Registreren bij de Azure Backup-service
Voordat u met de Azure Backup-service registreren kunt, moet u ervoor zorgen dat de [vereisten](backup-azure-dpm-introduction.md) wordt voldaan. U moet doen:

* Een geldige Azure-abonnement hebt
* Hebt u een back-upkluis

Uitvoeren als u wilt de kluisreferenties downloaden, de **Get-AzureBackupVaultCredentials** commandlet in een Azure PowerShell-console en opslaan in een handige locatie, zoals * C:\Downloads\*.

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

Registreren van de machine met de kluis wordt gedaan met behulp van de [Start DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) cmdlet:

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

Hiermee wordt de DPM-Server met de naam 'TestingServer' met Microsoft Azure-kluis met behulp van de referenties voor de opgegeven kluis registreren.

> [!IMPORTANT]
> Gebruik geen relatieve paden om op te geven het kluisreferentiebestand. U moet een absoluut pad opgeven als invoer voor de cmdlet.
>
>

### <a name="initial-configuration-settings"></a>Configuratie-instellingen
Zodra de DPM-Server is geregistreerd bij de Azure Backup-kluis, wordt deze gestart met de standaardinstellingen van het abonnement. Deze abonnementsinstellingen omvatten netwerken, versleuteling en de tijdelijke ruimte. Om te beginnen met het wijzigen van de instellingen van het abonnement moet u eerst een greep op de bestaande (standaard)-instellingen met behulp van de [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

Alle wijzigingen worden aangebracht op deze lokale PowerShell-object ```$setting``` en vervolgens het volledige object is doorgevoerd in DPM en Azure Backup deze op te slaan met behulp van de [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet. U moet gebruiken de ```–Commit``` vlag om ervoor te zorgen dat de wijzigingen worden doorgevoerd. De instellingen worden niet toegepast en gebruikt door Azure Backup tenzij doorgevoerd.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a>Netwerken
Als de connectiviteit van de DPM-machine met de Azure Backup-service op het internet via een proxyserver bevinden, moeten de proxy-instellingen worden opgegeven voor back-ups te kunnen uitvoeren. Dit wordt gedaan met behulp van de ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` en de ```ProxyPassword``` parameters met de [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet. In dit voorbeeld is het geen proxyserver zodat we alle informatie met betrekking tot de proxy expliciet wordt gewist.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

Bandbreedtegebruik kan ook worden beheerd met opties ```-WorkHourBandwidth``` en ```-NonWorkHourBandwidth``` voor een bepaald aantal dagen van de week. In dit voorbeeld stelt we niet beperking.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-the-staging-area"></a>De tijdelijke ruimte configureren
De Azure Backup-agent op de DPM-server moet een tijdelijke opslag voor gegevens die worden hersteld vanuit de cloud (lokaal faseringsgebied). Configureren het faseringsgebied met behulp van de [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet en de ```-StagingAreaPath``` parameter.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

In het bovenstaande voorbeeld wordt de tijdelijke ruimte worden ingesteld op *C:\StagingArea* in het PowerShell-object ```$setting```. Zorg ervoor dat de opgegeven map bestaat al, anders wordt het laatste doorvoeren van de abonnementsinstellingen zal mislukken.

### <a name="encryption-settings"></a>Versleutelingsinstellingen
De back-upgegevens verzonden naar Azure Backup is ter bescherming van de vertrouwelijkheid van de gegevens versleuteld. De wachtwoordzin voor versleuteling is het 'wachtwoord' voor het ontsleutelen van de gegevens op het moment van terugzetten. Het is belangrijk dat deze gegevens veilig en beveiligd wanneer deze is ingesteld.

In het volgende voorbeeld wordt de eerste opdracht converteert de ```passphrase123456789``` naar een veilige tekenreeks en wijst de veilige tekenreeks aan de variabele met de naam ```$Passphrase```. de tweede opdracht stelt u de beveiligde tekenreeks in ```$Passphrase``` als het wachtwoord voor het versleutelen van back-ups.

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> De wachtwoordzin gegevens veilig en beveiligd houden als deze eenmaal is ingesteld. Niet mogelijk om gegevens te herstellen van Azure zonder deze wachtwoordzin.
>
>

Op dit moment moet aangebracht alle vereiste wijzigingen aan de ```$setting``` object. Vergeet niet de wijzigingen doorvoeren.

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a>Beveiligen van gegevens naar Azure Backup
In deze sectie maakt u een productieserver toevoegen aan DPM en Beveilig vervolgens de gegevens naar de lokale DPM-opslag en klik vervolgens op Azure Backup. In de voorbeelden wordt gedemonstreerd hoe u back-up van bestanden en mappen. De logica kan gemakkelijk worden uitgebreid voor back-up van een DPM-ondersteunde gegevensbron. Alle DPM--ups worden geregeld door een Protection Group (PG) met vier onderdelen:

1. **Leden** is een lijst met alle beveiligbare objecten (ook wel bekend als *gegevensbronnen* in DPM) die u wilt beveiligen in dezelfde beveiligingsgroep. U wilt bijvoorbeeld productie virtuele machines in een beveiligingsgroep en SQL Server-databases in een andere beveiligingsgroep niet beveiligen, omdat er verschillende vereisten voor back-up. Voordat u kunt back-up elke gegevensbron op een productieserver moet u ervoor zorgen dat wordt de DPM-Agent op de server is geïnstalleerd en wordt beheerd door DPM. Volg de stappen voor [de DPM-Agent installeren](https://technet.microsoft.com/library/bb870935.aspx) en koppelt aan de juiste DPM-Server.
2. **Methode voor gegevensbeveiliging** Hiermee geeft u de back-up doellocaties - tape, schijf- en cloud. In ons voorbeeld beveiligen we gegevens naar de lokale schijf en naar de cloud.
3. Een **back-upschema** die aangeeft wanneer back-ups moeten worden genomen en hoe vaak de gegevens moeten worden gesynchroniseerd tussen de DPM-Server en de productieserver.
4. Een **bewaarschema** die aangeeft hoe lang voor het bewaren van de herstelpunten die in Azure.

### <a name="creating-a-protection-group"></a>Een beveiligingsgroep maken
Eerst maakt u een nieuwe beveiligingsgroep met de [nieuw DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

De bovenstaande cmdlet maakt een beveiligingsgroep met de naam *ProtectGroup01*. Een bestaande beveiligingsgroep kan ook later worden aangepast voor back-up toevoegen aan de Azure-cloud. Echter wijzigingen aanbrengen in de beveiligingsgroep - nieuwe of bestaande - we nodig hebt om een koppeling op een *bewerkbaar* object met de [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a>Groepsleden toe te voegen aan de beveiligingsgroep
Elke DPM-Agent kent de lijst van gegevensbronnen op de server waarop deze is geïnstalleerd. Een gegevensbron moet aan de beveiligingsgroep, de DPM-Agent eerst een lijst van de gegevensbronnen te verzenden naar de DPM-server. Een of meer gegevensbronnen zijn vervolgens geselecteerd en toegevoegd aan de beveiligingsgroep. De PowerShell-stappen die nodig zijn om op te halen bereiken hiervan zijn:

1. Haal een lijst met alle servers die worden beheerd door DPM via de DPM-Agent.
2. Kies een specifieke server.
3. Haal een lijst van alle gegevensbronnen op de server.
4. Kies een of meer gegevensbronnen en toe te voegen aan de beveiligingsgroep

De lijst met servers waarop de DPM-Agent is geïnstalleerd en wordt beheerd door de DPM-Server wordt verkregen met de [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet. In dit voorbeeld wordt filteren en alleen PS configureren met de naam *productionserver01* voor back-up.

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

Nu ophalen van de lijst van gegevensbronnen op ```$server``` met behulp van de [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet. In dit voorbeeld zijn er filteren voor het volume * D:\* die we willen configureren voor back-up. Deze gegevensbron wordt vervolgens toegevoegd aan de beveiligingsgroep met de [toevoegen DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet. Gebruik de *modifable* beveiliging groepsobject ```$MPG``` waarmee de toevoegingen.

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

Herhaal deze stap zo vaak als nodig is, totdat u de gekozen gegevensbronnen hebt toegevoegd aan de beveiligingsgroep toe. U kunt beginnen met slechts één gegevensbron en voltooien van de werkstroom voor het maken van de beveiligingsgroep en op een later tijdstip meer gegevensbronnen aan de beveiligingsgroep toevoegen.

### <a name="selecting-the-data-protection-method"></a>De methode voor gegevensbeveiliging selecteren
Zodra de gegevensbronnen zijn toegevoegd aan de beveiligingsgroep, de volgende stap is om op te geven van de beveiliging methode met behulp van de [Set DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet. In dit voorbeeld wordt de beveiligingsgroep worden ingesteld voor de lokale schijf en de cloud back-up. U moet ook opgeven van de gegevensbron die u beveiligen wilt naar de cloud met de [toevoegen DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet uit met - Online vlag.

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a>De bewaarperiode instellen
De bewaarperiode instelt voor de back-up verwijst met behulp van de [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet. Terwijl oneven de bewaarperiode instellen lijken voordat de back-upschema is gedefinieerd, met behulp van de ```Set-DPMPolicyObjective``` cmdlet stelt automatisch een standaard back-upschema die vervolgens kan worden gewijzigd. Het is altijd mogelijk om de back-up plannen eerst instellen en het bewaarbeleid na.

In het volgende voorbeeld wordt de cmdlet de retentie-parameters ingesteld voor back-ups van schijf. Dit, back-ups behouden voor 10 dagen en gegevens synchroniseren tussen de productieserver en de DPM-server om de 6 uur. De ```SynchronizationFrequencyMinutes``` bevat geen definitie van hoe vaak een back-up is gemaakt, maar hoe vaak gegevens worden gekopieerd naar de DPM-server; zo voorkomt u dat back-ups te groot.

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

Voor back-ups naar Azure (DPM verwijst naar deze poorten als Online back-ups) de bewaartermijnen kunnen worden geconfigureerd voor [langdurig bewaren volgens een schema opa-vader-zoon (algemene)](backup-azure-backup-cloud-as-tape.md). Dat wil zeggen, kunt u een gecombineerde bewaarbeleid met betrekking tot dagelijks, wekelijks, maandelijks en jaarlijks bewaarbeleid definiëren. In dit voorbeeld wordt een matrix die vertegenwoordigt de complexe bewaarschema die we willen maken en configureer vervolgens de bewaarperiode bereik met behulp de [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a>Stel de back-upschema
DPM een back-upschema standaard automatisch ingesteld als u beveiliging beoogd gebruik opgeeft de ```Set-DPMPolicyObjective``` cmdlet. U kunt wijzigen van de standaardplanningen met de [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet gevolgd door de [Set DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

In het bovenstaande voorbeeld ```$onlineSch``` is een matrix met vier elementen met de bestaande planning voor online beveiliging voor de beveiligingsgroep in het algemene schema:

1. ```$onlineSch[0]```bevat de dagelijkse planning
2. ```$onlineSch[1]```bevat de wekelijkse planning
3. ```$onlineSch[2]```bevat de maandelijks schema
4. ```$onlineSch[3]```bevat de jaarlijkse planning

Dus als u de wekelijkse planning wijzigen wilt, u moet om te verwijzen naar de ```$onlineSch[1]```.

### <a name="initial-backup"></a>Eerste back-up
Back-ups van een gegevensbron voor de eerste keer, moet DPM maken van een eerste replica die u maakt een kopie van de datasource die moet worden beveiligd op de DPM-replicavolume. Deze activiteit kunnen ofwel worden gepland voor een bepaald tijdstip of handmatig kan worden geactiveerd, met behulp van de [Set DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet met de parameter ```-NOW```.

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a>De grootte van de DPM-Replica en herstelpuntvolume wijzigen
U kunt ook de grootte van de DPM-replicavolume, evenals een gebruik van de volume Shadow Copy wijzigen [Set DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet zoals in de onderstaande voorbeeld: Get-DatasourceDiskAllocation - Datasource $DS $DS Set-DatasourceDiskAllocation - Datasource - ProtectionGroup $MPG-handmatige - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)

### <a name="committing-the-changes-to-the-protection-group"></a>De wijzigingen doorvoert aan de beveiligingsgroep
Tot slot moeten de wijzigingen worden doorgevoerd voordat DPM duren de back-up per de nieuwe configuratie van de beveiligingsgroep voordat kan. Dit wordt gedaan met behulp van de [Set DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-the-backup-points"></a>De back-uppunten weergeven
U kunt de [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet voor een lijst van alle herstelpunten voor een datasource. In dit voorbeeld wordt:

* alle PGs op de DPM-server die worden opgeslagen in een matrix ophalen```$PG```
* ophalen van de gegevensbronnen die overeenkomt met de```$PG[0]```
* alle herstelpunten voor een gegevensbron niet ophalen.

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a>Herstellen van gegevens die zijn beveiligd op Azure
Herstellen van gegevens is een combinatie van een ```RecoverableItem``` object en een ```RecoveryOption``` object. In de vorige sectie wij een lijst van de back-punten voor een datasource.

In het onderstaande voorbeeld ziet u hoe een Hyper-V virtuele machine van Azure Backup terugzetten back-uppunten combineren met het doel voor herstel. Dit omvat:

* Maken van een herstel optie met de [nieuw DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.
* Ophalen van de matrix van back-punten met behulp van de ```Get-DPMRecoveryPoint``` cmdlet.
* Een back-uppunt terugzetten vanaf kiezen.

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

De opdrachten kunnen gemakkelijk worden uitgebreid voor een gegevensbrontype.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over Azure Backup voor DPM [Inleiding tot DPM back-up](backup-azure-dpm-introduction.md)
