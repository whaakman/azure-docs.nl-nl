---
title: Referentieassets in Azure Automation
description: Referentieassets in Azure Automation bevatten beveiligingsreferenties op die kunnen worden gebruikt om de resources die worden gebruikt door het runbook of DSC-configuratie te verifiëren. In dit artikel wordt beschreven hoe u referentieassets maken en deze gebruiken in een runbook of DSC-configuratie.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 05/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1070cc97624b39ec929f5083f7073179fb1f068c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57899142"
---
# <a name="credential-assets-in-azure-automation"></a>Referentieassets in Azure Automation

Een Automation-referentieasset bevat een object dat beveiligingsreferenties, zoals een gebruikersnaam en wachtwoord bevat. Runbooks en DSC-configuraties kunnen-cmdlets die een PSCredential-object voor de verificatie accepteren gebruiken of ze kunnen extraheren de gebruikersnaam en het wachtwoord van de PSCredential-object om te bieden voor enkele toepassing of service-verificatie vereist. De eigenschappen van een referentie worden veilig opgeslagen in Azure Automation en kunnen worden geopend in het runbook of DSC-configuratie met de [Get-AutomationPSCredential](#activities) activiteit.

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../includes/gdpr-dsr-and-stp-note.md)]

> [!NOTE]
> Beveiligde activa in Azure Automation zijn referenties, certificaten, verbindingen en gecodeerde variabelen. Deze apparaten worden versleuteld en opgeslagen in Azure Automation met behulp van een unieke sleutel die wordt gegenereerd voor elk automation-account. Deze sleutel wordt opgeslagen in Key Vault. Voordat u een beveiligd bedrijfsmiddel opslaat, is de sleutel geladen uit Key Vault en vervolgens worden gebruikt voor het versleutelen van de asset.

## <a name="azure-classic-powershell-cmdlets"></a>Azure klassieke PowerShell-cmdlets

De cmdlets in de volgende tabel worden gebruikt voor het maken en beheren van automation-referentie-assets met Windows PowerShell.  Ze geleverd als onderdeel van de [Azure PowerShell-module](/powershell/azure/overview), die beschikbaar is voor gebruik in Automation-runbooks en DSC-configuraties.

| Cmdlets | Description |
|:--- |:--- |
| [Get-AzureAutomationCredential](/powershell/module/servicemanagement/azure/get-azureautomationcredential) |Haalt informatie op over een referentie-element. U kunt alleen de referentie op die zelf ophalen uit **Get-AutomationPSCredential** activiteit. |
| [New-AzureAutomationCredential](/powershell/module/servicemanagement/azure/new-azureautomationcredential) |Hiermee maakt u een nieuw Automation-referentie. |
| [Remove-AzureAutomationCredential](/powershell/module/servicemanagement/azure/new-azureautomationcredential) |Hiermee verwijdert u een Automation-referentie. |
| [Set-AzureAutomationCredential](/powershell/module/servicemanagement/azure/new-azureautomationcredential) |Hiermee stelt u de eigenschappen voor een bestaande Automation-referentie. |

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell-cmdlets

De cmdlets in de volgende tabel worden gebruikt voor AzureRM, maken en beheren van automation-referentie-assets met Windows PowerShell.  Ze geleverd als onderdeel van de [AzureRM.Automation module](/powershell/azure/overview), die beschikbaar is voor gebruik in Automation-runbooks en DSC-configuraties.

| Cmdlets | Description |
|:--- |:--- |
| [Get-AzureRmAutomationCredential](/powershell/module/azurerm.automation/get-azurermautomationcredential) |Haalt informatie op over een referentie-element.  |
| [New-AzureRmAutomationCredential](/powershell/module/azurerm.automation/new-azurermautomationcredential) |Hiermee maakt u een nieuw Automation-referentie. |
| [Remove-AzureRmAutomationCredential](/powershell/module/azurerm.automation/remove-azurermautomationcredential) |Hiermee verwijdert u een Automation-referentie. |
| [Set-AzureRmAutomationCredential](/powershell/module/azurerm.automation/set-azurermautomationcredential) |Hiermee stelt u de eigenschappen voor een bestaande Automation-referentie. |

## <a name="activities"></a>Activiteiten

De activiteiten in de volgende tabel worden gebruikt voor toegang tot de referenties in een runbook en DSC-configuraties.

| Activiteiten | Description |
|:--- |:--- |
| Get-AutomationPSCredential |Hiermee haalt u een referentie op die moet worden gebruikt in een runbook of DSC-configuratie. Retourneert een [System.Management.Automation.PSCredential](/dotnet/api/system.management.automation.pscredential) object. |

> [!NOTE]
> Vermijd het gebruik van variabelen in de parameter-Name van Get-AutomationPSCredential omdat dit kan detecteren van afhankelijkheden tussen runbooks of DSC-configuraties bemoeilijken en referentieassets tijdens de ontwerpfase.

## <a name="python2-functions"></a>Python2-functies

De functie in de volgende tabel wordt gebruikt voor toegang tot de referenties in een Python2-runbook.

| Function | Description |
|:---|:---|
| automationassets.get_automation_credential | Haalt informatie op over een referentie-element. |

> [!NOTE]
> U moet de module 'automationassets' aan de bovenkant van uw Python-runbook importeren om toegang tot de functies van de asset.

## <a name="creating-a-new-credential-asset"></a>Het maken van een nieuwe referentie-element

### <a name="to-create-a-new-credential-asset-with-the-azure-portal"></a>Een nieuwe referentieasset maken met de Azure-portal

1. Selecteer in uw automation-account **referenties** onder **gedeelde bronnen**.
1. Klik op **+ toevoegen van een referentie**.
1. Vul het formulier in en klikt u op **maken** om op te slaan van de nieuwe referentie.

> [!NOTE]
> Gebruikersaccounts die gebruikmaken van meerdere factoren verificatie worden niet ondersteund voor gebruik in Azure Automation.

### <a name="to-create-a-new-credential-asset-with-windows-powershell"></a>Een nieuwe referentieasset maken met Windows PowerShell

De volgende voorbeeldopdrachten laten zien hoe u een nieuw automation-referentie maken. Een PSCredential-object is gemaakt met de naam en het wachtwoord en klikt u vervolgens gebruikt voor het maken van de referentie-element. U kunt ook de **Get-Credential** cmdlet om te worden gevraagd een naam en wachtwoord.

```powershell
$user = "MyDomain\MyUser"
$pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
$cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred
```

## <a name="using-a-powershell-credential"></a>Met behulp van een PowerShell-referentie

Ophalen van een referentie-element in een runbook of DSC-configuratie met de **Get-AutomationPSCredential** activiteit. Hiermee wordt een [PSCredential-object](/dotnet/api/system.management.automation.pscredential) die u kunt gebruiken met een activiteit of cmdlet waarvoor een PSCredential-parameter. U kunt ook de eigenschappen van het referentieobject u afzonderlijk ophalen. Het object heeft een eigenschap voor de gebruikersnaam en het veilig wachtwoord, of kunt u de **GetNetworkCredential** methode om terug te keren een [NetworkCredential](/dotnet/api/system.net.networkcredential) object dat een niet-beveiligde versie van de het wachtwoord.

### <a name="textual-runbook-sample"></a>Tekstuele runbook-voorbeeld

De volgende voorbeeldopdrachten laten zien hoe u een PowerShell-referentie gebruiken in een runbook. In dit voorbeeld wordt de referentie op die wordt opgehaald en de gebruikersnaam en wachtwoord die zijn toegewezen aan variabelen.

```azurepowershell
$myCredential = Get-AutomationPSCredential -Name 'MyCredential'
$userName = $myCredential.UserName
$securePassword = $myCredential.Password
$password = $myCredential.GetNetworkCredential().Password
```

U kunt ook een referentie gebruiken voor verificatie op Azure met [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount). In de meeste gevallen moet u een [uitvoeren als-account](manage-runas-account.md) en op te halen met [Get-AutomationConnection](automation-connections.md).

```azurepowershell
$myCred = Get-AutomationPSCredential -Name 'MyCredential`
$userName = $myCred.UserName
$securePassword = $myCred.Password
$password = $myCred.GetNetworkCredential().Password

$myPsCred = New-Object System.Management.Automation.PSCredential ($userName,$password)

Connect-AzureRmAccount -Credential $myPsCred
```

### <a name="graphical-runbook-sample"></a>Voorbeeld van een grafisch runbook

U wilt toevoegen een **Get-AutomationPSCredential** activiteit aan een grafisch runbook door te selecteren met de rechtermuisknop op de referentie in het deelvenster bibliotheek van de grafische editor **toevoegen aan papier**.

![Referentie voor canvas-toevoegen](media/automation-credentials/credential-add-canvas.png)

De volgende afbeelding toont een voorbeeld van het gebruik van een referentie in een grafisch runbook.  In dit geval, deze wordt gebruikt voor verificatie voor een runbook met Azure-resources zoals beschreven in [Runbooks verifiëren met Azure AD-gebruikersaccount](automation-create-aduser-account.md).  De eerste activiteit haalt de referenties die toegang tot het Azure-abonnement heeft.  De **Add-AzureAccount** activiteit worden vervolgens deze referentie gebruikt voor verificatie voor alle activiteiten die later zijn dan deze.  Een [pijplijn koppeling](automation-graphical-authoring-intro.md#links-and-workflow) hier is al sinds **Get-AutomationPSCredential** een enkel object wordt verwacht.  

![Referentie voor canvas-toevoegen](media/automation-credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>Met behulp van een PowerShell-referentie in DSC

Terwijl DSC-configuraties in Azure Automation kunnen verwijzen naar referentieassets met behulp van **Get-AutomationPSCredential**, referentieassets kunnen ook worden doorgegeven via parameters, indien gewenst. Zie voor meer informatie, [in Azure Automation DSC-configuraties compileren](automation-dsc-compile.md#credential-assets).

## <a name="using-credentials-in-python2"></a>Met behulp van referenties in Python2

Het volgende voorbeeld toont een voorbeeld van de toegang tot de referenties in Python2-runbooks.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a credential
cred = automationassets.get_automation_credential("credtest")
print cred["username"]
print cred["password"]
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over koppelingen in het grafisch ontwerpen, [koppelingen in het grafisch ontwerpen](automation-graphical-authoring-intro.md#links-and-workflow)
* Zie voor meer informatie over de verschillende verificatiemethoden met Automation, [Azure Automation-beveiliging](automation-security-overview.md)
* Zie [Mijn eerste grafische runbook](automation-first-runbook-graphical.md) om aan de slag te gaan met grafische runbooks
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks 
* Als u wilt beginnen met Python2-runbooks, Zie [Mijn eerste Python2-runbook](automation-first-runbook-textual-python2.md) 
