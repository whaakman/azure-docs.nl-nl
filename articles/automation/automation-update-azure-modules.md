---
title: Azure-modules in Azure Automation bijwerken
description: Dit artikel wordt beschreven hoe u kunt algemene Azure PowerShell-modules standaardvoorziening in Azure Automation nu bijwerken.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/30/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c09c9df1b8af7adca5c0169cf31881121ba6cc99
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427481"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Het bijwerken van Azure PowerShell-modules in Azure Automation

Bijwerken van de Azure-modules in uw Automation-Account dat u wilt gebruiken de [bijwerken Azure-modules runbook](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update), deze is beschikbaar als open source. Aan de slag met de **Update AutomationAzureModulesForAccount** runbook bijwerken van uw Azure-modules downloaden via de [bijwerken Azure-modules runbook opslagplaats](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) op GitHub. U kunt vervolgens importeren in uw Automation-Account of als een script uitvoeren. Zie voor informatie over het importeren van een runbook in uw Automation-Account, [een runbook importeert](manage-runbooks.md#import-a-runbook).

De meest voorkomende AzureRM PowerShell-modules worden geboden door standaard in elk Automation-account. Het Azure-team werkt de Azure-modules regelmatig, daarom naar up-to-date te houden u wilt gebruiken de [Update AutomationAzureModulesForAccount](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) runbook om bij te werken van de modules in uw Automation-accounts.

Omdat modules regelmatig door de productgroep bijgewerkt worden, kunnen wijzigingen optreden met de cmdlets opgenomen. Deze actie kan een negatieve invloed hebben op uw runbooks afhankelijk van het type wijziging, zoals de naam van een parameter of volledig beëindigde van een cmdlet.

Om te voorkomen dat dit gevolgen heeft voor uw runbooks en de processen die ze automatiseren, testen en te valideren voordat u doorgaat. Als u een speciale Automation-account dat is bestemd voor dit doel niet hebt, houd rekening met het maken van een zodat u veel verschillende scenario's tijdens de ontwikkeling van uw runbooks testen kunt. Deze test moet iteratieve wijzigingen zoals het bijwerken van de PowerShell-modules bevatten.

Als u uw scripts lokaal ontwikkelt, is het aanbevolen dat de dezelfde moduleversies lokaal die u in uw Automation-Account hebt, wanneer de dezelfde resultaten testen om ervoor te zorgen dat u ontvangt. Nadat de resultaten worden gevalideerd en u de vereiste wijzigingen hebt toegepast, kunt u de wijzigingen kunt verplaatsen naar productie.

> [!NOTE]
> Een nieuw Automation-account kan niet de meest recente modules bevatten.

## <a name="considerations"></a>Overwegingen

Hier volgen enkele overwegingen rekening moet houden bij het gebruik van dit proces om bij te werken van uw Azure-Modules:

* Dit runbook biedt ondersteuning voor bijwerken van alleen de **Azure** en **AzureRm** momenteel modules. [Azure PowerShell Az-modules](/powershell/azure/new-azureps-module-az) in Automation-accounts worden ondersteund, maar kan niet worden bijgewerkt met dit runbook. Er zijn belangrijke factoren die u rekening mee dat moet bij het gebruik van de `Az` modules in uw Automation-Account voor meer informatie, Zie [met Az-modules in uw Automation-Account](az-modules.md).

* Voorkomen dat dit runbook starten op Automation-accounts die Az modules bevatten.

* Voordat u begint met dit runbook, zorg ervoor dat uw Automation-account heeft een [Azure uitvoeren als-accountreferenties](manage-runas-account.md) gemaakt.

* Kunt u deze code als een reguliere PowerShell-script in plaats van een runbook: Meld u aan met behulp van Azure de [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) eerst de opdracht, en vervolgens doorgegeven `-Login $false` aan het script.

* Als u dit runbook op de onafhankelijke clouds, gebruikt de `AzureRmEnvironment` parameter om door te geven van de juiste omgeving aan het runbook.  Acceptabele waarden zijn **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud**, en **AzureUSGovernment**. Deze waarden kunnen worden opgehaald uit met behulp van `Get-AzureRmEnvironment | select Name`. Als u niet een waarde voor deze parameter doorgeven, het runbook de openbare cloud van Azure wordt standaard **AzureCloud**

* Als u een specifieke versie van de Azure PowerShell-module gebruiken in plaats van de meest recente beschikbare op de PowerShell Gallery wilt, deze versies doorgeven aan de optionele `ModuleVersionOverrides` parameter van de **Update-AutomationAzureModulesForAccount**runbook. Zie voor voorbeelden van de [Update AutomationAzureModulesForAccount.ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
) runbook. Azure PowerShell-modules die niet worden vermeld in de `ModuleVersionOverrides` parameter worden bijgewerkt met de meest recente moduleversies op de PowerShell Gallery. Als u niets om weer te geven de `ModuleVersionOverrides` parameter, alle modules worden bijgewerkt met de meest recente moduleversies op de PowerShell Gallery. Dit gedrag is hetzelfde als de **Update Azure-Modules** knop.

## <a name="known-issues"></a>Bekende problemen

Er is een bekend probleem met het bijwerken van de AzureRM-modules in een Automation-Account dat in een resourcegroep gemaakt met een numerieke naam die met 0 begint. Voor het bijwerken van uw Azure-modules in uw Automation-Account, moet deze zich in een resourcegroep met een alfanumerieke naam. Resourcegroepen met numerieke namen die beginnen met 0 zijn kan niet worden bijgewerkt AzureRM-modules op dit moment.

## <a name="next-steps"></a>Volgende stappen

Ga naar de open-source [Update Azure-modules runbook](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) voor meer informatie over deze.
