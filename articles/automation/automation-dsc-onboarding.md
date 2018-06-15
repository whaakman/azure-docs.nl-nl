---
title: Computers voorbereiden voor beheer door Azure Automation DSC
description: Het instellen van computers voor beheer met Azure Automation DSC
services: automation
ms.service: automation
ms.component: dsc
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4493f9da0de12fbdfffdf0f4da0dd581ac3b589f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34195553"
---
# <a name="onboarding-machines-for-management-by-azure-automation-dsc"></a>Computers voorbereiden voor beheer door Azure Automation DSC

## <a name="why-manage-machines-with-azure-automation-dsc"></a>Waarom machines met een Azure Automation DSC beheren?

Zoals [PowerShell Desired State Configuration](https://technet.microsoft.com/library/dn249912.aspx), Azure Automation Desired State Configuration is een eenvoudige maar krachtige, configuration management-service voor DSC-knooppunten (fysieke en virtuele machines) in een cloud of on-premises datacenter. Hiermee kunt schaalbaarheid in duizenden computers snel en eenvoudig vanuit een centrale, veilige locatie. U kunt eenvoudig vrijgeven machines, toewijzen ze declaratieve configuraties en rapporten weergeven met elk van de computer de naleving van de gewenste status die u hebt opgegeven. De Azure Automation DSC-beheerlaag is DSC wat het Azure Automation-beheerlaag is PowerShell-scripts. Met andere woorden, op dezelfde manier die Azure Automation kunt u PowerShell-scripts beheren, kunt ook u de DSC-configuraties te beheren. Zie voor meer informatie over de voordelen van het gebruik van Azure Automation DSC, [overzicht van Azure Automation DSC](automation-dsc-overview.md).

Azure Automation DSC kunnen worden gebruikt voor het beheren van tal van machines:

* Virtuele machines in Azure (klassiek)
* Virtuele machines van Azure
* Amazon Web Services (AWS) virtuele machines
* Fysiek virtueel Windows machines on-premises of in een cloud dan Azure/AWS
* Fysiek virtueel Linux-machines on-premises in Azure of in een cloud dan Azure

Bovendien, als u niet klaar voor het beheren van machineconfiguratie vanuit de cloud, kan Azure Automation DSC ook worden gebruikt als een eindpunt in het rapport alleen-lezen. Hiermee kunt u de gewenste configuratie (push) via DSC on-premises instellen en uitgebreide rapportage details weergeven over de naleving van knooppunt met de gewenste status in Azure Automation.

> [!NOTE]
> Het beheren van Azure VM's met DSC is zonder extra kosten opgenomen als de virtuele machine DSC-uitbreiding geïnstalleerd groter dan 2,70 is. Raadpleeg de [ **Automation pagina met prijzen** ](https://azure.microsoft.com/pricing/details/automation/) voor meer informatie.


De volgende secties worden hoe kunt u vrijgeven elk type van de machine aan Azure Automation DSC.

## <a name="azure-virtual-machines-classic"></a>Virtuele machines in Azure (klassiek)

Met Azure Automation DSC kunt u eenvoudig vrijgeven virtuele machines in Azure (klassiek) voor het beheer van de configuratie met de Azure-portal of PowerShell. Achter de schermen en zonder een beheerder op afstand verbinding met de virtuele machine met de extensie Azure VM Desired State Configuration de virtuele machine geregistreerd bij Azure Automation DSC. Omdat de extensie Azure VM Desired State Configuration asynchroon wordt uitgevoerd, stappen voor het bijhouden van de voortgang of het oplossen van dit probleem vindt u in de volgende [ **probleemoplossing voor Azure virtuele machine voorbereiden** ](#troubleshooting-azure-virtual-machine-onboarding) sectie.

### <a name="azure-portal"></a>Azure Portal

In de [Azure-portal](http://portal.azure.com/), klikt u op **Bladeren** -> **virtuele machines (klassiek)**. Selecteer de Windows virtuele machine die u vrijgeven wilt. Klik op de virtuele machine dashboard blade **alle instellingen** -> **extensies** -> **toevoegen** -> **Azure Automation DSC** -> **maken**. Voer de [PowerShell DSC Local Configuration Manager-waarden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) vereist is voor uw gebruiksvoorbeeld registratiecode voor uw Automation-account en registratie-URL en eventueel een knooppuntconfiguratie toewijzen aan de virtuele machine.

![](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

De URL van de registratie van zoeken en sleutel voor het Automation-account om vrij te geven van de computer, Zie de volgende [ **registratie Secure** ](#secure-registration) sectie:

### <a name="powershell"></a>PowerShell

```powershell
# log in to both Azure Service Management and Azure Resource Manager
Add-AzureAccount
Connect-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ""
$ServiceName = ""
$AutomationAccountName = ""
$AutomationAccountResourceGroup = ""

# fill in the name of a Node Configuration in Azure Automation DSC, for this VM to conform to
# NOTE: DSC Node Configuration names are case sensitive in the portal.
$NodeConfigName = ""

# get Azure Automation DSC registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use the DSC extension to onboard the VM for management with Azure Automation DSC
$VM = Get-AzureVM -Name $VMName -ServiceName $ServiceName

$PublicConfiguration = ConvertTo-Json -Depth 8 @{
    SasToken = ""
    ModulesUrl = "https://eus2oaasibizamarketprod1.blob.core.windows.net/automationdscpreview/RegistrationMetaConfigV2.zip"
    ConfigurationFunction = "RegistrationMetaConfigV2.ps1\RegistrationMetaConfigV2"

# update these PowerShell DSC Local Configuration Manager defaults if they do not match your use case.
# See https://technet.microsoft.com/library/dn249922.aspx?f=255&MSPPError=-2147217396 for more details
    Properties = @{
    RegistrationKey = @{
        UserName = 'notused'
        Password = 'PrivateSettingsRef:RegistrationKey'
    }
    RegistrationUrl = $RegistrationInfo.Endpoint
    NodeConfigurationName = $NodeConfigName
    ConfigurationMode = "ApplyAndMonitor"
    ConfigurationModeFrequencyMins = 15
    RefreshFrequencyMins = 30
    RebootNodeIfNeeded = $False
    ActionAfterReboot = "ContinueConfiguration"
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
    -Version 2.19 `
    -PublicConfiguration $PublicConfiguration `
    -PrivateConfiguration $PrivateConfiguration `
    -ForceUpdate

$VM | Update-AzureVM
```

> [!NOTE]
> DSC-knooppuntconfiguratie zijn hoofdlettergevoelig in de portal. Als de aanvraag niet overeen komt wordt het knooppunt niet weergegeven onder DSC-knooppunten.

## <a name="azure-virtual-machines"></a>Virtuele machines van Azure

Azure Automation DSC kunt u eenvoudig vrijgeven Azure virtuele machines voor Configuratiebeheer, via de Azure-portal, Azure Resource Manager-sjablonen of PowerShell. Achter de schermen en zonder een beheerder op afstand verbinding met de virtuele machine met de extensie Azure VM Desired State Configuration de virtuele machine geregistreerd bij Azure Automation DSC. Omdat de extensie Azure VM Desired State Configuration asynchroon wordt uitgevoerd, stappen voor het bijhouden van de voortgang of het oplossen van dit probleem vindt u in de volgende [ **probleemoplossing voor Azure virtuele machine voorbereiden** ](#troubleshooting-azure-virtual-machine-onboarding) sectie.

### <a name="azure-portal"></a>Azure Portal

In de [Azure-portal](https://portal.azure.com/), navigeer naar de Azure Automation-account waarin u vrijgeven virtuele machines wilt. Klik op het dashboard van Automation-account, **DSC-knooppunten** -> **+ Azure VM toevoegen**.

Selecteer een virtuele machine van Azure om vrij te geven.

Als de computer beschikt niet over de PowerShell desired state-uitbreiding is geïnstalleerd en de energiestatus wordt uitgevoerd, klikt u op **Connect**.

Onder **registratie**, voer de [PowerShell DSC Local Configuration Manager-waarden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) vereist is voor uw gebruiksvoorbeeld, en desgewenst een knooppuntconfiguratie toewijzen aan de virtuele machine.

![](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sjablonen

Azure virtuele machines kunnen worden geïmplementeerd en vrijgegeven voor Azure Automation DSC via Azure Resource Manager-sjablonen. Zie [configureren van een virtuele machine via de DSC-extensie en Azure Automation DSC](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) voor een van de voorbeeldsjabloon die onboards een bestaande VM aan Azure Automation DSC. Zoeken naar de registratiecode en registratie-URL genomen als invoer in deze sjabloon, Zie de volgende [ **registratie Secure** ](#secure-registration) sectie.

### <a name="powershell"></a>PowerShell

De [registreren AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) cmdlet kan worden gebruikt voor het vrijgeven van virtuele machines in de Azure portal via PowerShell.

## <a name="amazon-web-services-aws-virtual-machines"></a>Amazon Web Services (AWS) virtuele machines

U kunt eenvoudig vrijgeven Amazon Web Services virtuele machines voor Configuratiebeheer door Azure Automation DSC met behulp van de AWS DSC-Toolkit. U kunt meer informatie over de toolkit [hier](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a>Fysiek virtueel Windows machines on-premises of in een cloud dan Azure/AWS

Lokale Windows-machines en Windows-machines in niet-Azure-clouds (zoals Amazon Web Services) kunnen ook worden vrijgegeven aan Azure Automation DSC zolang ze beschikken over uitgaande toegang tot internet, via een paar eenvoudige stappen:

1. Zorg ervoor dat de nieuwste versie van [WMF 5](http://aka.ms/wmf5latest) is geïnstalleerd op de computers die u voorbereiden voor Azure Automation DSC wilt.
2. Volg de aanwijzingen in de volgende sectie [ **genereren DSC metaconfigurations** ](#generating-dsc-metaconfigurations) voor het genereren van een map met de benodigde DSC-metaconfigurations.
3. Van toepassing op afstand de PowerShell DSC-metaconfiguratie toe op de machines die u vrijgeven wilt. **De machine met deze opdracht wordt uitgevoerd vanaf moet hebben tot de nieuwste versie van [WMF 5](http://aka.ms/wmf5latest) geïnstalleerd**:

    ```powershell
    Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
    ```

4. Als u de PowerShell DSC-metaconfigurations op afstand niet toepassen, kopieert u de map metaconfigurations uit stap 2 op elke machine om vrij te geven. Roep vervolgens **Set DscLocalConfigurationManager** lokaal op elke machine om vrij te geven.
5. Met behulp van de Azure-portal of cmdlets, Controleer of de machines om vrij te geven nu worden weergegeven als DSC-knooppunten die zijn geregistreerd in Azure Automation-account.

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a>Fysiek virtueel Linux-machines on-premises in Azure of in een cloud dan Azure

Lokale Linux-machines, Linux-machines in Azure en Linux-machines in niet-Azure-clouds kunnen ook worden vrijgegeven aan Azure Automation DSC zolang ze beschikken over uitgaande toegang tot internet, via een paar eenvoudige stappen:

1. Zorg ervoor dat de nieuwste versie van [PowerShell Desired State Configuration voor Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) is geïnstalleerd op de computers die u voorbereiden voor Azure Automation DSC wilt.
2. Als de [PowerShell DSC Local Configuration Manager standaardwaarden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) overeenkomen met uw gebruiksvoorbeeld en u wilt vrijgeven machines zoals dat zij **beide** halen uit en te rapporteren aan Azure Automation DSC:

   + Gebruik op elke Linux-machine voor Onboarding van Azure Automation DSC, Register.py om vrij te geven met de standaardinstellingen voor PowerShell DSC Local Configuration Manager:

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   + Zie de volgende informatie over de registratiecode en de registratie-URL voor uw Automation-account [ **registratie Secure** ](#secure-registration) sectie.

     Als de standaardwaarden van de PowerShell DSC Local Configuration Manager **doen** **niet** overeen uw gebruiksvoorbeeld, of u voorbereiden wilt, zodat ze alleen aan Azure Automation DSC rapporteren machines maar geen pull-configuratie of het PowerShell-modules doen vanuit het, volgt u stap 3-6. Ga anders verder met stap 6 rechtstreeks.

3. Volg de aanwijzingen in de volgende [ **genereren DSC metaconfigurations** ](#generating-dsc-metaconfigurations) sectie voor het genereren van een map met de benodigde DSC-metaconfigurations.
4. Van toepassing op afstand de PowerShell DSC-metaconfiguratie toe op de machines die u vrijgeven wilt:

    ```powershell
    $SecurePass = ConvertTo-SecureString -String "<root password>" -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential "root", $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard

    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

De machine met deze opdracht wordt uitgevoerd vanaf moet hebben tot de nieuwste versie van [WMF 5](http://aka.ms/wmf5latest) geïnstalleerd.

1. Als u de PowerShell DSC-metaconfigurations op afstand niet voor elke Linux-machine om vrij te geven toepassen, kopieert u de metaconfiguratie toe die overeenkomt met die machine uit de map die u in stap 5 naar de Linux-machine. Roep vervolgens `SetDscLocalConfigurationManager.py` lokaal op elke Linux-machine u wilt voorbereiden voor Azure Automation DSC:

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

2. Met behulp van de Azure-portal of cmdlets, Controleer of de machines om vrij te geven nu worden weergegeven als DSC-knooppunten die zijn geregistreerd in Azure Automation-account.

## <a name="generating-dsc-metaconfigurations"></a>DSC-metaconfigurations genereren

Om vrij te algemeen geven een aan Azure Automation DSC machine een [DSC-metaconfiguratie toe](https://msdn.microsoft.com/powershell/dsc/metaconfig) kan worden gegenereerd dat, wanneer toegepast, wordt uitgelegd van de DSC-agent op de machine op te halen uit en/of rapporteren aan Azure Automation DSC. DSC-metaconfigurations voor Azure Automation DSC kan worden gegenereerd met behulp van een PowerShell DSC-configuratie of de Azure Automation PowerShell-cmdlets.

> [!NOTE]
> DSC-metaconfigurations bevatten de geheimen die nodig zijn om vrij te geven een machine naar een Automation-account voor beheer. Zorg ervoor dat u goed beveiligen eventuele DSC-metaconfigurations die u maakt of verwijder ze na gebruik.

### <a name="using-a-dsc-configuration"></a>Met behulp van een DSC-configuratie

1. Open de PowerShell ISE als beheerder op een virtuele machine in uw lokale omgeving. De machine moet hebben tot de nieuwste versie van [WMF 5](http://aka.ms/wmf5latest) geïnstalleerd.
2. Kopieer het volgende script lokaal. Dit script bevat een PowerShell DSC-configuratie voor het maken van metaconfigurations en een opdracht voor het maken van de metaconfiguratie starten.

> [!NOTE]
> DSC-knooppuntconfiguratie zijn hoofdlettergevoelig in de portal. Als de aanvraag niet overeen komt wordt het knooppunt niet weergegeven onder DSC-knooppunten.

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

            [String]$ConfigurationMode = "ApplyAndMonitor",

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = "ContinueConfiguration",

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq "")
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
        $RefreshMode = "PUSH"
        }
        else
        {
        $RefreshMode = "PULL"
        }

        Node $ComputerName
        {

            Settings
            {
                RefreshFrequencyMins = $RefreshFrequencyMins
                RefreshMode = $RefreshMode
                ConfigurationMode = $ConfigurationMode
                AllowModuleOverwrite = $AllowModuleOverwrite
                RebootNodeIfNeeded = $RebootNodeIfNeeded
                ActionAfterReboot = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationDSC
                {
                    ServerUrl = $RegistrationUrl
                    RegistrationKey = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationDSC
                {
                ServerUrl = $RegistrationUrl
                RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationDSC
            {
                ServerUrl = $RegistrationUrl
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

3. Vul de registratiecode en de URL in voor uw Automation-account, evenals de namen van de machines om vrij te geven. Alle andere parameters zijn optioneel. Zie de volgende informatie over de registratiecode en de registratie-URL voor uw Automation-account [ **registratie Secure** ](#secure-registration) sectie.
4. Als u wilt dat de machines DSC-statusinformatie rapporteren aan Azure Automation DSC, maar geen pull-configuratie of PowerShell-modules, stelt u de **ReportOnly** parameter in op true.
5. Voer het script uit. U hebt nu een map met de naam **DscMetaConfigs** die in uw werkmap, bevat de PowerShell DSC-metaconfigurations voor de machines voorbereiden (als administrator):

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-the-azure-automation-cmdlets"></a>Azure Automation-cmdlets gebruiken

Als de PowerShell DSC Local Configuration Manager-standaardwaarden overeenkomen met uw gebruiksvoorbeeld en u vrijgeven machines wilt zo dat ze zowel pull van en aan Azure Automation DSC rapporteren, bieden de cmdlets van Azure Automation een vereenvoudigde methode voor het genereren van de DSC-metaconfigurations nodig:

1. Open de PowerShell-console of PowerShell ISE als beheerder op een virtuele machine in uw lokale omgeving.
2. Verbinding maken met behulp van Azure Resource Manager **Connect-AzureRmAccount**
3. De PowerShell DSC-metaconfigurations voor de machines die u wilt downloaden om vrij te geven van het Automation-account die u vrijgeven knooppunten wilt:

    ```powershell
    # Define the parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
    $Params = @{

        ResourceGroupName = 'ContosoResources'; # The name of the ARM Resource Group that contains your Azure Automation Account
        AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation Account where you want a node on-boarded to
        ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the meta configuration will be generated for
        OutputFolder = "$env:UserProfile\Desktop\";
    }
    # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
    # For more info about splatting, run: Get-Help -Name about_Splatting
    Get-AzureRmAutomationDscOnboardingMetaconfig @Params
    ```
    
4. U hebt nu een map met de naam ***DscMetaConfigs***, met de PowerShell DSC-metaconfigurations voor de machines voorbereiden (als administrator):
    
    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a>Beveiligde registratie

Machines kunnen veilig vrijgeven aan een Azure Automation-account via het protocol WMF 5 DSC-registratie, waardoor een DSC-knooppunt om een PowerShell DSC-Pull- of Reporting server (met inbegrip van Azure Automation DSC) te verifiëren. Het knooppunt wordt geregistreerd met de server op een **registratie URL**, verificatie uitvoert met behulp van een **registratiesleutel**. Tijdens de registratie, de DSC-knooppunt en DSC-Pull/Reporting-server om te onderhandelen over een uniek certificaat voor dit knooppunt wilt gebruiken voor verificatie op de na serverregistratie. Dit proces wordt voorkomen dat de knooppunten van imitatie van een die andere, zoals wanneer een knooppunt is geknoeid en gedragen met kwaadaardige bedoelingen vrijgegeven. Na de registratie, de registratiesleutel wordt niet gebruikt voor verificatie opnieuw en wordt verwijderd uit het knooppunt.

U kunt de vereiste informatie voor het protocol van de DSC-registratie van **sleutels** onder **Accountinstellingen** in de Azure portal. Deze blade openen door te klikken op het sleutelpictogram op de **Essentials** Configuratiescherm voor het Automation-account.

![](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

* Registratie-URL is het veld URL in de blade sleutels beheren.
* Registratiecode is de primaire toegangssleutel of de secundaire toegangssleutel op de blade sleutels beheren. De sleutel kan worden gebruikt.

Voor extra beveiliging, de primaire en secundaire toegangssleutel van een Automation-account op elk gewenst moment kunnen worden hersteld (op het **sleutels beheren** blade) om te voorkomen dat toekomstige knooppunt registraties met vorige sleutels.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Het voorbereiden van de virtuele machine van Azure oplossen

Azure Automation DSC kunt u eenvoudig Azure Windows VM's vrijgeven voor Configuratiebeheer. De extensie Azure VM Desired State Configuration wordt achter de schermen gebruikt voor het registreren van de virtuele machine met Azure Automation DSC. Omdat de extensie Azure VM Desired State Configuration asynchroon wordt uitgevoerd, kunnen de voortgang bijhouden en probleemoplossing van de uitvoering ervan belangrijk zijn.

> [!NOTE]
> Een voorbereiden op een Windows Azure VM aan Azure Automation DSC die gebruikmaakt van de extensie Azure VM Desired State Configuration-methode kan een uur duren voor het knooppunt om maximaal geregistreerd in Azure Automation weer te geven. Dit wordt veroorzaakt door de installatie van Windows Management Framework 5.0 op de virtuele machine voor de Azure VM DSC-uitbreiding vereist om vrij te geven is de VM naar Azure Automation DSC.

Als u wilt oplossen of de status van de Azure VM Desired State Configuration-extensie bekijken, in de Azure portal gaat u naar de virtuele machine wordt vrijgegeven en klik vervolgens op **extensies** onder **instellingen**. Klik vervolgens op **DSC** of **DSCForLinux** , afhankelijk van uw besturingssysteem. Voor meer informatie kunt u **gedetailleerde status weergeven**.

## <a name="certificate-expiration-and-reregistration"></a>Vervaldatum van het certificaat en servernaam

Na de registratie van een machine als een DSC-knooppunt in Azure Automation DSC zijn er een aantal redenen waarom u wellicht dat knooppunt in de toekomst opnieuw registreren:

* Na de registratie wordt elk knooppunt automatisch onderhandeld over een uniek certificaat voor verificatie, die na één jaar verloopt. Het protocol PowerShell DSC-registratie kan niet op dit moment automatisch certificaten vernieuwen wanneer ze bijna zijn verlopen, dus moet u de knooppunten van een jaar later opnieuw te registreren. Voordat het opnieuw te registreren, moet u ervoor zorgen dat elk knooppunt wordt uitgevoerd op de Windows Management Framework 5.0 RTM. Als een knooppunt authentication certificaat is verlopen en het knooppunt niet is geregistreerd, het knooppunt kan niet communiceren met Azure Automation en is gemarkeerd als 'Unresponsive'. Servernaam uitgevoerd 90 dagen of minder van de verlooptijd van het certificaat, of op elk gewenst moment na de verlooptijd certificaat resulteert in een nieuw certificaat wordt gegenereerd en gebruikt.
* Niet te wijzigen [PowerShell DSC Local Configuration Manager-waarden](https://msdn.microsoft.com/powershell/dsc/metaconfig4) die tijdens de initiële registratie van het knooppunt, zoals ConfigurationMode zijn ingesteld. Deze waarden DSC-agent kunnen op dit moment alleen worden gewijzigd via servernaam. De enige uitzondering hierop is de toegewezen aan het knooppunt knooppuntconfiguratie--dit kan rechtstreeks worden gewijzigd in Azure Automation DSC.

Servernaam kan worden uitgevoerd op dezelfde manier als die u geregistreerd het knooppunt in eerste instantie met een van de voorbereiding-methoden die in dit document worden beschreven. U hoeft niet de registratie van een knooppunt uit Azure Automation DSC voordat deze opnieuw te registreren.

## <a name="related-articles"></a>Verwante artikelen

* [Overzicht van Azure Automation DSC](automation-dsc-overview.md)
* [Azure Automation DSC-cmdlets](/powershell/module/azurerm.automation/#automation)
* [Azure Automation DSC-prijzen](https://azure.microsoft.com/pricing/details/automation/)
