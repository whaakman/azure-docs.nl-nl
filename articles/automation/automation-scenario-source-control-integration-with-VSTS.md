---
title: Azure Automation integreren met Azure DevOps Services broncodebeheer
description: Scenario begeleidt u bij het instellen van integratie met een Azure Automation-account en bronbeheer voor Azure DevOps-Services.
services: automation
author: eamonoreilly
ms.author: eamono
keywords: Azure powershell, Azure DevOps-Services, broncodebeheer, automation
ms.service: automation
ms.component: process-automation
ms.topic: conceptual
ms.date: 03/19/2017
ms.openlocfilehash: 022fca09b9e748c030df6b5fc944f7930942a6f7
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302405"
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-azure-devops"></a>Azure Automation-scenario: Automation integratie van broncodebeheer met Azure DevOps

In dit scenario hebt u een Azure DevOps-project dat u gebruikt voor het beheren van Azure Automation-runbooks of DSC-configuraties in broncodebeheer.
In dit artikel wordt beschreven hoe u Azure DevOps integreren met uw Azure Automation-omgeving zodat continue integratie gebeurt voor elke check-in.

## <a name="getting-the-scenario"></a>Het scenario ophalen

Dit scenario bestaat uit twee PowerShell-runbooks die u rechtstreeks vanuit importeren kunt de [Runbookgalerie](automation-runbook-gallery.md) in Azure portal of downloaden van de [PowerShell Gallery](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooks

Runbook | Beschrijving| 
--------|------------|
Synchronisatie-VSTS | Importeer runbooks of configuraties vanuit Azure DevOps-bronbeheer wanneer een check-in is voltooid. Als handmatig uitvoert, importeert en publiceert alle runbooks of configuraties in het Automation-account.| 
Synchronisatie-VSTSGit | Importeren van runbooks of configuraties van Azure DevOps onder Git-bronbeheer wanneer een check-in is voltooid. Als handmatig uitvoert, importeert en publiceert alle runbooks of configuraties in het Automation-account.|

### <a name="variables"></a>Variabelen

Variabele | Beschrijving|
-----------|------------|
VSToken | Beveiligde variabeleasset moet u maken met het persoonlijke toegangstoken van Azure DevOps. U kunt informatie over het maken van een persoonlijk toegangstoken van Azure DevOps op de [Azure DevOps-verificatiepagina](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).
## <a name="installing-and-configuring-this-scenario"></a>Dit scenario installeren en configureren

Maak een [persoonlijk toegangstoken](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) in Azure DevOps die u gebruiken om te synchroniseren van de runbooks of configuraties in uw automation-account.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Maak een [beveiligde variabele](automation-variables.md) in uw automation-account voor het opslaan van het persoonlijke toegangstoken zodat het runbook kan worden geverifieerd bij Azure DevOps en synchroniseren van de runbooks of configuraties in het Automation-account. U kunt deze VSToken naam.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Importeer het runbook dat worden gesynchroniseerd met uw runbooks of configuraties in het automation-account. U kunt de [voorbeeldrunbook Azure DevOps](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) of de [Azure DevOps met Git voorbeeldrunbook](https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) vanuit de PowerShellGallery.com afhankelijk van of u met Azure DevOps-broncodebeheer of Azure DevOps met Git en implementeren naar uw automation-account.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

U kunt nu [publiceren](automation-creating-importing-runbook.md#publishing-a-runbook) dit runbook zodat u een webhook kunt maken. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Maak een [webhook](automation-webhooks.md) voor deze runbook-synchronisatie-VSTS en vult u in de parameters zoals hieronder wordt weergegeven. Zorg ervoor dat u de webhook-url hebt gekopieerd, zoals u hebt deze nodig voor een haakje service in Azure DevOps. De VSAccessTokenVariableName is de naam (VSToken) van de beveiligde variabele die u eerder hebt gemaakt voor het persoonlijke toegangstoken bevatten. 

Integreren met Azure DevOps (Sync-VSTS.ps1), worden de volgende parameters:
### <a name="sync-vsts-parameters"></a>Synchronisatie-VSTS-Parameters

Parameter | Beschrijving| 
--------|------------|
WebhookData | Dit document bevat de inchecken informatie verzonden van de service Azure DevOps hook. U moet deze parameter leeg laten.| 
ResourceGroup | Dit is de naam van de resourcegroep waarin het automation-account.|
AutomationAccountName | De naam van het automation-account die wordt gesynchroniseerd met Azure DevOps.|
VSFolder | De naam van de map in Azure DevOps waar de runbooks en configuraties bestaan.|
VSAccount | De naam van het Azure DevOps-organisatie.| 
VSAccessTokenVariableName | De naam van de beveiligde variabele (VSToken) waarin het persoonlijke toegangstoken van Azure DevOps.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Als u van Azure DevOps met GIT (Sync-VSTSGit.ps1 gebruikmaakt) wordt uitgevoerd, de volgende parameters.

Parameter | Beschrijving|
--------|------------|
WebhookData | Dit bevat de inchecken informatie verzonden van de service Azure DevOps hook. U moet deze parameter leeg laten.| 
ResourceGroup | Deze de naam van de resourcegroep waarin het automation-account.|
AutomationAccountName | De naam van het automation-account die wordt gesynchroniseerd met Azure DevOps.|
VSAccount | De naam van het Azure DevOps-organisatie.|
VSProject | De naam van het project in Azure DevOps waar de runbooks en configuraties bestaan.|
GitRepo | De naam van de Git-opslagplaats.|
GitBranch | De naam van de vertakking in Azure DevOps Git-opslagplaats.|
Map | De naam van de map in Azure DevOps Git-branch.|
VSAccessTokenVariableName | De naam van de beveiligde variabele (VSToken) waarin het persoonlijke toegangstoken van Azure DevOps.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Een service-hook maken in Azure DevOps voor check-ins naar de map die wordt geactiveerd webhook op code is ingecheckt. Selecteer **Web Hooks** als de service om te integreren in wanneer u een nieuw abonnement maakt. U meer informatie over servicehooks op [documentatie voor Azure DevOps-Servicehooks](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

U zou nu moeten kunnen alle controles van uw runbooks en configuraties in Azure DevOps doen en hebben deze automatisch gesynchroniseerd naar uw automation-account.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Als u dit runbook handmatig uitvoert zonder wordt geactiveerd door Azure DevOps, kunt u de parameter webhookdata leeg laten en wordt een volledige synchronisatie van het Azure DevOps-map die is opgegeven.

Als u wilt verwijderen van het scenario, verwijdert u de service-hook uit Azure DevOps, verwijdert u het runbook en de variabele VSToken.
