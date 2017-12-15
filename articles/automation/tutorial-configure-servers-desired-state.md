---
title: Servers een gewenste status configureren en beheren van afwijking met Azure Automation | Microsoft Docs
description: Zelfstudie - server met Azure Automation DSC-configuraties beheren
services: automation
documentationcenter: automation
author: georgewallace
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: automation
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: gwallace
ms.custom: 
ms.openlocfilehash: c510b2244dfa85b12ed08cad9dbab75067ebe41a
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>Servers een gewenste status configureren en beheren van afwijking

Azure Automation gewenst State Configuration (DSC) kunt u configuraties voor uw servers opgeven en ervoor zorgen dat die servers in de opgegeven status gedurende een bepaalde periode.



> [!div class="checklist"]
> * Ingebouwde een virtuele machine kan worden beheerd door Azure Automation DSC
> * Een configuratie uploaden naar Azure Automation
> * Een configuratie in de configuratie van een knooppunt compileren
> * De configuratie van een knooppunt toewijzen aan een beheerde knooppunt
> * Controleer de status van naleving van een beheerde knooppunt

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt voltooid, moet u het:

* Een Azure Automation-account. Zie [Azure Uitvoeren-als-account](automation-sec-configure-azure-runas-account.md) voor instructies over het maken van een Azure Automation Uitvoeren-als-account.
* Een Azure Resource Manager VM (niet-klassieke) met Windows Server 2008 R2 of hoger. Zie [Uw eerste virtuele Windows-machine maken met behulp van Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor instructies voor het maken van een VM
* Azure PowerShell-moduleversie 3,6 of later. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).
* Als u bekend bent met DSC. Zie voor meer informatie over DSC [Windows PowerShell Desired Configuration overzicht](https://docs.microsoft.com/powershell/dsc/overview)

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Maken en uploaden van een configuratie voor Azure Automation

Voor deze zelfstudie gebruiken we een eenvoudige DSC-configuratie die ervoor zorgt dat IIS is geïnstalleerd op de virtuele machine.

Zie voor meer informatie over DSC-configuraties [DSC-configuraties](https://docs.microsoft.com/powershell/dsc/configurations).

Typ het volgende in een teksteditor en sla het bestand lokaal als `TestConfig.ps1`.

```powershell
configuration TestConfig {

   Node WebServer {

      WindowsFeature IIS {
         Ensure               = 'Present'
         Name                 = 'Web-Server'
         IncludeAllSubFeature = $true
      }
   }
}
```

Roep de `Import-AzureRmAutomationDscConfiguration` cmdlet voor het uploaden van de configuratie in uw Automation-account:

```powershell
 Import-AzureRmAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>Een configuratie in de configuratie van een knooppunt compileren

Een DSC-configuratie moet worden gecompileerd naar een knooppuntconfiguratie voordat deze kan worden toegewezen aan een knooppunt.

Zie voor meer informatie over het compileren van configuraties [DSC-configuraties](https://docs.microsoft.com/powershell/dsc/configurations).

Roep de `Start-AzureRmAutomationDscCompilationJob` cmdlet voor het compileren van de `TestConfig` configuratie in de configuratie van een knooppunt:

```powershell
Start-AzureRmAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

Hiermee maakt u de knooppuntconfiguratie van een met de naam `TestConfig.WebServer` in uw Automation-account.

## <a name="register-a-vm-to-be-managed-by-dsc"></a>Registreren van een virtuele machine kan worden beheerd door DSC

Azure Automation DSC kunt u virtuele Azure-machines (Classic en Resource Manager), lokale virtuele machines Linux machines, AWS virtuele machines en fysieke machines lokale beheren. In dit onderwerp wordt uitgelegd hoe u alleen Azure Resource Manager virtuele machines te registreren.
Zie voor meer informatie over het registreren van andere typen machines [machines voorbereiden voor beheer door Azure Automation DSC](automation-dsc-onboarding.md).

Roep de `Register-AzureRmAutomationDscNode` cmdlet registreren van uw virtuele machine met Azure Automation DSC.

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName "MyResourceGroup" -AutomationAccountName "myAutomationAccount" -AzureVMName "DscVm"
```

Hiermee wordt de opgegeven virtuele machine als een DSC-knooppunt in uw Azure Automation-account.

### <a name="specify-configuration-mode-settings"></a>Configuratie-instellingen voor de modus opgeven

Wanneer u een virtuele machine als een beheerde knooppunt registreert, kunt u ook de eigenschappen van de configuratie van opgeven.
U kunt bijvoorbeeld opgeven dat de status van de machine is slechts één keer worden toegepast (DSC probeert niet toepassen van de configuratie na de eerste controle) door te geven `ApplyOnly` als de waarde van de **ConfigurationMode** eigenschap :

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName "DscVm" -ConfigurationMode 'ApplyOnly'
```

U kunt ook opgeven hoe vaak DSC de configuratiestatus gecontroleerd met behulp van de **ConfigurationModeFrequencyMins** eigenschap:

```powershell
# Run a DSC check every 60 minutes
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName "DscVm" -ConfigurationModeFrequencyMins 60
```

Zie voor meer informatie over het instellen van configuratie-eigenschappen voor een beheerde knooppunt [registreren AzureRmAutomationDscNode](https://docs.microsoft.com/powershell/module/azurerm.automation/register-azurermautomationdscnode?view=azurermps-4.3.1&viewFallbackFrom=azurermps-4.2.0).

Zie voor meer informatie over DSC-configuratie-instellingen, [configureren van de lokale Configuration Manager](https://docs.microsoft.com/powershell/dsc/metaconfig).

## <a name="assign-a-node-configuration-to-a-managed-node"></a>De configuratie van een knooppunt toewijzen aan een beheerde knooppunt

Nu toewijzen we de gecompileerde knooppuntconfiguratie aan de virtuele machine wilt configureren.

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -Id $node.Id
```

Hiermee wijst u de knooppuntconfiguratie met de naam toe `TestConfig.WebServer` geregistreerde DSC-knooppunt met de naam `DscVm`.
Standaard wordt de DSC-knooppunt gecontroleerd op naleving van de knooppuntconfiguratie elke 30 minuten.
Zie voor meer informatie over het wijzigen van het controle-interval voor naleving [configureren van de lokale Configuration Manager](https://docs.microsoft.com/PowerShell/DSC/metaConfig)

## <a name="check-the-compliance-status-of-a-managed-node"></a>Controleer de status van naleving van een beheerde knooppunt

U kunt rapporten over de status van naleving van een DSC-knooppunt ophalen door het aanroepen van de `Get-AzureRmAutomationDscNodeReport` cmdlet:

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports = Get-AzureRmAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Id $node.Id

# Display the most recent report
$reports[0]
```

## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie over hoe voor vrijgeven knooppunten moeten worden beheerd met Azure Automation DSC zien [machines voorbereiden voor beheer door Azure Automation DSC](automation-dsc-onboarding.md)
* Zie voor meer informatie over het gebruik van de Azure-portal voor Automation DSC, [aan de slag met Azure Automation DSC](automation-dsc-getting-started.md)
* Zie voor meer informatie over het compileren van DSC-configuraties, zodat u deze aan de doelknooppunten toewijzen kunt, [compileren van configuraties in Azure Automation DSC](automation-dsc-compile.md)
* Zie voor referentiemateriaal voor PowerShell-cmdlet voor Azure Automation DSC, [Azure Automation DSC-cmdlets](/powershell/module/azurerm.automation/#automation)
* Zie voor informatie over prijzen, [prijzen van Azure Automation DSC](https://azure.microsoft.com/pricing/details/automation/)
* Zie voor een voorbeeld van het gebruik van Azure Automation DSC in een pijplijn continue implementatie [continue implementatie voor IaaS VM's met behulp van Azure Automation DSC en Chocolatey](automation-dsc-cd-chocolatey.md)
