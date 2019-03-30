---
title: Azure Backup - PowerShell gebruiken om te back-up van DPM-workloads
description: Meer informatie over het implementeren en beheren van Azure Backup voor Data Protection Manager (DPM) met behulp van PowerShell
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 1/23/2017
ms.author: adigan
ms.openlocfilehash: b16963265c971e604f03b51fd63f7fe411bab36e
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58651837"
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a>Met behulp van PowerShell back-ups implementeren en beheren in Azure voor Data Protection Manager (DPM)-servers

In dit artikel leest u hoe u PowerShell gebruikt tot het instellen van Azure Backup op een DPM-server en voor het beheren van back-up en herstel.

## <a name="setting-up-the-powershell-environment"></a>Instellen van de PowerShell-omgeving

Voordat u PowerShell gebruiken kunt voor het beheren van back-ups van Data Protection Manager naar Azure, moet u de juiste omgeving hebt in PowerShell. Aan het begin van de PowerShell-sessie, zorg ervoor dat u de volgende opdracht voor het importeren van de juiste modules en kunt u goed verwijzen naar de DPM-cmdlets uitvoert:

```powershell
& "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"
```

```Output
Welcome to the DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a>Installatie en registratie

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Om te beginnen, [download de nieuwste Azure PowerShell](/powershell/azure/install-az-ps).

De volgende installatie en registratie van taken kunnen worden geautomatiseerd met PowerShell:

* Een Recovery Services-kluis maken
* De Azure backup-agent installeren
* Registreren bij de Azure Backup-service
* Netwerkinstellingen
* Versleutelingsinstellingen

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

De volgende stappen leiden u bij het maken van een Recovery Services-kluis. Een Recovery Services-kluis is anders dan een back-upkluis.

1. Als u Azure Backup voor de eerste keer gebruikt, moet u de **registreren AzResourceProvider** cmdlet voor het registreren van de Azure Recovery Service-provider met uw abonnement.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

2. De Recovery Services-kluis is een ARM-resource, dus u moet dit binnen een resourcegroep te plaatsen. U kunt een bestaande resourcegroep gebruiken, of een nieuwe maken. Bij het maken van een nieuwe resourcegroep, geef de naam en locatie voor de resourcegroep.

    ```powershell
    New-AzResourceGroup –Name "test-rg" –Location "West US"
    ```

3. Gebruik de **New-AzRecoveryServicesVault** cmdlet voor het maken van een nieuwe kluis. Zorg ervoor dat de dezelfde locatie voor de kluis opgeven dat werd gebruikt voor de resourcegroep.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```

4. Geef het type opslagredundantie moet gebruiken. u kunt [lokaal redundante opslag (LRS)](../storage/common/storage-redundancy-lrs.md) of [geografisch redundante opslag (GRS)](../storage/common/storage-redundancy-grs.md). Het volgende voorbeeld ziet dat de optie - BackupStorageRedundancy voor testVault is ingesteld op GeoRedundant.

   > [!TIP]
   > Voor veel Azure Backup-cmdlets is het object Recovery Services-kluis als invoer vereist. Daarom is het handiger het object Backup Recovery Services-kluis in een variabele op te slaan.
   >
   >

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault –Name "testVault"
    Set-AzRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>De kluizen in een abonnement weergeven

Gebruik **Get-AzRecoveryServicesVault** om de lijst met alle kluizen in het huidige abonnement weer te geven. U kunt deze opdracht gebruiken om te controleren of een nieuwe kluis is gemaakt, of om te zien welke kluizen zijn beschikbaar in het abonnement.

Voert u de opdracht Get-AzRecoveryServicesVault, en alle kluizen in het abonnement worden weergegeven.

```powershell
Get-AzRecoveryServicesVault
```

```Output
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a>De Azure backup-agent installeren op een DPM-Server

Voordat u de Azure backup-agent installeert, moet u het installatieprogramma gedownload en aanwezig zijn op de Windows-Server. Krijgt u de nieuwste versie van het installatieprogramma van de [Microsoft Download Center](https://aka.ms/azurebackup_agent) of van de dashboardpagina van de Recovery Services-kluis. Het installatieprogramma opslaan op een toegankelijke locatie, zoals * C:\Downloads\*.

Als u wilt de agent hebt geïnstalleerd, kunt u de volgende opdracht uitvoeren in een PowerShell-console met verhoogde bevoegdheid **op de DPM-server**:

```powershell
MARSAgentInstaller.exe /q
```

Hiermee wordt de agent geïnstalleerd met de standaardopties. De installatie duurt een paar minuten op de achtergrond. Als u geen opgeeft de */nu* optie de **Windows Update** venster wordt geopend aan het einde van de installatie om te controleren op updates.

De agent wordt weergegeven in de lijst met geïnstalleerde programma's. De lijst met geïnstalleerde programma's wilt bekijken, gaat u naar **Configuratiescherm** > **programma's** > **programma's en onderdelen**.

![De agent is geïnstalleerd](./media/backup-dpm-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Opties voor de installatie

Als u wilt zien van alle opties die beschikbaar zijn via de opdrachtregel, gebruikt u de volgende opdracht uit:

```powershell
MARSAgentInstaller.exe /?
```

De beschikbare opties zijn onder andere:

| Optie | Details | Standaard |
| --- | --- | --- |
| /q |Stille installatie |- |
| / p: "locatie" |Pad naar de installatiemap voor de Azure backup-agent. |C:\Program Files\Microsoft Azure Recovery Services Agent |
| / s: "locatie" |Pad naar de cachemap voor de Azure backup-agent. |C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch |
| /m |Aanmelden voor Microsoft Update |- |
| /nu |Niet controleren op updates nadat de installatie is voltooid |- |
| /d |Hiermee verwijdert u Microsoft Azure Recovery Services-Agent |- |
| /pH |Proxyadres van Host |- |
| /po |Proxy-poortnummer voor Host |- |
| /pu |Gebruikersnaam voor proxy-Host |- |
| /pw |Wachtwoord voor proxy |- |

## <a name="registering-dpm-to-a-recovery-services-vault"></a>Registratie van DPM naar een Recovery Services-kluis

Nadat u de Recovery Services-kluis gemaakt, de meest recente agent en de kluisreferenties downloaden en opslaan in een handige locatie zoals C:\Downloads.

```powershell
$credspath = "C:\downloads"
$credsfilename = Get-AzRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
$credsfilename
```

```Output
C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

Voer op de DPM-server de [Start OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet voor het registreren van de machine bij de kluis.

```powershell
$cred = $credspath + $credsfilename
Start-OBRegistration-VaultCredentials $cred -Confirm:$false
```

```Output
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

### <a name="initial-configuration-settings"></a>Initiële configuratie-instellingen

Zodra de DPM-Server is geregistreerd bij de Recovery Services-kluis, begint deze met de standaardinstellingen van het abonnement. Deze instellingen omvatten netwerken, versleuteling en het faseringsgebied. Abonnement wijzigen moet u eerst een ingang krijgen op de bestaande (standaard)-instellingen met behulp van de [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:

```powershell
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

Alle wijzigingen zijn aangebracht aan deze lokale PowerShell-object ```$setting``` en vervolgens het volledige object is doorgevoerd naar DPM en Azure Backup op te slaan ze met behulp van de [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet. U moet gebruiken de ```–Commit``` vlag om ervoor te zorgen dat de wijzigingen worden doorgevoerd. De instellingen worden niet toegepast en gebruikt door Azure Backup, tenzij doorgevoerd.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="networking"></a>Netwerken

Als de verbinding van de DPM-machine naar de Azure Backup-service op het internet via een proxyserver, moeten klikt u vervolgens de instellingen van de proxyserver worden opgegeven voor geslaagde back-ups. Dit wordt gedaan met behulp van de ```-ProxyServer```en ```-ProxyPort```, ```-ProxyUsername``` en de ```ProxyPassword``` parameters met de [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet. In dit voorbeeld is er geen proxyserver, zodat we een proxy-gerelateerde informatie expliciet wordt gewist.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

Gebruik van netwerkbandbreedte kan ook worden beheerd met de opties van ```-WorkHourBandwidth``` en ```-NonWorkHourBandwidth``` voor een bepaald aantal dagen van de week. In dit voorbeeld zijn we niet instellen van een beperking.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

## <a name="configuring-the-staging-area"></a>Het faseringsgebied configureren

De Azure Backup-agent die wordt uitgevoerd op de DPM-server, heeft tijdelijke opslag voor gegevens die zijn hersteld vanuit de cloud (lokaal faseringsgebied). Configureren van de tijdelijke ruimte met behulp van de [Set DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet en de ```-StagingAreaPath``` parameter.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

In het bovenstaande voorbeeld wordt het faseringsgebied worden ingesteld op *C:\StagingArea* in de PowerShell-object ```$setting```. Zorg ervoor dat de opgegeven map bestaat al, anders mislukt de laatste doorvoering van de instellingen van het abonnement.

### <a name="encryption-settings"></a>Versleutelingsinstellingen

De back-upgegevens verzonden naar Azure Backup zijn versleuteld, zodat de vertrouwelijkheid van de gegevens. De wachtwoordzin voor versleuteling is het "wachtwoord" voor het ontsleutelen van de gegevens op het moment van herstel. Het is belangrijk dat u deze gegevens veilig en beveiligd wanneer deze optie is ingesteld.

In het volgende voorbeeld wordt de eerste opdracht converteert de tekenreeks ```passphrase123456789``` naar een beveiligde tekenreeks en wijst de veilige tekenreeks die moet de variabele met de naam ```$Passphrase```. de tweede opdracht stelt u de beveiligde tekenreeks in ```$Passphrase``` als het wachtwoord voor het versleutelen van back-ups.

```powershell
$Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> De wachtwoordzin gegevens veilig houden zodra deze is ingesteld. Niet mogelijk om gegevens te herstellen van Azure zonder dat deze wachtwoordzin.
>
>

Op dit moment moet aangebrachte alle vereiste wijzigingen aan de ```$setting``` object. Vergeet niet de wijzigingen wilt doorvoeren.

```powershell
Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a>Beveiligen van gegevens naar Azure Backup

In deze sectie maakt u een productieserver toevoegen aan DPM en Beveilig vervolgens de gegevens naar de lokale DPM-opslag en klik vervolgens op Azure Backup. In de voorbeelden, zullen we laten zien hoe u back-up van bestanden en mappen. De logica kan eenvoudig worden uitgebreid om back-up elke DPM-ondersteunde gegevensbron. Alle uw DPM-back-ups worden geregeld door een Protection Group (PG) met vier onderdelen:

1. **Groep leden** is een lijst met alle beveiligbare objecten (ook wel bekend als *gegevensbronnen* in DPM) die u wilt beveiligen in dezelfde beveiligingsgroep. U wilt bijvoorbeeld productie-VM's in een beveiligingsgroep en SQL Server-databases in een andere beveiligingsgroep beveiligen terwijl ze verschillende back-vereisten hebben mogelijk. Voordat u kunt back-up van een gegevensbron op een productieserver die u nodig hebt om te controleren of wordt de DPM-Agent is geïnstalleerd op de server en wordt beheerd door DPM. Volg de stappen voor [installeren van de DPM-Agent](https://technet.microsoft.com/library/bb870935.aspx) en deze te koppelen aan de juiste DPM-Server.
2. **Methode voor gegevensbeveiliging** Hiermee geeft u de back-up doellocaties - tape, schijf en cloud. In ons voorbeeld beveiligen we gegevens van de lokale schijf en naar de cloud.
3. Een **back-upschema** die aangeeft wanneer back-ups moeten worden genomen en hoe vaak de gegevens moeten worden gesynchroniseerd tussen de DPM-Server en de productieserver.
4. Een **bewaarschema** die aangeeft hoe lang wilt behouden van de herstelpunten in Azure.

### <a name="creating-a-protection-group"></a>Het maken van een beveiligingsgroep

Beginnen met het maken van een nieuwe beveiligingsgroep met de [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.

```powershell
$PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

De bovenstaand cmdlet maakt een beveiligingsgroep met de naam *ProtectGroup01*. Een bestaande beveiligingsgroep kan ook later worden gewijzigd om toe te voegen back-up naar de Azure-cloud. Eventuele wijzigingen aanbrengen in de beveiligingsgroep - nieuwe of bestaande - we moet echter een ingang krijgen op een *kan worden gewijzigd* object met de [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.

```powershell
$MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a>Groepsleden toevoegen aan de beveiligingsgroep

Elke DPM-beveiligingsagent weet de lijst met gegevensbronnen op de server waarop deze is geïnstalleerd. Een gegevensbron toevoegen aan de beveiligingsgroep, moet de DPM-Agent voor het verzenden eerst een lijst van de gegevensbronnen terug naar de DPM-server. Een of meer gegevensbronnen vervolgens worden geselecteerd en toegevoegd aan de beveiligingsgroep toe. De PowerShell-stappen die nodig zijn om dit te bereiken zijn:

1. Haal een lijst van alle servers die worden beheerd door DPM via de DPM-Agent.
2. Kies een specifieke server.
3. Haal een lijst met alle gegevensbronnen op de server.
4. Kies een of meer gegevensbronnen en toe te voegen aan de beveiligingsgroep

De lijst met servers waarop de DPM-Agent is geïnstalleerd en wordt beheerd door de DPM-Server wordt verkregen met de [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet. In dit voorbeeld wordt filteren en alleen PS configureren met de naam *productionserver01* voor back-up.

```powershell
$server = Get-ProductionServer -DPMServerName "TestingServer" | Where-Object {($_.servername) –contains “productionserver01”}
```

Nu de lijst met gegevensbronnen ophalen op ```$server``` met behulp van de [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet. In dit voorbeeld zijn we filteren op het volume *D:\\*  die we willen configureren voor back-up. Deze gegevensbron wordt vervolgens toegevoegd aan de beveiligingsgroep toe met behulp van de [toevoegen DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet. Houd er rekening mee te gebruiken de *kan worden gewijzigd* protection groepsobject ```$MPG``` waarmee de toevoegingen.

```powershell
$DS = Get-Datasource -ProductionServer $server -Inquire | Where-Object { $_.Name -contains “D:\” }

Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

Herhaal deze stap zo vaak als nodig is, totdat u de gekozen gegevensbronnen hebt toegevoegd aan de beveiligingsgroep toe. U kunt ook beginnen met slechts één gegevensbron en voltooien van de werkstroom voor het maken van de beveiligingsgroep en op een later tijdstip meer gegevensbronnen aan de beveiligingsgroep toevoegen.

### <a name="selecting-the-data-protection-method"></a>De methode voor gegevensbeveiliging selecteren

Zodra de gegevensbronnen zijn toegevoegd aan de beveiligingsgroep, de volgende stap is om op te geven van het gebruik van de beveiliging methode de [Set DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet. In dit voorbeeld is de beveiligingsgroep ingesteld voor de lokale schijf en cloudback-up. U moet ook opgeven van de gegevensbron die u wilt beveiligen met behulp van cloud de [toevoegen DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet met - Online vlag.

```powershell
Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a>Instellen van de bewaartermijn

De bewaarperiode instellen voor de back-up verwijst met behulp van de [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet. Terwijl het lijkt het alsof oneven is de bewaarperiode instellen voordat de back-upschema is gedefinieerd, met behulp van de ```Set-DPMPolicyObjective``` cmdlet stelt automatisch een standaard back-upschema die vervolgens kan worden gewijzigd. Het is altijd mogelijk de back-up plannen eerst instellen en het bewaarbeleid na.

In het onderstaande voorbeeld stelt de cmdlet de parameters van de bewaarperiode voor back-ups van schijf. Dit, back-ups behouden voor 10 dagen en gegevens synchroniseren tussen de productieserver en de DPM-server om de 6 uur. De ```SynchronizationFrequencyMinutes``` bevat geen definitie van hoe vaak een back-uppunt is gemaakt, maar hoe vaak gegevens worden gekopieerd naar de DPM-server.  Deze instelling voorkomt dat back-ups te groot.

```powershell
Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

Voor back-ups naar Azure (DPM verwijst naar deze als Online back-ups) de bewaartermijnen kunnen worden geconfigureerd voor [langdurig bewaren met behulp van een zogenaamde grootvader-vader-zoon-schema (algemene)](backup-azure-backup-cloud-as-tape.md). Dat wil zeggen, kunt u een gecombineerde bewaarbeleid met betrekking tot dagelijks, wekelijks, maandelijks en jaarlijks bewaarbeleid definiëren. In dit voorbeeld wordt een matrix vertegenwoordigen de complexe bewaarschema die we willen maken en configureer vervolgens de bewaarperiode bereik met behulp de [Set DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.

```powershell
$RRlist = @()
$RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
$RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
$RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
$RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a>Instellen van het back-upschema

DPM een back-upschema standaard wordt automatisch ingesteld als u de bescherming van objectieve met behulp van opgeeft de ```Set-DPMPolicyObjective``` cmdlet. U kunt de standaardschema's wijzigen met de [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet gevolgd door de [Set DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.

```powershell
$onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
Set-DPMProtectionGroup -ProtectionGroup $MPG
```

In het bovenstaande voorbeeld ```$onlineSch``` is een matrix met vier elementen met de bestaande planning voor online beveiliging voor de beveiligingsgroep in het algemene schema:

1. ```$onlineSch[0]``` bevat de dagelijkse planning
2. ```$onlineSch[1]``` bevat de wekelijkse planning
3. ```$onlineSch[2]``` bevat het maandelijkse schema
4. ```$onlineSch[3]``` bevat de jaarlijkse planning

Dus als u de wekelijkse planning te wijzigen wilt, u moet om te verwijzen naar de ```$onlineSch[1]```.

### <a name="initial-backup"></a>Eerste back-up

Wanneer back-ups van een gegevensbron voor de eerste keer, moeten door DPM worden eerste replica maakt die wordt gemaakt van een volledige kopie van de gegevensbron te worden beveiligd op de DPM-replicavolume is gemaakt. Deze activiteit kan worden gepland voor een bepaald tijdstip, of handmatig kan worden geactiveerd, met behulp van de [Set DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet met de parameter ```-NOW```.

```powershell
Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```

### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a>De grootte van de DPM-Replica en herstelpuntvolume wijzigen

U kunt ook de grootte van de DPM-replicavolume en het gebruik van de schaduwkopie-volume wijzigen [Set DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet zoals in het volgende voorbeeld: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation - Datasource $DS - ProtectionGroup $MPG-handmatige - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)

### <a name="committing-the-changes-to-the-protection-group"></a>Doorvoeren van de wijzigingen aan de beveiligingsgroep

Tot slot moeten de wijzigingen worden doorgevoerd voordat DPM duren de back-up per de nieuwe configuratie van de beveiligingsgroep voordat kan. Dit kan worden bereikt met behulp van de [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.

```powershell
Set-DPMProtectionGroup -ProtectionGroup $MPG
```

## <a name="view-the-backup-points"></a>De back-uppunten weergeven

U kunt de [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet voor een lijst van alle herstelpunten voor een gegevensbron. In dit voorbeeld wordt het volgende doen:

* alle PGs ophalen op de DPM-server en opgeslagen in een matrix ```$PG```
* ophalen van de gegevensbronnen die overeenkomt met de ```$PG[0]```
* Haal alle herstelpunten voor een gegevensbron.

```powershell
$PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
$DS = Get-DPMDatasource -ProtectionGroup $PG[0]
$RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a>Herstellen van gegevens die worden beveiligd op Azure

Herstellen van gegevens is een combinatie van een ```RecoverableItem``` object en een ```RecoveryOption``` object. Er is een lijst van de back-uppunten voor een gegevensbron in de vorige sectie.

In het onderstaande voorbeeld laten we zien hoe u een Hyper-V virtuele machine van Azure Backup herstellen door back-uppunten in combinatie met het doel voor herstel. Dit voorbeeld bevat:

* Het maken van een herstel optie met de [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.
* Ophalen van de matrix van back-uppunten met behulp van de ```Get-DPMRecoveryPoint``` cmdlet.
* Een back-uppunt uit te kiezen.

```powershell
$RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

$PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
$DS = Get-DPMDatasource -ProtectionGroup $PG[0]
$RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

De opdrachten kunnen gemakkelijk worden uitgebreid voor elk gegevensbrontype.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over DPM op Azure Backup [Inleiding tot DPM back-up](backup-azure-dpm-introduction.md)
