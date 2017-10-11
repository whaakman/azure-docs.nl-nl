---
title: Azure Automation integreren met broncodebeheer Visual Stuido Team Services | Microsoft Docs
description: Scenario begeleidt u bij het instellen van integratie met een Azure Automation-account en de bronbeheer Visual Stuido Team Services.
services: automation
documentationcenter: 
author: eamono
manager: 
editor: 
keywords: Azure powershell, VSTS, bronbeheer, automation
ms.assetid: a43b395a-e740-41a3-ae62-40eac9d0ec00
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.openlocfilehash: 01f9c01c9e04e02dbb548b68cf99684ba6ddd57e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a>Azure Automation-scenario - integratie van broncodebeheer automatisering met Visual Studio Team Services

In dit scenario hebt u een project voor Visual Studio Team Services die u gebruikt voor het beheren van Azure Automation-runbooks of DSC-configuraties onder bronbeheer.
In dit artikel wordt beschreven hoe VSTS integreren met uw Azure Automation-omgeving zodat continue integratie gebeurt voor elke controle in.

## <a name="getting-the-scenario"></a>Het scenario ophalen

Dit scenario bestaat uit twee PowerShell-runbooks die u rechtstreeks vanuit importeren kunt de [Runbookgalerie](automation-runbook-gallery.md) in de Azure-portal of downloaden van de [PowerShell Gallery](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooks

Runbook | Beschrijving| 
--------|------------|
Synchronisatie-VSTS | Importeren runbooks of configuraties van broncodebeheer VSTS als een check-in is voltooid. Als u handmatig uitvoert, wordt de importeren en publiceren van alle runbooks of -configuraties in het Automation-account.| 
Synchronisatie VSTSGit | Runbooks of configuraties uit importeren VSTS onder bronbeheer Git als een check-in is voltooid. Als u handmatig uitvoert, wordt de importeren en publiceren van alle runbooks of -configuraties in het Automation-account.|

### <a name="variables"></a>Variabelen

Variabele | Beschrijving|
-----------|------------|
VSToken | Beveiligde variabelenactivum maakt u met de VSTS persoonlijke toegangstoken. U kunt informatie over het maken van een VSTS persoonlijke toegangstoken op de [VSTS verificatiepagina](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview). 
## <a name="installing-and-configuring-this-scenario"></a>Dit scenario installeren en configureren

Maak een [persoonlijke toegangstoken](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) in VSTS die u gebruiken wilt voor het synchroniseren van de runbooks of -configuraties in uw automation-account.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Maak een [beveiligde variabele](automation-variables.md) in uw automation-account voor het opslaan van het persoonlijke toegangstoken zodat het runbook kan worden geverifieerd met VSTS en synchroniseren van de runbooks of -configuraties in het Automation-account. U kunt deze VSToken naam. 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Importeer het runbook dat uw runbooks of -configuraties in het automation-account wordt gesynchroniseerd. U kunt de [VSTS voorbeeldrunbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) of de VSTS met Git voorbeeldrunbook (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) van de PowerShellGallery.com afhankelijk van of u verbinding met broncodebeheer VSTS of VSTS gebruiken met Git en implementeren voor uw automation-account.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

U kunt nu [publiceren](automation-creating-importing-runbook.md#publishing-a-runbook) dit runbook zodat u kunt een webhook maken. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Maak een [webhook](automation-webhooks.md) voor deze runbook-synchronisatie VSTS en vul de parameters zoals hieronder wordt weergegeven. Zorg ervoor dat u de webhook-url kopiëren als u deze nodig voor een service haakje in VSTS. De VSAccessTokenVariableName is de naam (VSToken) van de beveiligde variabele die u eerder hebt gemaakt voor het persoonlijke toegangstoken houdt. 

Integratie met VSTS (Sync-VSTS.ps1), gaat de volgende parameters.
### <a name="sync-vsts-parameters"></a>Synchronisatie-VSTS Parameters

Parameter | Beschrijving| 
--------|------------|
WebhookData | Hiermee wordt het inchecken-informatie verzonden van de service VSTS hook bevatten. U moet deze parameter leeg laten.| 
ResourceGroup | Dit is de naam van de resourcegroep die het automation-account in.|
AutomationAccountName | De naam van het automation-account met VSTS moeten worden gesynchroniseerd.|
VSFolder | De naam van de map in VSTS waar de runbooks en configuraties bestaan.|
VSAccount | De naam van de Visual Studio Team Services-account.| 
VSAccessTokenVariableName | De naam van de beveiligde variabele (VSToken) die het VSTS persoonlijke toegangstoken bevat.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Als u van VSTS met GIT (Sync-VSTSGit.ps1 gebruikmaakt) wordt uitgevoerd, de volgende parameters.

Parameter | Beschrijving|
--------|------------|
WebhookData | Hiermee wordt het inchecken-informatie verzonden van de service VSTS hook bevatten. U moet deze parameter leeg laten.| ResourceGroup | Deze de naam van de resourcegroep die het automation-account in.|
AutomationAccountName | De naam van het automation-account met VSTS moeten worden gesynchroniseerd.|
VSAccount | De naam van de Visual Studio Team Services-account.|
VSProject | De naam van het project in VSTS waar de runbooks en configuraties bestaan.|
GitRepo | De naam van de Git-opslagplaats.|
GitBranch | De naam van de vertakking in VSTS Git-opslagplaats.|
Map | De naam van de map in VSTS Git vertakking.|
VSAccessTokenVariableName | De naam van de beveiligde variabele (VSToken) die het VSTS persoonlijke toegangstoken bevat.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Maak een haakje service in VSTS voor controle-modules naar de map die deze webhook bij code controle wordt geactiveerd. Selecteer Web Hiermee als de service om te integreren met wanneer u een nieuw abonnement maakt. Voor meer informatie over service hooks op [VSTS Service Hooks documentatie](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Nu moet u kunnen alle controle-modules van uw runbooks en de configuraties in VSTS doen en moet deze automatisch synchroniseren 'd in uw automation-account.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Als u dit runbook handmatig uitvoert zonder wordt geactiveerd door VSTS, kunt u de parameter webhookdata leeg laten en doet een volledige synchronisatie van de map VSTS die is opgegeven.

Als u wilt verwijderen van het scenario, verwijdert u de service hook uit VSTS, verwijdert u het runbook en de variabele VSToken.
