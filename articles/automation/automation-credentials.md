---
title: Referentieassets in Azure Automation | Microsoft Docs
description: Referentieassets in Azure Automation bevatten beveiligingsreferenties op die kunnen worden gebruikt voor verificatie aan resources die toegankelijk is voor het runbook of de DSC-configuratie. In dit artikel wordt beschreven hoe referentieassets maken en gebruiken in een runbook of de DSC-configuratie.
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 3209bf73-c208-425e-82b6-df49860546dd
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2017
ms.author: bwren
ms.openlocfilehash: ac253fda413718ded815c9a990ae61473a5d8870
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="credential-assets-in-azure-automation"></a>Referentieassets in Azure Automation
Een Automation-referentieasset bevat een [PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) object met beveiligingsgegevens zoals een gebruikersnaam en wachtwoord. Runbooks en DSC-configuraties kunt cmdlets die een PSCredential-object voor verificatie accepteren of ze kunnen extraheren gebruikersnaam en wachtwoord van de PSCredential-object om te bieden tot een bepaalde toepassing of service-verificatie vereist. De eigenschappen van een referentie worden veilig opgeslagen in Azure Automation en kunnen worden geopend in het runbook of de DSC-configuratie met de [Get-AutomationPSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) activiteit.

> [!NOTE]
> Beveiligde activa in Azure Automation zijn referenties, certificaten, verbindingen en gecodeerde variabelen. Deze activa zijn versleuteld en opgeslagen in de Azure Automation met een unieke sleutel die wordt gegenereerd voor elk automation-account. Deze sleutel is versleuteld met een basiscertificaat en opgeslagen in Azure Automation. Voordat u een beveiligd bedrijfsmiddel op te slaan, wordt de sleutel voor het automation-account worden ontsleuteld met het basiscertificaat en vervolgens worden gebruikt voor het versleutelen van de asset.  

## <a name="azure-classic-powershell-cmdlets"></a>Azure Classic PowerShell-cmdlets
De cmdlets in de volgende tabel worden gebruikt voor het maken en beheren van automation referentieassets met Windows PowerShell.  Ze worden verzonden als onderdeel van de [Azure PowerShell-module](/powershell/azure/overview) die beschikbaar is voor gebruik in Automation-runbooks en DSC-configuraties.

| Cmdlets | Beschrijving |
|:--- |:--- |
| [Get-AzureAutomationCredential](/powershell/module/azure/get-azureautomationcredential?view=azuresmps-3.7.0) |Informatie over een referentieasset opgehaald. U kunt alleen de referentie zelf ophalen uit **Get-AutomationPSCredential** activiteit. |
| [Nieuwe AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Hiermee maakt u een nieuw Automation-referentie. |
| [Remove - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Hiermee verwijdert u een Automation-referentie. |
| [Set - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Hiermee stelt u de eigenschappen voor een bestaand Automation-referentie. |

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell-cmdlets
De cmdlets in de volgende tabel worden gebruikt voor AzureRM, maken en beheren van automation referentieassets met Windows PowerShell.  Ze worden verzonden als onderdeel van de [AzureRM.Automation module](/powershell/azure/overview) die beschikbaar is voor gebruik in Automation-runbooks en DSC-configuraties.

| Cmdlets | Beschrijving |
|:--- |:--- |
| [Get-AzureRmAutomationCredential](/powershell/module/azurerm.automation/get-azurermautomationcredential?view=azurermps-4.4.0) |Informatie over een referentieasset opgehaald.  |
| [Nieuwe AzureRmAutomationCredential](/powershell/module/azurerm.automation/new-azurermautomationcredential?view=azurermps-4.4.0) |Hiermee maakt u een nieuw Automation-referentie. |
| [Verwijder AzureRmAutomationCredential](/powershell/module/azurerm.automation/remove-azurermautomationcredential?view=azurermps-4.4.0) |Hiermee verwijdert u een Automation-referentie. |
| [Set-AzureRmAutomationCredential](/powershell/module/azurerm.automation/set-azurermautomationcredential?view=azurermps-4.4.0) |Hiermee stelt u de eigenschappen voor een bestaand Automation-referentie. |

## <a name="runbook-activities"></a>Runbookactiviteiten
De activiteiten in de volgende tabel worden gebruikt voor toegang tot de referenties in een runbook en DSC-configuraties.

| Activiteiten | Beschrijving |
|:--- |:--- |
| Get-AutomationPSCredential |Hiermee haalt u een referentie gebruiken in een runbook of de DSC-configuratie. Retourneert een [System.Management.Automation.PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) object. |

> [!NOTE]
> Vermijd het gebruik van variabelen in de parameter-Name van Get-AutomationPSCredential omdat dit kan detecteren van afhankelijkheden tussen runbooks of DSC-configuraties bemoeilijken en referentieassets in de ontwerpfase.

## <a name="python2-functions"></a>Python2 functies
De functie in de volgende tabel wordt gebruikt voor toegang tot de referenties in een runbook Python2.

| Functie | Beschrijving |
|:---|:---|
| automationassets.get_automation_credential | Informatie over een referentieasset opgehaald. |

> [!NOTE]
> U moet de module 'automationassets' boven aan uw runbook Python importeren om de toegang tot de functies van de asset.

## <a name="creating-a-new-credential-asset"></a>Een nieuwe referentieasset maken

### <a name="to-create-a-new-credential-asset-with-the-azure-portal"></a>Een nieuwe referentieasset maken met de Azure-portal
1. Van uw automation-account, klikt u op de **activa** onderdeel openen de **activa** blade.
2. Klik op de **referenties** onderdeel openen de **referenties** blade.
3. Klik op **toevoegen van een referentie** boven aan de blade.
4. Vul het formulier en klik op **maken** om op te slaan van de nieuwe referentie.

### <a name="to-create-a-new-credential-asset-with-windows-powershell"></a>Een nieuwe referentieasset maken met Windows PowerShell
De volgende voorbeeldopdrachten laten zien hoe een nieuwe automation-referentie maken. Een PSCredential-object is gemaakt met de naam en het wachtwoord en vervolgens worden gebruikt voor het maken van de referentie-element. U kunt ook de **Get-Credential** cmdlet gevraagd een naam en wachtwoord typen.

    $user = "MyDomain\MyUser"
    $pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
    $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
    New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred

## <a name="using-a-powershell-credential"></a>Met behulp van een PowerShell-referentie
Ophalen van een referentie-element in een runbook of de DSC-configuratie met de **Get-AutomationPSCredential** activiteit. Hiermee wordt een [PSCredential-object](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) die u kunt gebruiken met een activiteit of cmdlet waarvoor een PSCredential-parameter. U kunt ook de eigenschappen van het referentieobject afzonderlijk gebruiken ophalen. Het object heeft een eigenschap voor de gebruikersnaam en het beveiligd wachtwoord of kunt u de **GetNetworkCredential** methode om te retourneren een [NetworkCredential](http://msdn.microsoft.com/library/system.net.networkcredential.aspx) object waarmee u een niet-beveiligde versie van het wachtwoord.

### <a name="textual-runbook-sample"></a>Voorbeeld van tekstueel runbook
De volgende voorbeeldopdrachten laten zien hoe een PowerShell-referentie gebruiken in een runbook. In dit voorbeeld wordt de referentie wordt opgehaald en de gebruikersnaam en wachtwoord die zijn toegewezen aan variabelen.

    $myCredential = Get-AutomationPSCredential -Name 'MyCredential'
    $userName = $myCredential.UserName
    $securePassword = $myCredential.Password
    $password = $myCredential.GetNetworkCredential().Password


### <a name="graphical-runbook-sample"></a>Grafisch runbook-voorbeeld
U toevoegen een **Get-AutomationPSCredential** activiteit aan een grafisch runbook door met de rechtermuisknop op de referentie in het deelvenster bibliotheek van de grafische editor en **toevoegen aan papier**.

![Referentie voor de canvas toevoegen](media/automation-credentials/credential-add-canvas.png)

De volgende afbeelding toont een voorbeeld van een referentie gebruiken in een grafisch runbook.  In dit geval wordt deze wordt gebruikt voor verificatie voor een runbook voor Azure-resources zoals beschreven in [Runbooks verifiëren met Azure AD-gebruikersaccount](automation-create-aduser-account.md).  De eerste activiteit haalt de referenties die toegang tot de Azure-abonnement heeft.  De **Add-AzureAccount** activiteit vervolgens deze referentie gebruikt voor verificatie voor alle activiteiten die na deze komen.  Een [pijplijnkoppeling](automation-graphical-authoring-intro.md#links-and-workflow) is hier sinds **Get-AutomationPSCredential** een enkel object verwacht.  

![Referentie voor de canvas toevoegen](media/automation-credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>Met behulp van een PowerShell-referentie in DSC
Terwijl in Azure Automation DSC-configuraties kunnen verwijzen naar referentieassets met **Get-AutomationPSCredential**, referentieassets kunnen ook worden doorgegeven via parameters, indien gewenst. Zie voor meer informatie [compileren van configuraties in Azure Automation DSC](automation-dsc-compile.md#credential-assets).

## <a name="using-credentials-in-python2"></a>Met behulp van referenties in Python2
Het volgende voorbeeld toont een voorbeeld van de toegang tot de referenties in Python2 runbooks.

    import automationassets
    from automationassets import AutomationAssetNotFound

    # get a credential
    cred = automationassets.get_automation_credential("credtest")
    print cred["username"]
    print cred["password"]

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over koppelingen in het grafisch ontwerpen, [koppelingen in het grafisch ontwerpen](automation-graphical-authoring-intro.md#links-and-workflow)
* Zie voor informatie over de verschillende verificatiemethoden met Automation, [Azure Automation-beveiliging](automation-security-overview.md)
* Zie [Mijn eerste grafische runbook](automation-first-runbook-graphical.md) om aan de slag te gaan met grafische runbooks
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks 
* Om te beginnen met Python2 runbooks, Zie [Mijn eerste Python2 runbook](automation-first-runbook-textual-python2.md) 

