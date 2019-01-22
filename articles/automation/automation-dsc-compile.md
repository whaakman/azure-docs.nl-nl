---
title: Configuraties in Azure Automation-staat configuratie compileren
description: In dit artikel wordt beschreven hoe u configuraties voor Azure Automation Desired State Configuration (DSC) worden gecompileerd.
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
ms.date: 09/10/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d49ab32ace1ad0900c4867a41aba56900ef2bcaa
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54423405"
---
# <a name="compiling-dsc-configurations-in-azure-automation-state-configuration"></a>DSC-configuraties in Azure Automation-staat configuratie compileren

U kunt Desired State Configuration (DSC)-configuraties op twee manieren met Azure Automation-staat configuratie compileren: in Azure portal en met Windows PowerShell. De volgende tabel om te bepalen wanneer u welke methode op basis van de kenmerken van elk:

**Azure Portal**

- Eenvoudigste methode met interactieve gebruikersinterface
- Formulier voor eenvoudige parameterwaarden opgeven
- Taakstatus probleem gemakkelijk bijhouden
- Toegang is geverifieerd met Azure-aanmelding

**Windows PowerShell**

- Aanroepen vanuit de opdrachtregel met Windows PowerShell-cmdlets
- Kan worden opgenomen in geautomatiseerde oplossing met meerdere stappen
- Eenvoudige en complexe parameterwaarden opgeven
- Taakstatus bijhouden
- Clients die zijn vereist ter ondersteuning van PowerShell-cmdlets
- ConfigurationData doorgeven
- Configuraties die gebruikmaken van referenties compileren

Nadat u hebt besloten van een methode voor het compileren, gebruikt u de volgende procedures om te beginnen met het compileren van.

## <a name="compiling-a-dsc-configuration-with-the-azure-portal"></a>Compileren van een DSC-configuratie met de Azure-portal

1. Via uw Automation-account, klikt u op **State configuration (DSC)**.
1. Klik op de **configuraties** tabblad en klik vervolgens op de naam van de configuratie compileren.
1. Klik op **compileren**.
1. Als de configuratie geen parameters heeft, wordt u gevraagd om te bevestigen of u wilt dit compileren. Als de configuratie van de parameters, heeft de **configuratie compileren** blade wordt geopend, zodat u parameterwaarden kunt opgeven. Zie de volgende [ **eenvoudige Parameters** ](#basic-parameters) sectie voor meer informatie over parameters.
1. De **Compilatietaak** pagina wordt geopend zodat u de status van de Compilatietaak en de configuraties van knooppunten (MOF configuratiedocumenten) het veroorzaakt worden geplaatst op de Azure Automation-staat configuratie Pull-Server kunt bijhouden.

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a>Compileren van een DSC-configuratie met Windows PowerShell

U kunt [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) te compileren met Windows PowerShell. De volgende voorbeeldcode compileren van een DSC-configuratie met de naam begint **SampleConfig**.

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'
```

`Start-AzureRmAutomationDscCompilationJob` retourneert een taakobject compilatie die u gebruiken kunt voor het bijhouden van de status ervan. U kunt dit object compileren met [`Get-AzureRmAutomationDscCompilationJob`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob)
om te bepalen van de status van de Compilatietaak en [`Get-AzureRmAutomationDscCompilationJobOutput`](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput)
om weer te geven de-streams (uitvoer). De volgende voorbeeldcode wordt gestart compilatie van de **SampleConfig** configuratie, wacht u totdat deze is voltooid en wordt vervolgens de streams.

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'SampleConfig'

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a>Eenvoudige Parameters

Parameterdeclaratie in DSC-configuraties, met inbegrip van de parametertypen en properties, werkt hetzelfde als in Azure Automation-runbooks. Zie [een runbook starten in Azure Automation](automation-starting-a-runbook.md) voor meer informatie over runbook-parameters.

Het volgende voorbeeld worden twee parameters met de naam **FeatureName** en **IsPresent**om te bepalen van de waarden van eigenschappen in de **ParametersExample.sample** knooppunt de configuratie, is gegenereerd tijdens de compilatie.

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]
        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = 'Present'
    if($IsPresent -eq $false)
    {
        $EnsureString = 'Absent'
    }

    Node 'sample'
    {
        WindowsFeature ($FeatureName + 'Feature')
        {
            Ensure = $EnsureString
            Name   = $FeatureName
        }
    }
}
```

DSC-configuraties die gebruikmaken van eenvoudige parameters in de portal voor Azure Automation State Configuration of met Azure PowerShell, kunt u compileren:

### <a name="portal"></a>Portal

In de portal, kunt u parameterwaarden opgeven na het klikken op **compileren**.

![Parameters voor de configuratie compileren](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a>PowerShell

PowerShell is vereist voor parameters in een [hashtabel](/powershell/module/microsoft.powershell.core/about/about_hash_tables) waar de sleutel overeenkomt met de parameternaam en de waarde gelijk is aan de waarde van parameter.

```powershell
$Parameters = @{
    'FeatureName' = 'Web-Server'
    'IsPresent' = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ParametersExample' -Parameters $Parameters
```

Zie voor meer informatie over het PSCredentials doorgeven als parameters [Referentieassets](#credential-assets) hieronder.

## <a name="composite-resources"></a>Samengestelde Resources

**Samengestelde Resources** kunt u gebruikmaken van DSC-configuraties als ingesloten resources binnen een configuratie. Hiermee kunt u meerdere configuraties toepassen op één resource. Zie [samengestelde bronnen: Met behulp van een DSC-configuratie als een resource](/powershell/dsc/authoringresourcecomposite) voor meer informatie over **samengestelde Resources**.

> [!NOTE]
> Opdat **samengestelde Resources** correct compileren, moet u eerst ervoor zorgen dat alle DSC-Resources die afhankelijk van de samengestelde eerst in de opslagplaats voor Azure Automation-Account-Modules zijn geïnstalleerd of deze correct wordt niet geïmporteerd.

Toevoegen van een DSC **samengestelde Resource**, moet u de resource-module toevoegen aan een archief (* .zip). Ga naar de opslagplaats Modules op uw Azure Automation-Account. Klik vervolgens op de knop 'Een Module toevoegen'.

![Module toevoegen](./media/automation-dsc-compile/add_module.png)

Ga naar de map waar het archief zich bevindt. Selecteer het bestand en klik op OK.

![Selecteer de Module](./media/automation-dsc-compile/select_dscresource.png)

U gaat terug naar de map van de modules, waar u met de status van controleren kunt uw **samengestelde Resource** terwijl deze uitgepakt en wordt geregistreerd bij Azure Automation.

![Samengestelde resources importeren](./media/automation-dsc-compile/register_composite_resource.png)

Als de module is geregistreerd, u kunt vervolgens klikt u op te valideren dat de **samengestelde Resources** zijn nu beschikbaar om te worden gebruikt in een configuratie.

![Samengestelde resources valideren](./media/automation-dsc-compile/validate_composite_resource.png)

Vervolgens u roept de **samengestelde Resource** in uw configuratie als volgt te werk:

```powershell
Node ($AllNodes.Where{$_.Role -eq 'WebServer'}).NodeName
{
    DomainConfig myCompositeConfig
    {
        DomainName = $DomainName
        Admincreds = $Admincreds
    }

    PSWAWebServer InstallPSWAWebServer
    {
        DependsOn = '[DomainConfig]myCompositeConfig'
    }
}
```

## <a name="configurationdata"></a>ConfigurationData

**ConfigurationData** kunt u voor het scheiden van structurele van elke omgeving-specifieke configuratie tijdens het gebruik van PowerShell DSC-configuratie. Zie [scheiden 'Wat' uit 'Waar' in PowerShell DSC](https://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) voor meer informatie over **ConfigurationData**.

> [!NOTE]
> U kunt **ConfigurationData** bij het compileren in Azure Automation-staat configuratie met behulp van Azure PowerShell, maar niet in de Azure-portal.

Het volgende van de DSC-voorbeeldconfiguratie maakt gebruik van **ConfigurationData** via de **$ConfigurationData** en **$AllNodes** trefwoorden. U moet ook de [ **xWebAdministration** module](https://www.powershellgallery.com/packages/xWebAdministration/) voor dit voorbeeld:

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq 'WebServer'}.NodeName
    {
        xWebsite Site
        {
            Name         = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure       = 'Present'
        }
    }
}
```

De voorgaande DSC-configuratie met PowerShell, kunt u compileren. De volgende PowerShell toegevoegd configuraties met twee knooppunten met de Azure Automation-configuratie Pull-Server: **ConfigurationDataSample.MyVM1** en **ConfigurationDataSample.MyVM3**:

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = 'MyVM1'
            Role = 'WebServer'
        },
        @{
            NodeName = 'MyVM2'
            Role = 'SQLServer'
        },
        @{
            NodeName = 'MyVM3'
            Role = 'WebServer'
        }
    )

    NonNodeData = @{
        SomeMessage = 'I love Azure Automation State Configuration and DSC!'
    }
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'ConfigurationDataSample' -ConfigurationData $ConfigData
```

## <a name="assets"></a>Assets

Asset verwijzingen zijn hetzelfde als in runbooks en Azure Automation State Configuration. Zie het volgende voor meer informatie:

- [Certificaten](automation-certificates.md)
- [Verbindingen](automation-connections.md)
- [Referenties](automation-credentials.md)
- [Variabelen](automation-variables.md)

### <a name="credential-assets"></a>Referentieassets

DSC-configuraties in Azure Automation kunnen verwijzen naar Automation referentieassets met behulp van de `Get-AutomationPSCredential` cmdlet. Als een configuratie heeft een parameter waarvoor een **PSCredential** typt, en vervolgens kunt u de `Get-AutomationPSCredential` cmdlet doordat de naam van een Azure Automation-referentieasset worden doorgegeven aan de cmdlet voor het ophalen van de referentie. U kunt dit object vervolgens gebruiken voor het vereisen van de parameter de **PSCredential** object. De Azure Automation-referentieasset met die naam is achter de schermen, opgehaald en doorgegeven aan de configuratie. In het volgende voorbeeld ziet u dit in actie.

Referenties te houden beveiligd in knooppuntconfiguraties (MOF configuratiedocumenten) is vereist voor het versleutelen van de referenties in het knooppunt van het MOF-configuratiebestand. Echter, momenteel u moet op de hoogte PowerShell DSC is goed om referenties als tekst zonder opmaak output tijdens het genereren van de configuratie MOF-knooppunt, omdat PowerShell DSC niet weten dat Azure Automation wordt worden versleutelen van het hele MOF-bestand nadat de generatie via een Compilatietaak.

U kunt zien dat PowerShell DSC waarmee dit voor referenties op die klopt moeten worden gegenereerd als tekst zonder opmaak in de gegenereerde knooppuntconfiguratie MOF-bestanden met behulp van [ **ConfigurationData**](#configurationdata). U moet doorgeven `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** voor elk knooppunt-blok de naam die wordt weergegeven in de DSC-configuratie en maakt gebruik van referenties.

Het volgende voorbeeld toont een DSC-configuratie die gebruikmaakt van een Automation-referentieasset.

```powershell
Configuration CredentialSample
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    $Cred = Get-AutomationPSCredential 'SomeCredentialAsset'

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath      = '\\Server\share\path\file.ext'
            DestinationPath = 'C:\destinationPath'
            Credential      = $Cred
        }
    }
}
```

De voorgaande DSC-configuratie met PowerShell, kunt u compileren. De volgende PowerShell toegevoegd configuraties met twee knooppunten met de Azure Automation-configuratie Pull-Server: **CredentialSample.MyVM1** en **CredentialSample.MyVM2**.

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = '*'
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = 'MyVM1'
        },
        @{
            NodeName = 'MyVM2'
        }
    )
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'MyAutomationAccount' -ConfigurationName 'CredentialSample' -ConfigurationData $ConfigData
```

> [!NOTE]
> Als de compilatie is voltooid krijgt u mogelijk een foutbericht weergegeven: **De module 'Microsoft.PowerShell.Management' is niet geïmporteerd omdat de module 'Microsoft.PowerShell.Management' is al geïmporteerd.** Deze waarschuwing kan veilig worden genegeerd.

## <a name="importing-node-configurations"></a>Knooppuntconfiguraties importeren

U kunt ook knooppuntconfiguraties (MOF-bestanden) die zijn gecompileerd buiten Azure importeren. Een voordeel hiervan is dat de knooppuntconfiguraties kunnen worden ondertekend. Een ondertekende knooppuntconfiguratie is lokaal op een beheerde knooppunt gecontroleerd door de DSC-agent, waarbij u ervoor zorgt dat de configuratie wordt toegepast op het knooppunt afkomstig van een geautoriseerde bron is.

> [!NOTE]
> U kunt importeren ondertekend configuraties in uw Azure Automation-account, maar Azure Automation biedt momenteel geen ondersteuning ondertekende-configuraties compileren.

> [!NOTE]
> Een configuratiebestand van het knooppunt mag niet groter zijn dan 1 MB toe te staan dat moet worden geïmporteerd in Azure Automation zijn.

Zie voor meer informatie over hoe u zich knooppuntconfiguraties [verbeteringen in WMF 5.1 - configuratie van aanmelding en module](/powershell/wmf/5.1/dsc-improvements#dsc-module-and-configuration-signing-validations).

### <a name="importing-a-node-configuration-in-the-azure-portal"></a>Importeren van de configuratie van een knooppunt in de Azure-portal

1. Via uw Automation-account, klikt u op **State configuration (DSC)** onder **Configuratiebeheer**.
1. In de **State configuration (DSC)** pagina, klikt u op de **configuraties** tabblad en klik vervolgens op **+ toevoegen**.
1. In de **importeren** pagina, klikt u op het mappictogram naast de **configuratiebestand van het knooppunt** tekstvak om te bladeren naar een knooppunt-configuratiebestand (MOF) op uw lokale computer.

   ![Zoeken naar een lokaal bestand](./media/automation-dsc-compile/import-browse.png)

1. Voer een naam in de **configuratienaam** tekstvak. Deze naam moet overeenkomen met de naam van de configuratie van waaruit de knooppuntconfiguratie is gecompileerd.
1. Klik op **OK**.

### <a name="importing-a-node-configuration-with-powershell"></a>Importeren van de configuratie van een knooppunt met PowerShell

U kunt de [importeren AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) cmdlet voor het importeren van de knooppuntconfiguratie van een in uw automation-account.

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName 'MyAutomationAccount' -ResourceGroupName 'MyResourceGroup' -ConfigurationName 'MyNodeConfiguration' -Path 'C:\MyConfigurations\TestVM1.mof'
```

## <a name="next-steps"></a>Volgende stappen

- Als u wilt beginnen, Zie [aan de slag met Azure Automation State Configuration](automation-dsc-getting-started.md)
- Zie voor meer informatie over het DSC-configuraties compileren zodat u ze aan de doelknooppunten toewijzen kunt, [-configuraties in Azure Automation-staat configuratie compileren](automation-dsc-compile.md)
- Zie voor naslagdocumentatie voor PowerShell-cmdlet, [Statusconfiguratie van Azure Automation-cmdlets](/powershell/module/azurerm.automation/#automation)
- Zie voor informatie over de prijzen [Statusconfiguratie van Azure Automation-prijzen](https://azure.microsoft.com/pricing/details/automation/)
- Zie voor een voorbeeld van het gebruik van Azure Automation State Configuration in een pijplijn voor continue implementatie [continue implementatie met behulp van Azure Automation Statusconfiguratie- en Chocolatey](automation-dsc-cd-chocolatey.md)
