---
title: Compileren van configuraties in Azure Automation DSC | Microsoft Docs
description: In dit artikel wordt beschreven hoe Desired State Configuration (DSC)-configuraties voor Azure Automation worden gecompileerd.
services: automation
documentationcenter: na
author: eslesar
manager: carmonm
ms.assetid: 49f20b31-4fa5-4712-b1c7-8f4409f1aecc
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 02/07/2017
ms.author: magoedte; eslesar
ms.openlocfilehash: 1aadd604e676659475f00760af3b0bdfb13a4792
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
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

Nadat u hebt besloten een compilatie-methode, kunt u de desbetreffende procedures hieronder om te beginnen met het compileren van volgen.

## <a name="compiling-a-dsc-configuration-with-the-azure-portal"></a>Compileren van een DSC-configuratie met de Azure-portal

1. Van uw Automation-account, klikt u op **DSC-configuraties**.
2. Klik op een configuratie om de blade te openen.
3. Klik op **compileren**.
4. Als de configuratie geen parameters heeft, wordt u gevraagd om te bevestigen of u wilt compileren. Als de configuratie van de parameters, heeft de **configuratie compileren** blade geopend zodat u parameterwaarden kan opgeven. Zie de [ **fundamentele Parameters** ](#basic-parameters) sectie hieronder voor meer informatie over parameters.
5. De **Compilatietaak** blade wordt geopend zodat u de status van de Compilatietaak en de knooppuntconfiguraties (MOF configuratiedocumenten) het veroorzaakt worden geplaatst op de Azure Automation DSC-Pull-Server kunt bijhouden.

## <a name="compiling-a-dsc-configuration-with-windows-powershell"></a>Compileren van een DSC-configuratie met Windows PowerShell

U kunt [ `Start-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/start-azurermautomationdsccompilationjob) te compileren met Windows PowerShell starten. De volgende voorbeeldcode wordt gestart compilatie van een DSC-configuratie aangeroepen **SampleConfig**.

```powershell
Start-AzureRmAutomationDscCompilationJob -ResourceGroupName "MyResourceGroup" -AutomationAccountName "MyAutomationAccount" -ConfigurationName "SampleConfig"
```

`Start-AzureRmAutomationDscCompilationJob`retourneert een taakobject compilatie waarmee u kunt de status ervan bijhouden. U kunt dit object compilatie met [ `Get-AzureRmAutomationDscCompilationJob` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjob) om te bepalen van de status van de Compilatietaak en [ `Get-AzureRmAutomationDscCompilationJobOutput` ](/powershell/module/azurerm.automation/get-azurermautomationdsccompilationjoboutput) om weer te geven van de stromen (uitvoer). De volgende voorbeeldcode wordt gestart compilatie van de **SampleConfig** configuratie, wacht totdat deze is voltooid en wordt vervolgens de stromen.

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

## <a name="configurationdata"></a>ConfigurationData
**ConfigurationData** kunt u afzonderlijke structurele configuratie uit een specifieke configuratie van de omgeving tijdens het gebruik van PowerShell DSC. Zie [scheiden 'Wat' op 'Waar' in PowerShell DSC](http://blogs.msdn.com/b/powershell/archive/2014/01/09/continuous-deployment-using-dsc-with-minimal-change.aspx) voor meer informatie over **ConfigurationData**.

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

U kunt de DSC-configuratie hierboven met PowerShell compileren. De onderstaande PowerShell voegt u twee knooppuntconfiguraties toe aan de Azure Automation DSC-Pull-Server: **ConfigurationDataSample.MyVM1** en **ConfigurationDataSample.MyVM3**:

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

Terwijl in Azure Automation DSC-configuraties kunnen verwijzen naar referentieassets met **Get-AzureRmAutomationCredential**, referentieassets kunnen ook worden doorgegeven via parameters, indien gewenst. Als een parameter van het duurt een configuratie voordat **PSCredential** typt, moet u de naam van een Azure Automation-referentieasset doorgeven als waarde van de parameter in plaats van een PSCredential-object. Achter de schermen worden wordt de Azure Automation-referentieasset met deze naam opgehaald en doorgegeven aan de configuratie.

Referenties houden beveiligd in knooppuntconfiguraties (MOF configuratiedocumenten) is vereist voor het versleutelen van de referenties in het knooppunt configuratie MOF-bestand. Azure Automation hierbij nog een stapje verder neemt en versleutelt het hele MOF-bestand. Echter, momenteel u moet hoogte PowerShell DSC dat dit klopt om referenties als tekst zonder opmaak tijdens het genereren van het knooppunt configuratie MOF output omdat PowerShell DSC niet weet dat Azure Automation wordt worden versleutelen van het hele MOF-bestand na de generatie via een Compilatietaak.

U kunt zien PowerShell DSC die dit voor referenties klopt output in tekst zonder opmaak in de gegenereerde knooppuntconfiguratie MOF-bestanden met [ **ConfigurationData**](#configurationdata). U moet doorgeven `PSDscAllowPlainTextPassword = $true` via **ConfigurationData** voor elk knooppunt-blok-naam die wordt weergegeven in de DSC-configuratie en de referenties worden gebruikt.

Het volgende voorbeeld toont een DSC-configuratie die gebruikmaakt van een Automation-referentieasset.

```powershell
Configuration CredentialSample
{
    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -AutomationAccountName "AutomationAcct" -Name "SomeCredentialAsset"

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

U kunt de DSC-configuratie hierboven met PowerShell compileren. De onderstaande PowerShell voegt u twee knooppuntconfiguraties toe aan de Azure Automation DSC-Pull-Server: **CredentialSample.MyVM1** en **CredentialSample.MyVM2**.

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

## <a name="importing-node-configurations"></a>Knooppuntconfiguraties importeren

U kunt ook knooppunt configuratons (MOF-bestanden) die u hebt gecompileerd buiten Azure importeren. Een voordeel hiervan is dat confiturations knooppunt kan worden ondertekend.
Een ondertekende knooppuntconfiguratie is lokaal op een beheerde knooppunt gecontroleerd door de DSC-agent, waarbij u ervoor zorgt dat de configuratie die wordt toegepast op het knooppunt van een gemachtigde bron afkomstig is.

> [!NOTE]
> U kunt importeren gebruiken configuraties in uw Azure Automation-account is ondertekend, maar Azure Automation biedt momenteel geen ondersteuning voor het compileren van ondertekende configuraties.

> [!NOTE]
> Een configuratiebestand knooppunt mag niet groter zijn dan 1 MB te laten worden geïmporteerd in Azure Automation zijn.

U kunt informatie over het ondertekenen van knooppuntconfiguraties op https://msdn.microsoft.com/en-us/powershell/wmf/5.1/dsc-improvements#how-to-sign-configuration-and-module.

### <a name="importing-a-node-configuration-in-the-azure-portal"></a>Importeren van de configuratie van een knooppunt in de Azure portal

1. Van uw Automation-account, klikt u op **DSC-knooppuntconfiguraties**.

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



