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
ms.date: 09/26/2018
ms.openlocfilehash: b06e315cc12856976dce87791b423d10f002c6df
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801434"
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-azure-devops"></a>Azure Automation-scenario: Automation integratie van broncodebeheer met Azure DevOps

> [!NOTE]
> Er is een nieuwe ervaring voor broncodebeheer. Zie voor meer informatie over de nieuwe ervaring, [broncodebeheer (Preview)](source-control-integration.md).

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

Importeer het runbook dat worden gesynchroniseerd met uw runbooks of configuraties in het automation-account. Kunt u de [Azure DevOps-voorbeeldrunbook](https://www.powershellgallery.com/packages/Sync-VSTS) of de [Azure DevOps met Git voorbeeldrunbook](https://www.powershellgallery.com/packages/Sync-VSTSGit) uit de [PowerShell Gallery](https://www.powershellgallery.com) afhankelijk van of u Azure DevOps-bron besturingselement of Azure DevOps met Git en implementeren in uw automation-account.

![PowerShell Gallery](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

U kunt nu [publiceren](automation-creating-importing-runbook.md#publishing-a-runbook) dit runbook zodat u een webhook kunt maken.

![Pulish runbook](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

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

![Webhook](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

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

![GIT-Webhook](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Een service-hook maken in Azure DevOps voor check-ins naar de map die wordt geactiveerd webhook op code is ingecheckt. Selecteer **Web Hooks** als de service om te integreren in wanneer u een nieuw abonnement maakt. U meer informatie over servicehooks op [documentatie voor Azure DevOps-Servicehooks](https://www.visualstudio.com/docs/marketplace/integrate/service-hooks/get-started).
![Hook-service](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

U zou nu moeten kunnen alle controles van uw runbooks en configuraties in Azure DevOps doen en hebben deze automatisch gesynchroniseerd naar uw automation-account.

![Synchronisatie van runbook-uitvoer](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Als u dit runbook handmatig uitvoert zonder wordt geactiveerd door Azure DevOps, kunt u de parameter webhookdata leeg laten en wordt een volledige synchronisatie van het Azure DevOps-map die is opgegeven.

Als u wilt verwijderen van het scenario, verwijdert u de service-hook uit Azure DevOps, verwijdert u het runbook en de variabele VSToken.
