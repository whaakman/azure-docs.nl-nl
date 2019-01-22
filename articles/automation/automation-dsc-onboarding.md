---
title: Onboarding van machines voor beheer met Azure Automation State Configuration
description: Het instellen van machines voor beheer met Azure Automation State Configuration
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
ms.topic: conceptual
ms.date: 08/08/2018
manager: carmonm
ms.openlocfilehash: 74587d05ec27e87d87aa3647c22f5c90a9cdcf58
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54436387"
---
# <a name="onboarding-machines-for-management-by-azure-automation-state-configuration"></a>Onboarding van machines voor beheer met Azure Automation State Configuration

## <a name="why-manage-machines-with-azure-automation-state-configuration"></a>Waarom machines met Azure Automation State Configuration beheren?

Zoals [PowerShell Desired State Configuration](/powershell/dsc/overview), Statusconfiguratie van Azure Automation is een eenvoudige maar krachtige, configuration management-service voor DSC-knooppunten (fysieke en virtuele machines) in een cloud of on-premises datacenter . Het biedt schaalbaarheid voor duizenden machines snel en eenvoudig vanuit een centrale, veilige locatie. Kunt u machines snel vrijgeven, wijzen ze declaratieve configuraties en rapporten weergeven die met elk van de computer de naleving van de gewenste status die u hebt opgegeven. De Azure Automation State Configuration management-laag is DSC wat de beheerlaag van Azure Automation is PowerShell-scripts. Met andere woorden, op dezelfde manier waarmee u Azure Automation kunt u PowerShell-scripts beheren, kunt ook u beheer DSC-configuraties. Zie voor meer informatie over de voordelen van het gebruik van Azure Automation State Configuration, [overzicht van Azure Automation State Configuration](automation-dsc-overview.md).

Configuratie van Azure Automation-status kan worden gebruikt voor het beheren van verschillende machines:

- Virtuele Azure-machines (geïmplementeerd in het klassieke implementatiemodel en Azure Resource Manager-implementatiemodel)
- Amazon Web Services (AWS) EC2 instances
- Fysieke/virtuele Windows-machines on-premises of in een andere cloud dan Azure/AWS
- Fysieke/virtuele Linux-machines on-premises, in Azure, of in een andere cloud dan Azure

Bovendien, als u niet klaar voor het beheren van de configuratie van machine vanuit de cloud, kan Azure Automation State Configuration ook worden gebruikt als een eindpunt in het rapport alleen-lezen. Hiermee kunt u de gewenste configuratie (push) via DSC on-premises en uitgebreide rapportage details weergeven over de naleving van knooppunt van de gewenste status in Azure Automation.

> [!NOTE]
> Het beheren van virtuele Azure-machines met de configuratie van de status is opgenomen zonder extra kosten als de virtuele machine DSC-extensie geïnstalleerd dan 2.70 is. Raadpleeg de [ **Automation pagina met prijzen** ](https://azure.microsoft.com/pricing/details/automation/) voor meer informatie.

De volgende secties wordt beschreven hoe u kunt Onboarding van elk type machine naar Azure Automation State Configuration.

## <a name="azure-virtual-machines-classic"></a>Virtuele Azure-machines (klassiek)

Met Azure Automation State Configuration kunt u eenvoudig Azure-machines vrij (klassiek) voor beheer van de configuratie met behulp van de Azure portal of PowerShell. Achter de schermen en zonder dat een beheerder die op afstand verbinding met de virtuele machine, registreert de Desired State Configuration van Azure VM-extensie de virtuele machine met Azure Automation State Configuration. Omdat de Azure VM Desired State Configuration-extensie wordt asynchroon uitgevoerd, stappen om de voortgang bijhouden of oplossen van dit probleem vindt u in de volgende [ **onboarding voor probleemoplossing voor Azure-virtuele machine** ](#troubleshooting-azure-virtual-machine-onboarding) sectie.

### <a name="azure-portal"></a>Azure Portal

In de [Azure-portal](https://portal.azure.com/), klikt u op **Bladeren** -> **virtuele machines (klassiek)**. Selecteer de Windows-VM die u vrijgeven wilt. Klik op de blade voor het dashboard van de virtuele machine **alle instellingen** -> **extensies** -> **toevoegen** -> **Azure Automation DSC** -> **maken**.
Voer de [PowerShell DSC Local Configuration Manager-waarden](/powershell/dsc/metaconfig4) vereist zijn voor uw situatie, registratiesleutel van uw Automation-account en registratie-URL en eventueel een knooppuntconfiguratie toewijzen aan de virtuele machine.

![Azure VM-extensies voor DSC](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

Te vinden van de registratie-URL en de sleutel voor het Automation-account voor de Onboarding van de machine als u wilt, Zie de volgende [ **beveiligde registratie** ](#secure-registration) sectie:

### <a name="powershell"></a>PowerShell

```powershell
# log in to both Azure Service Management and Azure Resource Manager
Add-AzureAccount
Connect-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ''
$ServiceName = ''
$AutomationAccountName = ''
$AutomationAccountResourceGroup = ''

# fill in the name of a Node Configuration in Azure Automation State Configuration, for this VM to conform to
# NOTE: DSC Node Configuration names are case sensitive in the portal.
$NodeConfigName = ''

# get Azure Automation State Configuration registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use the DSC extension to onboard the VM for management with Azure Automation State Configuration
$VM = Get-AzureVM -Name $VMName -ServiceName $ServiceName

$PublicConfiguration = ConvertTo-Json -Depth 8 @{
    SasToken = ''
    ModulesUrl = 'https://eus2oaasibizamarketprod1.blob.core.windows.net/automationdscpreview/RegistrationMetaConfigV2.zip'
    ConfigurationFunction = 'RegistrationMetaConfigV2.ps1\RegistrationMetaConfigV2'

# update these PowerShell DSC Local Configuration Manager defaults if they do not match your use case.
# See https://docs.microsoft.com/powershell/dsc/metaConfig for more details
    Properties = @{
        RegistrationKey = @{
            UserName = 'notused'
            Password = 'PrivateSettingsRef:RegistrationKey'
        }
        RegistrationUrl = $RegistrationInfo.Endpoint
        NodeConfigurationName = $NodeConfigName
        ConfigurationMode = 'ApplyAndMonitor'
        ConfigurationModeFrequencyMins = 15
        RefreshFrequencyMins = 30
        RebootNodeIfNeeded = $False
        ActionAfterReboot = 'ContinueConfiguration'
        AllowModuleOverwrite = $False
    }
}

$PrivateConfiguration = ConvertTo-Json -Depth 8 @{
    Items = @{
        RegistrationKey = $RegistrationInfo.PrimaryKey
    }
}

$VM = Set-AzureVMExtension `
    -VM $vm `
    -Publisher Microsoft.Powershell `
    -ExtensionName DSC `
    -Version 2.76 `
    -PublicConfiguration $PublicConfiguration `
    -PrivateConfiguration $PrivateConfiguration `
    -ForceUpdate

$VM | Update-AzureVM
```

> [!NOTE]
> Status configuratie-knooppuntconfiguratie namen zijn hoofdlettergevoelig in de portal. Als de aanvraag niet overeen komt het knooppunt wordt niet weergegeven op de **knooppunten** tabblad.

## <a name="azure-virtual-machines"></a>Virtuele machines van Azure

Status van Azure Automation-configuratie kunt u eenvoudig Azure-machines vrij voor Configuratiebeheer, met de Azure portal, Azure Resource Manager-sjablonen of PowerShell. Achter de schermen en zonder dat een beheerder die op afstand verbinding met de virtuele machine, registreert de Desired State Configuration van Azure VM-extensie de virtuele machine met Azure Automation State Configuration.
Omdat de Azure VM Desired State Configuration-extensie wordt asynchroon uitgevoerd, stappen om de voortgang bijhouden of oplossen van dit probleem vindt u in de volgende [ **onboarding voor probleemoplossing voor Azure-virtuele machine** ](#troubleshooting-azure-virtual-machine-onboarding) sectie.

### <a name="azure-portal"></a>Azure Portal

In de [Azure-portal](https://portal.azure.com/), gaat u naar de Azure Automation-account waar u virtuele machines activeren. Op de pagina configuratie van de status en de **knooppunten** tabblad **+ toevoegen**.

Selecteer een virtuele machine van Azure voor de onboarding.

Als de computer beschikt niet over de PowerShell desired state-uitbreiding geïnstalleerd en de status van de stroom wordt uitgevoerd, klikt u op **Connect**.

Onder **registratie**, voer de [PowerShell DSC Local Configuration Manager-waarden](/powershell/dsc/metaconfig4) vereist zijn voor uw situatie, en (optioneel) een knooppuntconfiguratie toewijzen aan de virtuele machine.

![Onboarding](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

Virtuele machines van Azure kan worden geïmplementeerd en vrijgegeven aan een Azure Automation State Configuration via Azure Resource Manager-sjablonen. Zie [configureren van een virtuele machine via DSC-extensie en Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) voor een voorbeeldsjabloon uitvoeren van Onboarding van een bestaande virtuele machine voor Azure Automation State Configuration. Zoeken naar de registratiesleutel en registratie-URL genomen als invoer in deze sjabloon, ziet u de volgende [ **beveiligde registratie** ](#secure-registration) sectie.

### <a name="powershell"></a>PowerShell

De [registreren AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) cmdlet kan worden gebruikt om virtuele machines in Azure portal via PowerShell activeren.

## <a name="amazon-web-services-aws-virtual-machines"></a>Amazon Web Services (AWS) virtuele machines

U kunt eenvoudig onboarding Amazon Web Services virtuele machines voor Configuratiebeheer van Azure Automation State Configuration met behulp van de DSC-Toolkit van AWS. U kunt meer informatie over de toolkit [hier](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a>Fysieke/virtuele Windows-machines on-premises of in een andere cloud dan Azure/AWS

On-premises Windows-computers en Windows-machines in niet-Azure-clouds (zoals Amazon Web Services) is ook toegevoegd aan Azure Automation State Configuration, zolang ze uitgaande toegang tot internet via een paar eenvoudige stappen hebben:

1. Zorg ervoor dat de nieuwste versie van [WMF 5](https://aka.ms/wmf5latest) is geïnstalleerd op de machines die u voorbereiden voor Azure Automation State Configuration wilt.
1. Volg de aanwijzingen in de volgende sectie [ **genereren DSC metaconfigurations** ](#generating-dsc-metaconfigurations) voor het genereren van een map met de benodigde DSC-metaconfigurations.
1. De PowerShell DSC-metaconfiguration op afstand van toepassing op de machines die u vrijgeven wilt. **De machine met deze opdracht wordt uitgevoerd vanaf moet de nieuwste versie van [WMF 5](https://aka.ms/wmf5latest) geïnstalleerd**:

   ```powershell
   Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
   ```

1. Als u niet de PowerShell DSC-metaconfigurations op afstand toepassen, kopieert u de map metaconfigurations uit stap 2 op elke machine voor onboarding. Roep vervolgens **Set-DscLocalConfigurationManager** lokaal op elke machine voor onboarding.
1. Met behulp van de Azure portal of cmdlets, Controleer of de machines om vrij te nu als de configuratie van de status van weergegeven knooppunten in uw Azure Automation-account geregistreerd.

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a>Fysieke/virtuele Linux-machines on-premises, in Azure, of in een andere cloud dan Azure

On-premises Linux-computers, Linux-machines in Azure en Linux-machines in niet-Azure-clouds mag ook toegevoegd aan Azure Automation State Configuration, zolang ze uitgaande toegang tot internet via een paar eenvoudige stappen hebben:

1. Zorg ervoor dat de nieuwste versie van [PowerShell Desired State Configuration voor Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) is geïnstalleerd op de machines die u voorbereiden voor Azure Automation State Configuration wilt.
1. Als de [standaardinstellingen voor PowerShell DSC Local Configuration Manager](/powershell/dsc/metaconfig4) overeenkomen met uw situatie, en u wilt vrijgeven machines zodanig dat ze **beide** ophalen uit en te rapporteren aan de configuratie van Azure Automation-status:

   - Gebruik op elke Linux-machine voor Onboarding van Azure Automation status Configuratin `Register.py` moet worden vrijgegeven met de standaardinstellingen voor PowerShell DSC Local Configuration Manager:

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   - Als de registratiesleutel en de registratie-URL voor uw Automation-account, bezoekt u de volgende [ **beveiligde registratie** ](#secure-registration) sectie.

     Als de PowerShell DSC Local Configuration Manager standaard **niet** overeenkomst uw situatie, of u voorbereiden wilt, zodat ze alleen aan de configuratie van Azure Automation-status rapporteren machines, maar kan geen pull-configuratie of PowerShell modules, volgt u stap 3-6. Ga anders verder rechtstreeks naar stap 6.

1. Volg de aanwijzingen in de volgende [ **genereren DSC metaconfigurations** ](#generating-dsc-metaconfigurations) sectie voor het genereren van een map met de benodigde DSC-metaconfigurations.
1. De PowerShell DSC-metaconfiguration op afstand van toepassing op de machines die u vrijgeven wilt:

    ```powershell
    $SecurePass = ConvertTo-SecureString -String '<root password>' -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential 'root', $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard
    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

De machine met deze opdracht wordt uitgevoerd vanaf moet de nieuwste versie van [WMF 5](https://aka.ms/wmf5latest) geïnstalleerd.

1. Als u de PowerShell DSC-metaconfigurations op afstand niet voor elke Linux-machine voor onboarding toepassen, kopieert u de metaconfiguration overeenkomt met die computer uit de map die u in stap 5 naar de Linux-machine. Roep vervolgens `SetDscLocalConfigurationManager.py` lokaal op elke Linux-machine u wilt voorbereiden in Azure Automation-staat configuratie:

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

1. Met behulp van de Azure portal of cmdlets, Controleer of de machines om vrij te nu als DSC-knooppunten die zijn geregistreerd in uw Azure Automation-account weergegeven.

## <a name="generating-dsc-metaconfigurations"></a>DSC-metaconfigurations genereren

Voor de algemeen onboarding voor een machine naar Azure Automation State Configuration, een [DSC metaconfiguration](/powershell/dsc/metaconfig) kunnen worden gegenereerd dat, wanneer toegepast, instrueert de DSC-agent op de machine op te halen uit en/of Azure Automation-status rapporteren De configuratie. DSC-metaconfigurations voor configuratie van Azure Automation-status kan worden gegenereerd met behulp van een PowerShell DSC-configuratie of de Azure Automation PowerShell-cmdlets.

> [!NOTE]
> DSC-metaconfigurations bevatten de geheimen die nodig zijn voor de onboarding een machine aan een Automation-account voor beheer. Zorg ervoor dat u een goede bescherming van een DSC-metaconfigurations die u maakt of verwijder ze na gebruik.

### <a name="using-a-dsc-configuration"></a>Met behulp van een DSC-configuratie

1. Open de VSCode (of uw favoriete editor) als een beheerder zijn in een virtuele machine in uw lokale omgeving. De machine moet de nieuwste versie van [WMF 5](https://aka.ms/wmf5latest) geïnstalleerd.
1. Kopieer het volgende script lokaal. Met dit script bevat een PowerShell DSC-configuratie voor het maken van metaconfigurations en een opdracht voor het maken van de metaconfiguration een vliegende start.

> [!NOTE]
> Status configuratie-knooppuntconfiguratie namen zijn hoofdlettergevoelig in de portal. Als de aanvraag niet overeen komt het knooppunt wordt niet weergegeven op de **knooppunten** tabblad.

   ```powershell
   # The DSC configuration that will generate metaconfigurations
   [DscLocalConfigurationManager()]
   Configuration DscMetaConfigs
   {
        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = 'ApplyAndMonitor',

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = 'ContinueConfiguration',

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq '')
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
            $RefreshMode = 'PUSH'
        }
        else
        {
            $RefreshMode = 'PULL'
        }

        Node $ComputerName
        {
            Settings
            {
                RefreshFrequencyMins           = $RefreshFrequencyMins
                RefreshMode                    = $RefreshMode
                ConfigurationMode              = $ConfigurationMode
                AllowModuleOverwrite           = $AllowModuleOverwrite
                RebootNodeIfNeeded             = $RebootNodeIfNeeded
                ActionAfterReboot              = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl          = $RegistrationUrl
                    RegistrationKey    = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationStateConfiguration
                {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationStateConfiguration
            {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
   }

    # Create the metaconfigurations
    # NOTE: DSC Node Configuration names are case sensitive in the portal.
    # TODO: edit the below as needed for your use case
   $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
   }

   # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   DscMetaConfigs @Params
   ```

1. Vul de registratiesleutel en de URL in voor uw Automation-account, evenals de namen van de machines voor de onboarding. Alle andere parameters zijn optioneel. Als de registratiesleutel en de registratie-URL voor uw Automation-account, bezoekt u de volgende [ **beveiligde registratie** ](#secure-registration) sectie.
1. Als u de machines voor het rapporteren van DSC-statusinformatie naar Azure Automation State Configuration, maar geen pull-configuratie of PowerShell-modules wilt, stelt u de **ReportOnly** parameter op waar.
1. Voer het script uit. U hebt nu een map met de naam **DscMetaConfigs** in uw werkmap waarin u de PowerShell DSC-metaconfigurations voor de machines moet worden vrijgegeven (als beheerder):

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-the-azure-automation-cmdlets"></a>Met behulp van de Azure Automation-cmdlets

Als de standaardwaarden van de PowerShell DSC Local Configuration Manager overeenkomen met uw situatie en u vrijgeven machines wilt zodanig dat ze zowel waaruit en aan de configuratie van Azure Automation-status rapporteren, bieden de Azure Automation-cmdlets een vereenvoudigde methode voor het genereren van de DSC-metaconfigurations nodig:

1. Open de PowerShell-console of VSCode als beheerder op een virtuele machine in uw lokale omgeving.
2. Verbinding maken met behulp van Azure Resource Manager `Connect-AzureRmAccount`
3. Download de PowerShell DSC-metaconfigurations voor de machines die u wilt zorgen voor Onboarding van het Automation-account dat u vrijgeven knooppunten wilt:

   ```powershell
   # Define the parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
   $Params = @{
       ResourceGroupName = 'ContosoResources'; # The name of the Resource Group that contains your Azure Automation Account
       AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation Account where you want a node on-boarded to
       ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the meta configuration will be generated for
       OutputFolder = "$env:UserProfile\Desktop\";
   }
   # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   Get-AzureRmAutomationDscOnboardingMetaconfig @Params
   ```

1. U hebt nu een map met de naam ***DscMetaConfigs***, met de PowerShell DSC-metaconfigurations voor de machines moet worden vrijgegeven (als beheerder):

    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a>Beveiligde registratie

Machines kunnen veilig vrijgeven aan een Azure Automation-account via het protocol voor het registreren van WMF 5 DSC, waarmee een DSC-knooppunt om te verifiëren met een PowerShell DSC-Pull- of Reporting server (met inbegrip van Azure Automation State Configuration). Het knooppunt wordt geregistreerd met de server op een **registratie-URL**, waarbij verificatie met een **registratiesleutel**. Onderhandelen over een uniek certificaat nodig voor dit knooppunt om te gebruiken voor verificatie op de na serverregistratie tijdens de registratie, het DSC-knooppunt en de DSC-Pull/Reporting-server. Dit proces wordt voorkomen dat de onboarding-knooppunten uit zich voordoen als een die andere, bijvoorbeeld als een knooppunt wordt aangetast en bedoelingen aan de hand. Na de registratie, de registratiesleutel niet wordt gebruikt voor verificatie opnieuw uit en wordt verwijderd uit het knooppunt.

Krijgt u de informatie die nodig zijn voor de configuratie van de status registratie-protocol van **sleutels** onder **Accountinstellingen** in Azure portal. Open deze blade door te klikken op het sleutelpictogram op de **Essentials** deelvenster voor het Automation-account.

![Azure automation-sleutels en de URL](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

- Registratie-URL is de URL-veld in de blade sleutels beheren.
- Registratiesleutel is de primaire toegangssleutel of secundaire toegangssleutel in de blade sleutels beheren. Een sleutel kan worden gebruikt.

Voor extra beveiliging kunt de primaire en secundaire toegangssleutel van een Automation-account op elk gewenst moment kunnen worden hersteld (op de **sleutels beheren** pagina) om te voorkomen dat toekomstige knooppunt registraties met vorige sleutels.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Oplossen van problemen met virtuele Azure-machine voorbereiden

Status van Azure Automation-configuratie kunt u eenvoudig Azure Windows VM's vrijgeven voor Configuratiebeheer. Achter de schermen, wordt de Azure VM Desired State Configuration-extensie gebruikt voor het registreren van de virtuele machine met Azure Automation State Configuration. Omdat de Azure VM Desired State Configuration-extensie wordt asynchroon uitgevoerd, kunnen de voortgang bijhouden en het oplossen van de uitvoering ervan belangrijk zijn.

> [!NOTE]
> Methode voor onboarding van een Azure Windows VM naar Azure Automation status-configuratie die gebruikmaakt van de Desired State Configuration van Azure VM-extensie kan een uur duren voor het knooppunt om maximaal geregistreerd in Azure Automation weer te geven. Dit komt door de installatie van Windows Management Framework 5.0 op de virtuele machine door de Azure VM DSC-extensie, die vereist voor de onboarding is de virtuele machine naar Azure Automation State Configuration.

Als u wilt oplossen of de status van de Desired State Configuration van Azure VM-extensie bekijken, in Azure portal gaat u naar de virtuele machine onboarding wordt uitgevoerd, en klik vervolgens op **extensies** onder **instellingen**. Klik vervolgens op **DSC** of **DSCForLinux** afhankelijk van uw besturingssysteem. Voor meer informatie kunt u **gedetailleerde status weergeven**.

## <a name="certificate-expiration-and-reregistration"></a>Certificaat verloopt en opnieuw registreren

Er zijn een aantal redenen waarom u wellicht opnieuw te registreren dat knooppunt in de toekomst na de registratie van een virtuele machine als een DSC-knooppunt in de configuratie van Azure Automation-status:

- Nadat u hebt geregistreerd, wordt elk knooppunt automatisch onderhandeld over een uniek certificaat voor verificatie die na één jaar verloopt. De PowerShell DSC-registratie-protocol kan niet op dit moment automatisch certificaten vernieuwen wanneer ze bijna zijn verlopen, moet u de knooppunten van een jaar later opnieuw te registreren. Voordat u opnieuw te registreren, zorg ervoor dat elk knooppunt wordt uitgevoerd Windows Management Framework 5.0 RTM. Als het verificatiecertificaat van een knooppunt is verlopen, en het knooppunt niet is geregistreerd, het knooppunt kan niet communiceren met Azure Automation en is gemarkeerd als 'Unresponsive'. Servernaam uitgevoerd 90 dagen of minder vanaf het moment dat certificaat verlopen, of op elk gewenst moment na de vervaldatum certificaat resulteert in een nieuw certificaat wordt gegenereerd en die wordt gebruikt.
- Niet te wijzigen [PowerShell DSC Local Configuration Manager-waarden](/powershell/dsc/metaconfig4) die zijn ingesteld tijdens de eerste registratie van het knooppunt, zoals ConfigurationMode. Deze waarden DSC-agent kunnen op dit moment alleen worden gewijzigd via opnieuw registreren. De enige uitzondering hierop is de knooppuntconfiguratie is toegewezen aan het knooppunt--dit kan worden gewijzigd in Azure Automation DSC rechtstreeks.

Opnieuw registreren kan worden uitgevoerd op dezelfde manier als die u geregistreerd het knooppunt in eerste instantie wordt met behulp van een van de onboarding-methoden die in dit document worden beschreven. U hoeft niet een knooppunt van de configuratie van Azure Automation-status registratie voordat u deze opnieuw te registreren.

## <a name="next-steps"></a>Volgende stappen

- Als u wilt beginnen, Zie [aan de slag met Azure Automation State Configuration](automation-dsc-getting-started.md)
- Zie voor meer informatie over het DSC-configuraties compileren zodat u ze aan de doelknooppunten toewijzen kunt, [-configuraties in Azure Automation-staat configuratie compileren](automation-dsc-compile.md)
- Zie voor naslagdocumentatie voor PowerShell-cmdlet, [Statusconfiguratie van Azure Automation-cmdlets](/powershell/module/azurerm.automation/#automation)
- Zie voor informatie over de prijzen [Statusconfiguratie van Azure Automation-prijzen](https://azure.microsoft.com/pricing/details/automation/)
- Zie voor een voorbeeld van het gebruik van Azure Automation State Configuration in een pijplijn voor continue implementatie [continue implementatie met behulp van Azure Automation Statusconfiguratie- en Chocolatey](automation-dsc-cd-chocolatey.md)
