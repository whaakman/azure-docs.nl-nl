---
title: Servers configureren met een gewenste status en afwijkingen beheren met Azure Automation
description: Zelfstudie - server-configuraties met Azure Automation State Configuration beheren
services: automation
ms.service: automation
ms.component: dsc
author: bobbytreed
ms.author: robreed
manager: carmonm
ms.topic: conceptual
ms.date: 08/08/2018
ms.openlocfilehash: 000875e4e591bcfe94ec99f8b16c8ec40bf52cf4
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45629879"
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>Servers configureren met een gewenste status en afwijking beheren

Statusconfiguratie van Azure Automation kunt u configuraties voor uw servers opgeven en zorg ervoor dat deze servers bevinden zich in de opgegeven status na verloop van tijd.

> [!div class="checklist"]
> - Onboarding een virtuele machine kan worden beheerd door Azure Automation DSC
> - Een configuratie uploaden naar Azure Automation
> - Een configuratie in een knooppuntconfiguratie compileren
> - Een knooppuntconfiguratie toewijzen aan een beheerd knooppunt
> - Controleer de status van naleving van een beheerde knooppunt

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

- Een Azure Automation-account. Zie [Azure Uitvoeren-als-account](automation-sec-configure-azure-runas-account.md) voor instructies over het maken van een Azure Automation Uitvoeren-als-account.
- Een Azure Resource Manager VM (niet klassiek) met Windows Server 2008 R2 of hoger. Zie [Uw eerste virtuele Windows-machine maken met behulp van Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md) voor instructies voor het maken van een VM
- Azure PowerShell-moduleversie 3.6 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).
- Als u bekend bent met Desired State Configuration (DSC). Zie voor meer informatie over DSC [Windows PowerShell Desired State Configuration-overzicht](https://docs.microsoft.com/powershell/dsc/overview)

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Connect-AzureRmAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Maken en uploaden van een configuratie voor Azure Automation

Voor deze zelfstudie gebruiken we een eenvoudige DSC-configuratie die ervoor zorgt dat IIS is geïnstalleerd op de virtuele machine.

Zie [DSC-configuraties](/powershell/dsc/configurations) voor meer informatie over DSC-configuraties.

Typ het volgende in een teksteditor en sla het bestand lokaal op als `TestConfig.ps1`.

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

## <a name="compile-a-configuration-into-a-node-configuration"></a>Een configuratie in een knooppuntconfiguratie compileren

Een DSC-configuratie moet worden gecompileerd in de configuratie van een knooppunt voordat deze kan worden toegewezen aan een knooppunt.

Zie voor meer informatie over het compileren van configuraties [DSC-configuraties](/powershell/dsc/configurations).

Roep de `Start-AzureRmAutomationDscCompilationJob` cmdlet voor het compileren van de `TestConfig` configuratie in de configuratie van een knooppunt:

```powershell
Start-AzureRmAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

Hiermee maakt u de knooppuntconfiguratie van een met de naam `TestConfig.WebServer` in uw Automation-account.

## <a name="register-a-vm-to-be-managed-by-state-configuration"></a>Registreren van een virtuele machine kan worden beheerd door de configuratie van status

U kunt Azure Automation State Configuration gebruiken voor het beheren van Azure-VM's (zowel klassieke als Resource Manager), on-premises VM's, Linux machines, AWS-VM's en on-premises fysieke computers. In dit onderwerp behandelen we alleen Azure Resource Manager-VM's te registreren. Zie voor meer informatie over het registreren van andere typen machines [Onboarding van machines voor beheer met Azure Automation State Configuration](automation-dsc-onboarding.md).

Roep de `Register-AzureRmAutomationDscNode` cmdlet voor het registreren van uw virtuele machine met Azure Automation State Configuration.

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm'
```

Dit wordt de opgegeven virtuele machine geregistreerd als een beheerde knooppunt in de configuratie van de status.

### <a name="specify-configuration-mode-settings"></a>Configuratie-instellingen voor de modus opgeven

Als u een virtuele machine als een beheerde knooppunt registreert, kunt u ook de eigenschappen van de configuratie opgeven. U kunt bijvoorbeeld opgeven dat de status van de machine is slechts één keer worden toegepast (DSC probeert niet om toe te passen van de configuratie na de eerste controle) door op te geven `ApplyOnly` als de waarde van de **ConfigurationMode** eigenschap :

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationMode 'ApplyOnly'
```

U kunt ook opgeven hoe vaak DSC controleert of de status van de configuratie met behulp van de **ConfigurationModeFrequencyMins** eigenschap:

```powershell
# Run a DSC check every 60 minutes
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationModeFrequencyMins 60
```

Zie voor meer informatie over het instellen van configuratie-eigenschappen voor een beheerd knooppunt [registreren AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode).

Zie voor meer informatie over DSC-configuratie-instellingen, [de Local Configuration Manager configureren](/powershell/dsc/metaconfig).

## <a name="assign-a-node-configuration-to-a-managed-node"></a>Een knooppuntconfiguratie toewijzen aan een beheerd knooppunt

Nu kunnen we de gecompileerde knooppuntconfiguratie toewijzen aan de virtuele machine die we willen configureren.

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -Id $node.Id
```

Hiermee wordt de configuratie van de knooppunten met de naam toegewezen `TestConfig.WebServer` naar het geregistreerde DSC-knooppunt met de naam `DscVm`.
De DSC-knooppunt is standaard ingeschakeld voor naleving van de knooppuntconfiguratie elke 30 minuten.
Zie voor meer informatie over het wijzigen van het interval voor controle op naleving [de Local Configuration Manager configureren](/PowerShell/DSC/metaConfig).

## <a name="check-the-compliance-status-of-a-managed-node"></a>Controleer de status van naleving van een beheerde knooppunt

U kunt rapporten over de status van naleving van een beheerde knooppunt krijgen door het aanroepen van de `Get-AzureRmAutomationDscNodeReport` cmdlet:

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports = Get-AzureRmAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Id $node.Id

# Display the most recent report
$reports[0]
```

## <a name="next-steps"></a>Volgende stappen

- Als u wilt beginnen, Zie [aan de slag met Azure Automation State Configuration](automation-dsc-getting-started.md)
- Voor meer informatie over hoe voor onboarding knooppunten, Zie [Onboarding van machines voor beheer met Azure Automation State Configuration](automation-dsc-onboarding.md)
- Zie voor meer informatie over het DSC-configuraties compileren zodat u ze aan de doelknooppunten toewijzen kunt, [-configuraties in Azure Automation-staat configuratie compileren](automation-dsc-compile.md)
- Zie voor naslagdocumentatie voor PowerShell-cmdlet, [Statusconfiguratie van Azure Automation-cmdlets](/powershell/module/azurerm.automation/#automation)
- Zie voor informatie over de prijzen [Statusconfiguratie van Azure Automation-prijzen](https://azure.microsoft.com/pricing/details/automation/)
- Zie voor een voorbeeld van het gebruik van Azure Automation State Configuration in een pijplijn voor continue implementatie [continue implementatie met behulp van Azure Automation Statusconfiguratie- en Chocolatey](automation-dsc-cd-chocolatey.md)