---
title: PowerShell gebruiken om back-up van Windows Server naar Azure | Microsoft Docs
description: Meer informatie over het implementeren en beheren van Azure Backup met behulp van PowerShell
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 65218095-2996-44d9-917b-8c84fc9ac415
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: saurse;markgal;jimpark;nkolli;trinadhk
ms.openlocfilehash: d3f165c749af0553c4918b33b0d24cc1e21af2a9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a>Met behulp van PowerShell back-ups implementeren en beheren in Azure voor een Windows-server/Windows-client
> [!div class="op_single_selector"]
> * [ARM](backup-client-automation.md)
> * [Klassiek](backup-client-automation-classic.md)
>
>

In dit artikel leest u hoe u PowerShell gebruikt voor het instellen van Azure Backup op Windows Server of een Windows-client en het beheren van back-up en herstel.

## <a name="install-azure-powershell"></a>Azure PowerShell installeren
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Dit artikel is gericht op Azure Resource Manager (ARM) en de MS Online back-up PowerShell-cmdlets waarmee u een Recovery Services-kluis gebruiken in een resourcegroep.

In oktober 2015 is Azure PowerShell 1.0 uitgebracht. Deze release is geslaagd 0.9.8 vrijgeven en over een aantal belangrijke wijzigingen, met name in het naamgevingspatroon van de cmdlets worden gebracht. 1.0-cmdlets volgen het naamgevingspatroon {werkwoord}-AzureRm {zelfstandig naamwoord}; terwijl de namen in versie 0.9.8 geen **Rm** bevatten (bijvoorbeeld New-AzureRmResourceGroup in plaats van New-AzureResourceGroup). Wanneer u Azure PowerShell 0.9.8 gebruikt, moet u eerst de Resource Manager-modus inschakelen door de opdracht **Switch Azure AzureResourceManager** uit te voeren. Met deze opdracht hoeft niet in 1.0 of hoger.

Als u wilt gebruiken de scripts die zijn geschreven voor de 0.9.8 omgeving in de omgeving 1.0 of hoger, u moet zorgvuldig bijwerken en de scripts in een testomgeving te testen voordat u ze in productie gebruikt om te voorkomen dat onverwachte impact.

[Download de nieuwste versie van PowerShell](https://github.com/Azure/azure-powershell/releases) (minimaal vereiste versie is: 1.0.0)

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken
De volgende stappen leiden u bij het maken van een Recovery Services-kluis. Een Recovery Services-kluis is anders dan een back-upkluis.

1. Als u Azure Backup voor de eerste keer gebruikt, moet u de **registreren AzureRMResourceProvider** cmdlet worden de Azure Recovery Service provider geregistreerd bij uw abonnement.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. De Recovery Services-kluis is een ARM-bron, dus u moet deze binnen een resourcegroep te plaatsen. U kunt een bestaande resourcegroep gebruiken of een nieuwe maken. Bij het maken van een nieuwe resourcegroep, geef de naam en locatie voor de resourcegroep.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "WestUS"
    ```
3. Gebruik de **nieuw AzureRmRecoveryServicesVault** cmdlet voor het maken van de nieuwe kluis. Zorg ervoor dat Geef dezelfde locatie voor de kluis werd gebruikt voor de resourcegroep.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "WestUS"
    ```
4. Geef het type van de redundantie van gegevensopslag worden gebruikt. u kunt [lokaal redundante opslag (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) of [geografisch redundante opslag (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). Het volgende voorbeeld ziet dat de optie - BackupStorageRedundancy voor testVault is ingesteld op GeoRedundant.

   > [!TIP]
   > Veel Azure Backup-cmdlets moet de Recovery Services-kluis-object als invoer. Daarom is het handig zijn voor het opslaan van het back-up Recovery Services-kluis-object in een variabele.
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>De kluizen in een abonnement weergeven
Gebruik **Get-AzureRmRecoveryServicesVault** de lijst met alle kluizen weergeven in het huidige abonnement. U kunt deze opdracht gebruiken om te controleren of een nieuwe kluis is gemaakt, of om te zien welke kluizen zijn beschikbaar in het abonnement.

Voer de opdracht **Get-AzureRmRecoveryServicesVault**, en alle kluizen in het abonnement worden weergegeven.

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="installing-the-azure-backup-agent"></a>De Azure Backup agent installeren
Voordat u de Azure Backup agent installeert, moet u het installatieprogramma gedownload en aanwezig is op de Windows Server hebben. U kunt de nieuwste versie van het installatieprogramma van de [Microsoft Download Center](http://aka.ms/azurebackup_agent) of van de pagina Dashboard van de Recovery Services-kluis. Het installatieprogramma opslaan op een toegankelijke locatie zoals * C:\Downloads\*.

U kunt ook gebruik PowerShell om het downloadprogramma voor het:
 
 ```
 $MarsAURL = 'Http://Aka.Ms/Azurebackup_Agent'
 $WC = New-Object System.Net.WebClient
 $WC.DownloadFile($MarsAURL,'C:\downloads\MARSAgentInstaller.EXE')
 C:\Downloads\MARSAgentInstaller.EXE /q
 ```

Voer de volgende opdracht in een PowerShell-console met verhoogde bevoegdheid voor het installeren van de agent:

```
PS C:\> MARSAgentInstaller.exe /q
```

Hiermee installeert u de agent met de standaardopties. De installatie duurt een paar minuten op de achtergrond. Als u niet geeft de */nu* optie wordt de **Windows Update** venster wordt geopend aan het einde van de installatie om te controleren op updates. Eenmaal geïnstalleerd, wordt de agent wordt weergegeven in de lijst met geïnstalleerde programma's.

De lijst met geïnstalleerde programma's wilt bekijken, gaat u naar **Configuratiescherm** > **programma's** > **programma's en onderdelen**.

![Agent is geïnstalleerd](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Opties voor de installatie
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

## <a name="registering-windows-server-or-windows-client-machine-to-a-recovery-services-vault"></a>Registreren van Windows Server of Windows client-computer naar een Recovery Services-kluis
Nadat u de Recovery Services-kluis hebt gemaakt, downloadt de nieuwste agent en de kluisreferenties en sla deze op een handige locatie zoals C:\Downloads.

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
```

Voer op de Windows Server of Windows client-computer, de [Start OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet de machine te registreren met de kluis.
Deze en andere cmdlets gebruikt voor back-up, zijn afkomstig van de module MSONLINE de AgentInstaller Mars toegevoegd als onderdeel van het installatieproces. 

Het installatieprogramma van Agent wordt niet bijgewerkt voor de $Env: PSModulePath variabele. Dit betekent dat module automatisch laden is mislukt. U kunt dit oplossen kunt u het volgende doen:

```
PS C:\>  $Env:psmodulepath += ';C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules
```

U kunt ook kunt u handmatig laden de module in uw script als volgt:

```
PS C:\>  Import-Module  'C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup'

```

Zodra u de cmdlets Online back-up geladen, kunt u de referenties voor de kluis registreren:


```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :WestUS
Machine registration succeeded.
```

> [!IMPORTANT]
> Gebruik geen relatieve paden om op te geven het kluisreferentiebestand. U moet een absoluut pad opgeven als invoer voor de cmdlet.
>
>

## <a name="networking-settings"></a>Netwerkinstellingen
Wanneer de connectiviteit van de Windows-computer met internet via een proxyserver is, kunnen de proxy-instellingen ook worden opgegeven met de agent. In dit voorbeeld is het geen proxyserver zodat we alle informatie met betrekking tot de proxy expliciet wordt gewist.

Bandbreedtegebruik kan ook worden beheerd met de opties van ```work hour bandwidth``` en ```non-work hour bandwidth``` voor een bepaald aantal dagen van de week.

De proxy- en bandbreedte details van de instelling wordt gedaan met behulp van de [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a>Versleutelingsinstellingen
De back-upgegevens verzonden naar Azure Backup is ter bescherming van de vertrouwelijkheid van de gegevens versleuteld. De wachtwoordzin voor versleuteling is het 'wachtwoord' voor het ontsleutelen van de gegevens op het moment van terugzetten.

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
PS C:\> $PassPhrase = ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force 
PS C:\> $PassCode   = 'AzureR0ckx'
PS C:\> Set-OBMachineSetting -EncryptionPassPhrase $PassPhrase
Server properties updated successfully
```

> [!IMPORTANT]
> De wachtwoordzin gegevens veilig en beveiligd houden als deze eenmaal is ingesteld. Niet zijn mogelijk om gegevens te herstellen van Azure zonder deze wachtwoordzin.
>
>

## <a name="back-up-files-and-folders"></a>Back-up maken van bestanden en mappen
Alle back-ups van Windows-Servers en clients op Azure Backup worden geregeld door een beleid. Het beleid bestaat uit drie delen:

1. Een **back-upschema** die aangeeft wanneer back-ups moeten worden genomen en gesynchroniseerd met de service.
2. Een **bewaarschema** die aangeeft hoe lang voor het bewaren van de herstelpunten die in Azure.
3. Een **opnemen of uitsluiten opgeven van een bestand** die bepaalt wat moet een back-up.

In dit document, omdat er bij het automatiseren van back-up, gebruiken we dat er niets is geconfigureerd. We beginnen met het maken van een nieuwe back-upbeleid met de [nieuw OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet.

```
PS C:\> $newpolicy = New-OBPolicy
```

Op dit moment het beleid is leeg en andere cmdlets zijn nodig om te definiëren welke items worden opgenomen of uitgesloten, wanneer back-ups wordt uitgevoerd en wanneer de back-ups worden opgeslagen.

### <a name="configuring-the-backup-schedule"></a>Het back-upschema configureren
De eerste 3 deel van een beleid is het back-upschema die wordt gemaakt met behulp van de [nieuw OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet. Het back-upschema definieert wanneer back-ups moeten worden genomen. Bij het maken van een planning die u wilt 2 invoerparameters opgeven:

* **Dagen van de week** die de back-up moet worden uitgevoerd. U kunt de back-uptaak op slechts één dag of elke dag van de week of een combinatie in tussen uitvoeren.
* **Momenten van de dag** wanneer de back-up moet worden uitgevoerd. U kunt maximaal 3 verschillende tijdstippen van de dag wanneer de back-up wordt geactiveerd definiëren.

U kunt bijvoorbeeld een back-upbeleid dat wordt uitgevoerd op 4 uur elke zaterdag en zondag configureren.

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

Het back-upschema moet worden gekoppeld aan een beleid en dit kan worden gerealiseerd met behulp van de [Set OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a>Een bewaarbeleid configureren
Het bewaarbeleid definieert hoe lang er herstelpunten gemaakt op basis van de back-uptaken worden bewaard. Bij het maken van een nieuw bewaarbeleid beleid met de [nieuw OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) cmdlet, kunt u het aantal dagen dat de back-up herstelpunten moeten worden bewaard met Azure Backup. Het onderstaande voorbeeld wordt een bewaarbeleid van 7 dagen.

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

Het bewaarbeleid moet worden gekoppeld aan het belangrijkste beleid met de cmdlet [Set OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### <a name="including-and-excluding-files-to-be-backed-up"></a>Opnemen en uitsluiten van bestanden naar een back-up
Een ```OBFileSpec``` object definieert de bestanden moeten worden opgenomen en uitgesloten van een back-up. Dit is een reeks regels die het bereik van de beveiligde bestanden en mappen op een computer. U kunt als veel bestand opgenomen of uitgesloten regels zoals vereist en deze koppelen aan een beleid hebben. Wanneer u een nieuw OBFileSpec-object maakt, kunt u:

* Geef de bestanden en mappen moeten worden opgenomen
* Geef de bestanden en mappen moeten worden uitgesloten
* Geef recursieve back-up van gegevens in een map (of) of alleen de site op het hoogste bestanden in de opgegeven map een back-.

De laatste wordt bereikt door middel van de niet-recursieve - vlag in de opdracht New-OBFileSpec.

In het onderstaande voorbeeld we back-up volume C: en D: en de OS-binaire bestanden in de Windows-map en eventuele tijdelijke mappen uitsluiten. Hiervoor maakt u twee specificaties met behulp van het bestand de [nieuw OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - één voor insluiting en één voor uitsluiting. Als de bestandsspecificaties hebt gemaakt, ze zijn gekoppeld aan het beleid voor gebruik van de [toevoegen OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-the-policy"></a>Het beleid wordt toegepast
Nu het groepsbeleidsobject is voltooid en heeft een bijbehorende back-upschema, bewaarbeleid en een lijst opnemen of uitsluiten van bestanden. Dit beleid kan nu worden doorgevoerd voor Azure Backup te gebruiken. Voordat u het zojuist gemaakte beleid Zorg ervoor dat er geen bestaande back-upbeleid gekoppeld aan de server met behulp van de [verwijderen OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet. Verwijderen van het beleid wordt om bevestiging gevraagd. Het gebruik van de bevestiging overslaan de ```-Confirm:$false``` vlag met de cmdlet.

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

Vastleggen van het groepsbeleidsobject wordt gedaan met behulp van de [Set OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet. Dit wordt ook gevraagd om bevestiging. Het gebruik van de bevestiging overslaan de ```-Confirm:$false``` vlag met de cmdlet.

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want to save this backup policy ? [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

U kunt de details van het bestaande back-upbeleid bekijken de [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet. U kunt inzoomen verder met de [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet voor het back-upschema en de [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet voor het bewaarbeleid

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-ad-hoc-backup"></a>Een ad-hoc back-up maken
Wanneer een back-upbeleid is ingesteld plaats de back-ups per de planning. Activering van een ad-hoc back-up is ook mogelijk met behulp van de [Start OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:

```
PS C:> Get-OBPolicy | Start-OBBackup
Initializing
Taking snapshot of volumes...
Preparing storage...
Generating backup metadata information and preparing the metadata VHD...
Data transfer is in progress. It might take longer since it is the first backup and all data needs to be transferred...
Data transfer completed and all backed up data is in the cloud. Verifying data integrity...
Data transfer completed
In progress...
Job completed.
The backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a>Gegevens terugzetten vanuit Azure Backup
Deze sectie leidt u door de stappen voor het automatiseren van herstel van gegevens vanuit Azure Backup. In dat geval omvat de volgende stappen:

1. Kies het bronvolume
2. Kies een back-up dat u wilt herstellen
3. Kies een item herstellen
4. Het herstelproces geactiveerd

### <a name="picking-the-source-volume"></a>Het bronvolume verzamelen
Om een item uit Azure back-up herstelt, moet u eerst de bron van het item te identificeren. Aangezien we de opdrachten zijn uitgevoerd in de context van een Windows Server of een Windows-client, wordt de machine al geïdentificeerd. De volgende stap bij het identificeren van de bron is het identificeren van het volume met het. Een lijst van volumes of bronnen wordt een back-up van deze computer kan worden opgehaald door het uitvoeren van de [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet. Met deze opdracht retourneert een matrix met alle bronnen die back-up van deze server /-client.

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-from-which-to-restore"></a>Het kiezen van een back-uppunt waaruit u wilt herstellen
Ophalen een lijst met back-uppunten door het uitvoeren van de [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet met de juiste parameters. In ons voorbeeld kiest u de meest recente back-up worden gemaakt voor het bronvolume *D:* en deze gebruiken voor het herstellen van een specifiek bestand.

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
Het object ```$rps``` is een matrix met back-uppunten. Het eerste element is het laatste herstelpunt en het ne element wordt het oudste punt. Als u het laatste herstelpunt, gebruiken we ```$rps[0]```.

### <a name="choosing-an-item-to-restore"></a>Een item herstellen kiezen
De exacte bestand of map gebruik om te herstellen, recursief identificeren de [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet. De maphiërarchie kan worden bekeken met uitsluitend op deze manier de ```Get-OBRecoverableItem```.

In dit voorbeeld als we willen het bestand terugzetten *finances.xls* we kan verwijzen naar die met behulp van het object ```$filesFolders[1]```.

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

U kunt ook zoeken naar items terugzetten met behulp van de ```Get-OBRecoverableItem``` cmdlet. In ons voorbeeld om te zoeken naar *finances.xls* we kan een ingang krijgen op het bestand door deze opdracht uit te voeren:

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-the-restore-process"></a>Activering van het herstelproces
Als u wilt het herstelproces geactiveerd, moeten we eerst de herstelopties opgeven. Dit kan worden gedaan met behulp van de [nieuw OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet. In dit voorbeeld gaan we ervan uit dat we wilt herstellen van bestanden voor de *C:\temp*. Laten we ook uitgaan dat we wilt overslaan van bestanden die al bestaan op de doelmap *C:\temp*. Gebruik de volgende opdracht voor het maken van een hersteloptie:

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

Nu het herstelproces geactiveerd met behulp van de [Start OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) opdracht op de geselecteerde ```$item``` uit de uitvoer van de ```Get-OBRecoverableItem``` cmdlet:

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```


## <a name="uninstalling-the-azure-backup-agent"></a>De Azure Backup agent verwijderen
Verwijderen van de Azure Backup agent kan worden gedaan met behulp van de volgende opdracht:

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

Verwijderen van de binaire bestanden van de agent van de machine heeft een aantal gevolgen te overwegen:

* Het bestandsfilter wordt verwijderd van de machine en bijhouden van wijzigingen is gestopt.
* Alle beleidsgegevens van de machine is verwijderd, maar de beleidsgegevens blijft in de service worden opgeslagen.
* Alle back-upschema's worden verwijderd en er is geen verdere back-ups worden gemaakt.

De gegevens worden echter opgeslagen in Azure blijft en door u aan de hand van de configuratie van beleid is bewaren wordt bewaard. Oudere punten worden automatisch verouderde.

## <a name="remote-management"></a>Extern beheer
Het beheer om de Azure Backup-agent, beleid en gegevensbronnen kan op afstand worden uitgevoerd via PowerShell. De computer die extern worden beheerd moet juist worden voorbereid.

De WinRM-service is standaard geconfigureerd voor handmatig opstarten. Het opstarttype moet worden ingesteld op *automatische* en de service moet worden gestart. Om te controleren of de WinRM-service wordt uitgevoerd, de waarde van de eigenschap Status moet *met*.

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

PowerShell moet worden geconfigureerd voor externe toegang.

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

De computer kan nu worden beheerd op afstand - vanaf de installatie van de agent. Bijvoorbeeld, het volgende script kopieert de agent naar de externe computer en installeert deze.

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over Azure Backup voor Windows Server /-Client, Zie

* [Kennismaking met Azure Backup](backup-introduction-to-azure-backup.md)
* [Back-up van Windows-Servers](backup-configure-vault.md)
