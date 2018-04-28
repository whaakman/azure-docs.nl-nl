---
title: Compileren van configuraties in Azure Automation DSC
description: In dit artikel wordt beschreven hoe Desired State Configuration (DSC)-configuraties voor Azure Automation worden gecompileerd.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: b085a75acc4d7744587f0c93482435b5b8c28fc2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="compiling-configurations-in-azure-automation-dsc"></a>Compileren van configuraties in Azure Automation DSC

U kunt configuraties Desired State Configuration (DSC) op twee manieren met Azure Automation compileren: in de Azure-portal en met Windows PowerShell. De volgende tabel kunt u bepalen wanneer de methode die op basis van de kenmerken van elk gebruiken:

### <a name="azure-portal"></a>Azure Portal

* Eenvoudigste methode met interactieve gebruikersinterface
* Formulier eenvoudige parameterwaarden opgeven
* Eenvoudig bijhouden taakstatus
* Toegang met aanmelding bij Azure is geverifieerd

### <a name="windows-powershell"></a>Windows Powershell

* Aanroepen vanuit de opdrachtregel met Windows PowerShell-cmdlets
* Kan worden opgenomen in een geautomatiseerde oplossing met meerdere stappen
* Eenvoudige en complexe parameterwaarden opgeven
* Taakstatus bijhouden
* Client vereist ter ondersteuning van PowerShell-cmdlets
* ConfigurationData doorgeven
* Configuraties die gebruikmaken van referenties compileren

Zodra u hebt besloten een methode compilatie, gebruik de volgende procedures om te starten compileren.

## <a name="compiling-a-dsc-configuration-with-the-azure-portal"></a>Compileren van een DSC-configuratie met de Azure-portal

1. Van uw Automation-account, klikt u op **DSC-configuraties**.
2. Klik op een configuratie om de blade te openen.
3. Klik op **compileren**.
4. Als de configuratie geen parameters heeft, wordt u gevraagd om te bevestigen of u wilt compileren. Als de configuratie van de parameters, heeft de **configuratie compileren** blade wordt geopend zodat u parameterwaarden kan opgeven. Zie de volgende [ **fundamentele Parameters** ](#basic-parameters) sectie voor meer informatie over parameters.
5. De **Compilatietaak** blade wordt geopend zodat u de status van de Compilatietaak en de knooppuntconfiguraties (MOF configuratiedocumenten) het veroorzaakt worden geplaatst op de Azure Automation DSC-Pull-Server kunt bijhouden.

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a>Compileren van een DSC-configuratie met Windows PowerShell

U kunt [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) te compileren met Windows PowerShell starten. De volgende voorbeeldcode wordt gestart compilatie van een DSC-configuratie aangeroepen **SampleConfig**.

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

`Start-AzureRmAutomationDscCompilationJob` retourneert een taakobject compilatie waarmee u kunt de status ervan bijhouden. U kunt dit object compilatie met [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) om te bepalen van de status van de Compilatietaak en [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) om weer te geven van de stromen (uitvoer). De volgende voorbeeldcode wordt gestart compilatie van de **SampleConfig** configuratie, wacht totdat deze is voltooid en wordt vervolgens de stromen.

```powershell
$CompilationJob = Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"

while($CompilationJob.EndTime –eq $null -and $CompilationJob.Exception –eq $null)
{
    $CompilationJob = $CompilationJob | Get-AzureRmAutomationDscCompilationJob
    Start-Sleep -Seconds 3
}

$CompilationJob | Get-AzureRmAutomationDscCompilationJobOutput –Stream Any
```

## <a name="basic-parameters"></a>Basic-Parameters
Parameterdeclaratie in DSC-configuraties, inclusief parametertypen en eigenschappen werkt hetzelfde als in Azure Automation-runbooks. Zie [een runbook starten in Azure Automation](automation-starting-a-runbook.md) voor meer informatie over runbook-parameters.

Het volgende voorbeeld worden twee parameters aangeroepen **FeatureName** en **IsPresent**om te bepalen van de waarden van eigenschappen in de **ParametersExample.sample** knooppuntconfiguratie, gegenereerd tijdens de compilatie.

```powershell
Configuration ParametersExample
{
    param(
        [Parameter(Mandatory=$true)]

        [string] $FeatureName,

        [Parameter(Mandatory=$true)]
        [boolean] $IsPresent
    )

    $EnsureString = "Present"
    if($IsPresent -eq $false)
    {
        $EnsureString = "Absent"
    }

    Node "sample"
    {
        WindowsFeature ($FeatureName + "Feature")
        {
            Ensure = $EnsureString
            Name = $FeatureName
        }
    }
}
```

U kunt compileren DSC-configuraties die gebruikmaken van eenvoudige parameters in de Azure Automation DSC-portal of met Azure PowerShell:

### <a name="portal"></a>Portal

In de portal kunt u parameterwaarden opgeven wanneer u op **compileren**.

![alternatieve tekst](./media/automation-dsc-compile/DSC_compiling_1.png)

### <a name="powershell"></a>PowerShell

PowerShell-parameters in vereist een [hashtabel](http://technet.microsoft.com/library/hh847780.aspx) waarbij de sleutel komt overeen met de parameternaam van de en de waarde gelijk is aan de waarde van parameter.

```powershell
$Parameters = @{
    "FeatureName" = "Web-Server"
    "IsPresent" = $False
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ParametersExample" -Parameters $Parameters
```

Zie voor meer informatie over PSCredentials doorgeeft als parameters <a href="#credential-assets"> **Referentieassets** </a> hieronder.

## <a name="composite-resources"></a>Samengestelde bronnen

**Samengestelde bronnen** kunt u DSC-configuraties gebruiken als ingesloten resources binnen een configuratie. Hiermee kunt u meerdere configuraties toepassen op één resource. Zie [samengestelde bronnen: met een DSC-configuratie als een resource](https://docs.microsoft.com/powershell/dsc/authoringresourcecomposite) voor meer informatie over **samengestelde bronnen**

> [!NOTE]
> Opdat **samengestelde bronnen** correct compileren, moet u eerst ervoor zorgen dat eventuele DSC-Resources die afhankelijk is van de samengestelde waarde eerst in de opslagplaats Azure Automation-Account-Modules zijn geïnstalleerd of deze correct wordt niet geïmporteerd.

Toevoegen van een DSC **samengestelde bron**, moet u de module resource toevoegen aan een archief (* .zip). Ga naar de opslagplaats Modules op uw Azure Automation-Account. Klik vervolgens op de knop 'Add een Module'.

![Module toevoegen](./media/automation-dsc-compile/add_module.png)

Ga naar de map waar het archief zich bevindt. Selecteer het archiefbestand, en klik op OK.

![Selecteer de Module](./media/automation-dsc-compile/select_dscresource.png)

Gaat u terug naar de map modules, waar u met de status van controleren kunt uw **samengestelde bron** terwijl het uitgepakt en wordt geregistreerd bij Azure Automation.

![Samengestelde bron importeren](./media/automation-dsc-compile/register_composite_resource.png)

Wanneer de module is geregistreerd, u kunt vervolgens klikken om te valideren dat de **samengestelde bronnen** zijn nu beschikbaar moet worden gebruikt in een configuratie.

![Samengestelde bron valideren](./media/automation-dsc-compile/validate_composite_resource.png)

Vervolgens u roept de **samengestelde bron** in uw configuratie als volgt te werk:

```powershell

    Node ($AllNodes.Where{$_.Role -eq "WebServer"}).NodeName
    {
            
            JoinDomain DomainJoin
            {
                DomainName = $DomainName
                Admincreds = $Admincreds
            }

            PSWAWebServer InstallPSWAWebServer
            {
                DependsOn = "[JoinDomain]DomainJoin"
            }        
    }

```

## <a name="configurationdata"></a>ConfigurationData
**ConfigurationData** kunt u afzonderlijke structurele configuratie van elke omgeving-specifieke configuratie tijdens het gebruik van PowerShell DSC. Zie [scheiden 'Wat' op 'Waar' in PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) voor meer informatie over **ConfigurationData**.

> [!NOTE]
> U kunt **ConfigurationData** bij het compileren van in Azure Automation DSC met Azure PowerShell, maar niet in de Azure-portal.

Het volgende DSC-voorbeeldconfiguratie gebruikt **ConfigurationData** via de **$ConfigurationData** en **$AllNodes** trefwoorden. U moet ook de [ **xWebAdministration** module](https://www.powershellgallery.com/packages/xWebAdministration/) voor dit voorbeeld:

```powershell
Configuration ConfigurationDataSample
{
    Import-DscResource -ModuleName xWebAdministration -Name MSFT_xWebsite

    Write-Verbose $ConfigurationData.NonNodeData.SomeMessage

    Node $AllNodes.Where{$_.Role -eq "WebServer"}.NodeName
    {
        xWebsite Site
        {
            Name = $Node.SiteName
            PhysicalPath = $Node.SiteContents
            Ensure   = "Present"
        }
    }
}
```

U kunt de voorgaande DSC-configuratie met PowerShell compileren. De volgende PowerShell voegt twee knooppuntconfiguraties aan het Azure Automation DSC-Pull-Server: **ConfigurationDataSample.MyVM1** en **ConfigurationDataSample.MyVM3**:

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "MyVM1"
            Role = "WebServer"
        },
        @{
            NodeName = "MyVM2"
            Role = "SQLServer"
        },
        @{
            NodeName = "MyVM3"
            Role = "WebServer"
        }
    )

    NonNodeData = @{
        SomeMessage = "I love Azure Automation DSC!"
    }
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "ConfigurationDataSample" -ConfigurationData $ConfigData
```

## <a name="assets"></a>Assets

Asset-verwijzingen zijn hetzelfde als in Azure Automation DSC-configuraties en runbooks. Zie de volgende onderwerpen voor meer informatie:

* [Certificaten](automation-certificates.md)
* [Verbindingen](automation-connections.md)
* [Referenties](automation-credentials.md)
* [Variabelen](automation-variables.md)

### <a name="credential-assets"></a>Referentieassets

Terwijl in Azure Automation DSC-configuraties kunnen verwijzen naar referentieassets met **Get-AutomationPSCredential**, referentieassets kunnen ook worden doorgegeven via parameters, indien gewenst. Als een parameter van het duurt een configuratie voordat **PSCredential** typt, moet u de naam van een Azure Automation-referentieasset doorgeven als waarde van de parameter in plaats van een PSCredential-object. Achter de schermen worden de Azure Automation-referentieasset met die naam opgehaald en doorgegeven aan de configuratie.

Referenties houden beveiligd in knooppuntconfiguraties (MOF configuratiedocumenten) is vereist voor het versleutelen van de referenties in het knooppunt configuratie MOF-bestand. Azure Automation hierbij nog een stapje verder neemt en versleutelt het hele MOF-bestand. Echter, momenteel u moet hoogte PowerShell DSC dat dit klopt om referenties als tekst zonder opmaak tijdens het genereren van het knooppunt configuratie MOF output omdat PowerShell DSC niet weet dat Azure Automation wordt worden versleutelen van het hele MOF-bestand na de generatie via een Compilatietaak.

U kunt zien PowerShell DSC die dit voor referenties klopt output in tekst zonder opmaak in de gegenereerde knooppuntconfiguratie MOF-bestanden met [ **ConfigurationData**](#configurationdata). U moet doorgeven `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** voor elk knooppunt-blok-naam die wordt weergegeven in de DSC-configuratie en de referenties worden gebruikt.

Het volgende voorbeeld toont een DSC-configuratie die gebruikmaakt van een Automation-referentieasset.

```powershell
Configuration CredentialSample
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration
    $Cred = Get-AutomationPSCredential "SomeCredentialAsset"

    Node $AllNodes.NodeName
    {
        File ExampleFile
        {
            SourcePath = "\\Server\share\path\file.ext"
            DestinationPath = "C:\destinationPath"
            Credential = $Cred
        }
    }
}
```

U kunt de voorgaande DSC-configuratie met PowerShell compileren. De volgende PowerShell voegt twee knooppuntconfiguraties aan het Azure Automation DSC-Pull-Server: **CredentialSample.MyVM1** en **CredentialSample.MyVM2**.

```powershell
$ConfigData = @{
    AllNodes = @(
        @{
            NodeName = "*"
            PSDscAllowPlainTextPassword = $True
        },
        @{
            NodeName = "MyVM1"
        },
        @{
            NodeName = "MyVM2"
        }
    )
}

Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "CredentialSample" -ConfigurationData $ConfigData
```

> [!NOTE]
> Wanneer de compilatie is voltooid krijgt u een foutbericht waarin wordt gemeld: **de module 'Microsoft.PowerShell.Management' is niet geïmporteerd omdat de module 'Microsoft.PowerShell.Management' is al geïmporteerd.** Deze waarschuwing kan veilig worden genegeerd.

## <a name="importing-node-configurations"></a>Knooppuntconfiguraties importeren

U kunt ook knooppuntconfiguraties (MOF-bestanden) die u hebt gecompileerd buiten Azure importeren. Een voordeel hiervan is dat de knooppuntconfiguraties kunnen worden ondertekend.
Een ondertekende knooppuntconfiguratie is lokaal op een beheerde knooppunt gecontroleerd door de DSC-agent, waarbij u ervoor zorgt dat de configuratie die wordt toegepast op het knooppunt van een gemachtigde bron afkomstig is.

> [!NOTE]
> U kunt importeren gebruiken configuraties in uw Azure Automation-account is ondertekend, maar Azure Automation biedt momenteel geen ondersteuning voor het compileren van ondertekende configuraties.

> [!NOTE]
> Een configuratiebestand knooppunt mag niet groter zijn dan 1 MB te laten worden geïmporteerd in Azure Automation zijn.

Leert u hoe u aan te melden knooppuntconfiguraties op https://msdn.microsoft.com/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.

### <a name="importing-a-node-configuration-in-the-azure-portal"></a>Importeren van de configuratie van een knooppunt in de Azure portal

1. Van uw Automation-account, klikt u op **DSC-knooppuntconfiguraties** onder **Configuratiebeheer**.

    ![DSC-knooppuntconfiguraties](./media/automation-dsc-compile/node-config.png)
2. In de **DSC-knooppuntconfiguraties** blade, klikt u op **toevoegen van een NodeConfiguration**.
3. In de **importeren** blade, klik op het pictogram naast de **configuratiebestand knooppunt** textbox om te bladeren naar een knooppunt-configuratiebestand (MOF) op de lokale computer.

    ![Zoeken naar een lokaal bestand](./media/automation-dsc-compile/import-browse.png)
4. Voer een naam in de **configuratienaam** textbox. Deze naam moet overeenkomen met de naam van de configuratie van waaruit de knooppuntconfiguratie is gecompileerd.
5. Klik op **OK**.

### <a name="importing-a-node-configuration-with-powershell"></a>Importeren van de configuratie van een knooppunt met PowerShell

U kunt de [importeren AzureRmAutomationDscNodeConfiguration](/powershell/module/azurerm.automation/import-azurermautomationdscnodeconfiguration) cmdlet voor het importeren van de knooppuntconfiguratie van een in uw automation-account.

```powershell
Import-AzureRmAutomationDscNodeConfiguration -AutomationAccountName "MyAutomationAccount" -ResourceGroupName "MyResourceGroup" -ConfigurationName "MyNodeConfiguration" -Path "C:\MyConfigurations\TestVM1.mof"
```



