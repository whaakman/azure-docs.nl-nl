---
title: Implementeer de mobiliteit van Site Recovery-service met Azure Automation DSC | Microsoft Docs
description: Hierin wordt beschreven hoe u automatisch de Azure Site Recovery Mobility-service en de Azure-agent implementeren voor VM VMware en fysieke server-replicatie naar Azure met Azure Automation DSC
services: site-recovery
documentationcenter: 
author: krnese
manager: lorenr
editor: 
ms.assetid: 1f8cd3ac-0522-48eb-a5f0-679ee9192ddb
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2017
ms.author: krnese
ms.openlocfilehash: 118a2e775ae3d036f58989d9778104e372e8c701
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="deploy-the-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a>De Mobility-service met Azure Automation DSC voor replicatie van de virtuele machine implementeren
In de Operations Management Suite bieden wij u met een uitgebreide back-up en noodherstel die u als onderdeel van uw bedrijfscontinuïteitsplan kunt gebruiken.

We deze reis samen met Hyper-V gestart met behulp van Hyper-V Replica. Maar we zijn uitgebreid ter ondersteuning van een heterogene setup omdat klanten hebben van meerdere hypervisors en platforms in hun clouds.

Als u VMware werkbelastingen en/of fysieke servers vandaag, een beheerserver alle Azure Site Recovery-onderdelen wordt uitgevoerd in uw omgeving voor het afhandelen van de communicatie- en replicatie met Azure, wanneer Azure de bestemming is.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>De Site Recovery Mobility-service implementeren met behulp van Automation DSC
Laten we beginnen als volgt een snel overzicht van de werking van deze beheerserver.

De beheerserver wordt uitgevoerd voor verschillende serverfuncties. Een van deze functies is *configuratie*, die coördineert de communicatie en processen voor replicatie en herstel van gegevens beheert.

Bovendien de *proces* rol fungeert als replicatiegateway. Deze rol ontvangt replicatiegegevens van beveiligde bronmachines, optimaliseert met caching, compressie en codering en stuurt deze naar Azure storage-account. Een van de functies voor de Procesrol is ook voor push-installatie van de Mobility-service naar beveiligde computers en het uitvoeren van automatische detectie van virtuele VMware-machines.

Als er een failback vanuit Azure, de *hoofddoel* rol de replicatiegegevens als onderdeel van deze bewerking wordt afgehandeld.

Voor de beveiligde machines we zijn afhankelijk van de *Mobility-service*. Dit onderdeel wordt geïmplementeerd op elke machine (VM VMware of fysieke server) die u wilt repliceren naar Azure. Deze gegevens schrijfbewerkingen op de machine vastgelegd en stuurt deze door naar de beheerserver (Procesrol).

U bent betreft de bedrijfscontinuïteit, is het belangrijk om te begrijpen uw werkbelastingen, uw infrastructuur en de betrokken onderdelen. U kunt vervolgens voldoet aan de vereisten voor de beoogde hersteltijd (RTO) als het beoogde herstelpunt (RPO). De Mobility-service is in deze context sleutel om ervoor te zorgen dat uw werkbelastingen zijn beveiligd, zoals u zou verwachten.

Hoe kunt u, op een geoptimaliseerde manier ervoor zorgen dat u een betrouwbare beveiligde ingesteld met behulp van een aantal onderdelen van Operations Management Suite hebt?

In dit artikel bevat een voorbeeld van hoe u Azure Automation Desired State Configuration (DSC), samen met Site Recovery gebruiken kunt om ervoor te zorgen dat:

* De Mobility-service en de Azure VM-agent zijn geïmplementeerd met de Windows-machines die u wilt beveiligen.
* De Mobility-service en de Azure VM-agent worden altijd uitgevoerd wanneer Azure het replicatiedoel is.

## <a name="prerequisites"></a>Vereisten
* Een opslagplaats voor het opslaan van de vereiste instellingen
* Een opslagplaats voor het opslaan van de vereiste wachtwoordzin te registreren met de beheerserver

  > [!NOTE]
  > Een unieke wachtwoordzin wordt gegenereerd voor elke beheerserver. Als u implementeren meerdere beheerservers wilt, hebt u om ervoor te zorgen dat de juiste wachtwoordzin is opgeslagen in het bestand passphrase.txt.
  >
  >
* Windows Management Framework (WMF) 5.0 geïnstalleerd op de computers die u wilt inschakelen voor beveiliging (een vereiste voor Automation DSC)

  > [!NOTE]
  > Als u gebruiken DSC voor Windows-machines waarvoor WMF 4.0 is geïnstalleerd wilt, Zie de sectie [DSC gebruiken in niet-verbonden omgevingen](## Use DSC in disconnected environments).
  

De Mobility-service kan worden geïnstalleerd via de opdrachtregel en verschillende argumenten. Daarom moet u de binaire bestanden (nadat ze het uitpakken van uw instellingen) hebben en deze opslaan op een locatie waar u ze ophalen kunt met behulp van een DSC-configuratie.

## <a name="step-1-extract-binaries"></a>Stap 1: De binaire bestanden uitpakken
1. De bestanden die u nodig hebt om deze installatie wilt uitpakken, blader naar de volgende map op de beheerserver:

    **\Microsoft azure Site Recovery\home\svsystems\pushinstallsvc\repository**

    In deze map ziet u een MSI-bestand met de naam:

    **Microsoft ASR_UA_version_Windows_GA_date_Release.exe**

    Gebruik de volgende opdracht om op te halen van het installatieprogramma:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**
2. Selecteer alle bestanden en ze verzenden naar een gecomprimeerde map (ingepakte).

U hebt nu de binaire bestanden die u de installatie van de Mobility-service automatiseren moet met behulp van Automation DSC.

### <a name="passphrase"></a>Wachtwoordzin
Vervolgens moet u bepalen waar u deze gecomprimeerde map geplaatst. U kunt een Azure storage-account gebruiken zoals later, voor het opslaan van de wachtwoordzin op die u nodig hebt voor de installatie. De agent wordt vervolgens geregistreerd met de beheerserver als onderdeel van het proces.

De wachtwoordzin op die u hebt verkregen tijdens de implementatie van de beheerserver kan als passphrase.txt naar een tekstbestand worden opgeslagen.

Plaats de gecomprimeerde map en de wachtwoordzin in een specifieke container in Azure storage-account.

![Locatie van de map](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Als u liever om deze bestanden op een share op uw netwerk te houden, kunt u dit doet. Alleen moet u ervoor zorgen dat de DSC-resource die u later gaat gebruiken toegang heeft en de installatie en wachtwoordzin kunt downloaden.

## <a name="step-2-create-the-dsc-configuration"></a>Stap 2: De DSC-configuratie maken
De installatie, is afhankelijk van WMF 5.0. WMF 5.0 moet aanwezig zijn voor de machine niet toepassen op de configuratie via Automation DSC.

De omgeving de volgende voorbeeld DSC-configuratie gebruikt:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
De configuratie wordt het volgende doen:

* De variabelen vertelt de configuratie waar u de binaire bestanden voor de Mobility-service en de Azure VM-agent, waar u de wachtwoordzin en waar de uitvoer wordt opgeslagen.
* De configuratie importeert de xPSDesiredStateConfiguration DSC-resource, zodat u kunt `xRemoteFile` de bestanden te downloaden uit de opslagplaats.
* De configuratie maakt een map waar u wilt opslaan van de binaire bestanden.
* De resource archief wordt Pak de bestanden van de gecomprimeerde map.
* De bron van de installatie van het pakket installeert de Mobility-service van de UNIFIEDAGENT. EXE-installatieprogramma met de specifieke argumenten. (De variabelen die samenstellen van de argumenten moeten worden gewijzigd naar aanleiding van uw omgeving.)
* Het pakket AzureAgent resource installeert de Azure VM-agent, die wordt aanbevolen voor elke virtuele machine die wordt uitgevoerd in Azure. De Azure VM-agent maakt het ook mogelijk extensies toevoegen aan de virtuele machine na een failover.
* De service of meer resources zorgt ervoor dat de bijbehorende Mobility-services en de Azure-services altijd worden uitgevoerd.

De configuratie opslaan als **ASRMobilityService**.

> [!NOTE]
> Vergeet niet ter vervanging van de CSIP in uw configuratie zodat de werkelijke-beheerserver, zodat de agent goed zijn verbonden en de juiste wachtwoordzin die wordt gebruikt.
>
>

## <a name="step-3-upload-to-automation-dsc"></a>Stap 3: Upload naar Automation DSC
Omdat de DSC-configuratie die u hebt aangebracht, een vereiste resource DSC-module (xPSDesiredStateConfiguration) importeren wordt, moet u die module in Automation importeren voordat u de DSC-configuratie uploadt.

Aanmelden bij uw Automation-account, blader naar **activa** > **Modules**, en klik op **bladeren galerie**.

Hier kunt u zoeken naar de module en importeer ze in uw account.

![Module importeren](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Wanneer u klaar bent, gaat u naar de computer waar u de Azure Resource Manager-modules geïnstalleerd hebt en gaat u verder met het importeren van de zojuist gemaakte DSC-configuratie.

### <a name="import-cmdlets"></a>Cmdlets voor importeren
In PowerShell, moet u zich aanmelden bij uw Azure-abonnement. Wijzig de cmdlets voor overeenstemming met uw omgeving en uw Automation-accountgegevens in een variabele vastleggen:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Uploaden van de configuratie van Automation DSC met behulp van de volgende cmdlet:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>De Automation DSC-configuratie compileren
Vervolgens moet u de Automation DSC-configuratie compileren zodat u beginnen kunt met knooppunten te registreren. U bereiken die door de volgende cmdlet:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Dit kan enkele minuten duren, omdat u de configuratie in feite naar de gehoste service voor DSC-pull implementeert.

Nadat u de configuratie compileren, kunt u de informatie over de taak ophalen met behulp van PowerShell (Get-AzureRmAutomationDscCompilationJob) of met behulp van de [Azure-portal](https://portal.azure.com/).

![Taak ophalen](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

U hebt nu gepubliceerd en DSC-configuratie geüpload naar Automation DSC.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Stap 4: Vrijgeven machines Automation DSC
> [!NOTE]
> Een van de vereisten voor het voltooien van dit scenario is dat uw Windows-machines worden bijgewerkt met de nieuwste versie van WMF. U kunt downloaden en installeren van de juiste versie voor uw platform van de [Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=50395).
>
>

U maakt nu een metaconfig voor DSC die u op de knooppunten worden toegepast. Als u wilt dit is gelukt, moet u voor het ophalen van de eindpunt-URL en de primaire sleutel voor uw geselecteerde Automation-account in Azure. U vindt deze waarden onder **sleutels** op de **alle instellingen** blade voor het Automation-account.

![Sleutelwaarden](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

In dit voorbeeld hebt u een fysieke Windows Server 2012 R2-server die u beveiligen wilt met behulp van Site Recovery.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Controleren of er bestandsbewerkingen wijzigen in het register
Voordat u begint met de server aan de Automation DSC-eindpunt kunt koppelen, wordt u aangeraden te controleren of in behandeling zijnde bewerkingen voor een bestandsserver wijzigen in het register. Ze mogelijk de installatie verbieden vanwege een opnieuw opstarten in behandeling is voltooid.

Voer de volgende cmdlet om te controleren of er geen opnieuw opstarten in behandeling op de server:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Als u dit leeg ziet, bent u OK om door te gaan. Als dat niet het geval is, moet u dit oplossen door de server opnieuw opstarten tijdens een onderhoudsvenster.

De configuratie toepassen op de server, start PowerShell Integrated Scripting Environment (ISE) en voer het volgende script. Dit is in wezen een DSC lokale configuratie waarmee de lokale Configuration Manager-engine registreren bij de Automation DSC-service en de specifieke configuratie (ASRMobilityService.localhost) op te halen.

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Deze configuratie zorgt ervoor dat de lokale Configuration Manager-engine zelf registreren bij Automation DSC. Dit wordt ook bepalen hoe de engine moet worden toegepast, wat dit moet doen als er een configuratie-afwijking (ApplyAndAutoCorrect) en hoe dit moet doorgaan met de configuratie als een herstart vereist is.

Nadat u hebt dit script uitvoert, moet het knooppunt eerst registreren bij Automation DSC.

![Knooppunt-registratie in voortgang](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Als u terug naar de Azure-portal gaat, ziet u dat het nieuw ingeschreven knooppunt nu is gepubliceerd in de portal.

![Geregistreerde knooppunt in de portal](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

U kunt de volgende PowerShell-cmdlet om te controleren of het knooppunt juist is geregistreerd kunt uitvoeren op de server:

```powershell
Get-DscLocalConfigurationManager
```

Nadat de configuratie is opgehaald en toegepast op de server, kunt u dit controleren door de volgende cmdlet:

```powershell
Get-DscConfigurationStatus
```

De uitvoer ziet u dat de server heeft de configuratie is opgehaald:

![Uitvoer](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Bovendien de installatie van de Mobility-service heeft een eigen logboekbestanden die kan worden gevonden op *SystemDrive*\ProgramData\ASRSetupLogs.

Dat is alles. U hebt nu geïmplementeerd en geregistreerd van de Mobility-service op de computer die u beveiligen wilt met behulp van Site Recovery. DSC ervoor dat de vereiste services altijd worden uitgevoerd.

![Geslaagde implementatie](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Nadat de beheerserver de geslaagde implementatie detecteert, kunt u beveiliging configureren en inschakelen van replicatie op de machine met behulp van Site Recovery.

## <a name="use-dsc-in-disconnected-environments"></a>Gebruik van DSC in niet-verbonden omgevingen
Als uw machines zijn niet met Internet verbonden, kunt u nog steeds zijn afhankelijk van de DSC implementeren en configureren van de Mobility-service op de werkbelastingen die u wilt beveiligen.

U kunt uw eigen DSC-pull-server in uw omgeving te bieden in wezen dezelfde mogelijkheden die u via Automation DSC instantiëren. Dat wil zeggen, wordt de clients klikt, de configuratie het eindpunt van de DSC (nadat deze geregistreerd). Een andere optie is echter handmatig pushen van de DSC-configuratie op uw computers, lokaal of extern.

Houd er rekening mee dat in dit voorbeeld, wordt er een extra parameter voor de computernaam. De externe bestanden zich nu op een externe share die toegankelijk moet zijn door de machines die u wilt beveiligen. Het einde van het script enacts van de configuratie en start vervolgens de DSC-configuratie toepassen op de doelcomputer.

### <a name="prerequisites"></a>Vereisten
Zorg ervoor dat de xPSDesiredStateConfiguration PowerShell-module is geïnstalleerd. Voor Windows-computers waarop WMF 5.0 wordt geïnstalleerd, kunt u de module xPSDesiredStateConfiguration door de volgende cmdlet wordt uitgevoerd op de doelcomputers te installeren:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

U kunt ook downloaden en opslaan van de module als u wilt distribueren naar de Windows-machines waarvoor WMF 4.0. Deze cmdlet uitvoeren op een machine waarin PowerShellGet (WMF 5.0) aanwezig is:

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Voor WMF 4.0, zorg er ook voor dat de [update voor Windows 8.1 KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) op de computers is geïnstalleerd.

De volgende configuratie kan worden geactiveerd met Windows-machines waarvoor WMF 5.0 en WMF 4.0:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Als u wilt het instantiëren van uw eigen DSC-pull-server in uw bedrijfsnetwerk om na te bootsen van de mogelijkheden die u kunt via Automation DSC, Zie [instellen van een DSC-webserver pull](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Optioneel: Een DSC-configuratie met behulp van een Azure Resource Manager-sjabloon implementeren
In dit artikel is gericht op hoe kunt u uw eigen DSC-configuratie om automatisch te implementeren van de Mobility-service en de Azure VM-Agent-- en ervoor te zorgen dat ze worden uitgevoerd op de machines die u wilt beveiligen. We hebben ook een Azure Resource Manager-sjabloon die u deze DSC-configuratie naar een nieuwe of bestaande Azure Automation-account implementeert. De sjabloon wordt invoerparameters gebruiken om Automation activa waarin u de variabelen voor uw omgeving te maken.

Nadat u de sjabloon implementeert, kunt u gewoon verwijzen naar stap 4 in deze handleiding om vrij te geven uw machines.

De sjabloon wordt het volgende doen:

1. Gebruik een bestaand automatiseringsaccount of een nieuwe maken
2. Invoerparameters voor nemen:
   * ASRRemoteFile--de locatie waar u de Mobility-service-instellingen hebt opgeslagen
   * ASRPassphrase--de locatie waar u het bestand passphrase.txt hebt opgeslagen
   * ASRCSEndpoint--het IP-adres van de beheerserver
3. Importeer de PowerShell-module xPSDesiredStateConfiguration
4. Maken en de DSC-configuratie compileren

De voorgaande stappen vindt plaats in de juiste volgorde, zodat u uw computers voor beveiliging voor onboarding starten kunt.

De sjabloon met instructies voor implementatie bevindt zich op [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

De sjabloon implementeren met behulp van PowerShell:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Volgende stappen
Nadat u de Mobility-service-agents te implementeren, kunt u [replicatie inschakelen](site-recovery-vmware-to-azure.md) voor de virtuele machines.
