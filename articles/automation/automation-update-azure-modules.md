---
title: Azure-modules in Azure Automation bijwerken
description: Dit artikel wordt beschreven hoe u kunt algemene Azure PowerShell-modules standaardvoorziening in Azure Automation nu bijwerken.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 03174e6336589f8aa49a7fc7197da1301ff54400
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58009776"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Het bijwerken van Azure PowerShell-modules in Azure Automation

Bijwerken van de Azure-modules in uw Automation-Account wordt aangeraden u gebruikt de [bijwerken Azure-modules runbook](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update), dit is nu open-source. Bovendien kunt u de **Azure-Modules bijwerken** knop in de portal naar uw Azure-modules bijwerken. Zie voor meer informatie over het gebruik van de open-source-runbook, [Update Azure-Modules met open-source runbook](#open-source).

De meest voorkomende Azure PowerShell-modules worden geboden door standaard in elk Automation-account. De Azure-modules worden regelmatig bijgewerkt in de Azure-team. In uw Automation-account krijgt u een manier om bij te werken van de modules in het account als nieuwe versies beschikbaar via de portal zijn.

Omdat modules regelmatig door de productgroep bijgewerkt worden, kunnen wijzigingen optreden met de cmdlets opgenomen. Deze actie kan een negatieve invloed hebben op uw runbooks afhankelijk van het type wijziging, zoals de naam van een parameter of volledig beëindigde van een cmdlet.

Om te voorkomen dat dit gevolgen heeft voor uw runbooks en de processen die ze automatiseren, testen en te valideren voordat u doorgaat. Als u een speciale Automation-account dat is bestemd voor dit doel niet hebt, houd rekening met het maken van een zodat u veel verschillende scenario's tijdens de ontwikkeling van uw runbooks testen kunt. Deze test moet iteratieve wijzigingen zoals het bijwerken van de PowerShell-modules bevatten. 

Als u uw scripts lokaal ontwikkelt, is het aanbevolen dat de dezelfde moduleversies lokaal die u in uw Automation-Account hebt, wanneer de dezelfde resultaten testen om ervoor te zorgen dat u ontvangt. Nadat de resultaten worden gevalideerd en u de vereiste wijzigingen hebt toegepast, kunt u de wijzigingen kunt verplaatsen naar productie.

> [!NOTE]
> Een nieuw Automation-account kan niet de meest recente modules bevatten.

## <a name="open-source"></a>Azure-Modules bijwerken met open source-runbook

Aan de slag met de **Update AutomationAzureModulesForAccount** runbook bijwerken van uw Azure-modules downloaden via de [bijwerken Azure-modules runbook opslagplaats](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) op GitHub. U kunt vervolgens importeren in uw Automation-Account of als een script uitvoeren. De instructies hiervoor vindt u de [Update Azure-modules runbook opslagplaats](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update).

### <a name="considerations"></a>Overwegingen

Hier volgen enkele overwegingen rekening moet houden bij het gebruik van dit proces om bij te werken van uw Azure-Modules:

* Als u dit runbook met de naam van de oorspronkelijke importeert `Update-AutomationAzureModulesForAccount`, overschrijft het interne runbook met deze naam. Als gevolg hiervan het geïmporteerde runbook wordt uitgevoerd wanneer de **Update Azure-Modules** knop wordt gepusht of wanneer dit runbook wordt opgeroepen rechtstreeks via Azure Resource Manager-API voor dit Automation-account.

* Dit runbook biedt ondersteuning voor bijwerken van alleen de **Azure** en **AzureRm** momenteel modules. [Azure PowerShell Az-modules](/powershell/azure/new-azureps-module-az) in Automation-accounts worden ondersteund, maar kan niet worden bijgewerkt met dit runbook.

* Voorkomen dat dit runbook starten op Automation-accounts die Az modules bevatten.

* Voordat u begint met dit runbook, zorg ervoor dat uw Automation-account heeft een [Azure uitvoeren als-accountreferenties](manage-runas-account.md) gemaakt.

* Kunt u deze code als een reguliere PowerShell-script in plaats van een runbook: alleen Meld u aan bij Azure met de [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) eerst de opdracht, en vervolgens doorgegeven `-Login $false` aan het script.

* Als u dit runbook op de onafhankelijke clouds, gebruikt de `AzureRmEnvironment` parameter om door te geven van de juiste omgeving aan het runbook.  Acceptabele waarden zijn **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud**, en **AzureUSGovernment**. Deze waarden kunnen worden opgehaald uit met behulp van `Get-AzureRmEnvironment | select Name`. Als u niet een waarde voor deze parameter doorgeven, het runbook de openbare cloud van Azure wordt standaard **AzureCloud**

* Als u een specifieke versie van de Azure PowerShell-module gebruiken in plaats van de meest recente beschikbare op de PowerShell Gallery wilt, deze versies doorgeven aan de optionele `ModuleVersionOverrides` parameter van de **Update-AutomationAzureModulesForAccount**runbook. Zie voor voorbeelden van de [Update AutomationAzureModulesForAccount.ps1](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update/blob/master/Update-AutomationAzureModulesForAccount.ps1
) runbook. Azure PowerShell-modules die niet worden vermeld in de `ModuleVersionOverrides` parameter worden bijgewerkt met de meest recente moduleversies op de PowerShell Gallery. Als u niets om weer te geven de `ModuleVersionOverrides` parameter, alle modules worden bijgewerkt met de meest recente moduleversies op de PowerShell Gallery. Dit gedrag is hetzelfde als de **Update Azure-Modules** knop.

## <a name="update-azure-modules-in-the-azure-portal"></a>Azure-Modules bijwerken in Azure portal

1. Op de pagina Modules van uw Automation-account, wordt de optie **Update Azure-Modules**. Het altijd ingeschakeld.<br><br> ![De optie Azure-Modules in Modules pagina bijwerken](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

   > [!NOTE]
   > Voordat u bijwerkt van uw Azure-modules die het wordt aanbevolen dat u werkt ze bij in een test Automation-Account om ervoor te zorgen dat werken uw bestaande scripts zoals verwacht voordat het bijwerken van uw Azure-modules.
   >
   > De **Update Azure-Modules** knop is alleen beschikbaar in de openbare cloud. het is niet beschikbaar in de [soevereine regio's](https://azure.microsoft.com/global-infrastructure/). Gebruik de **Update AutomationAzureModulesForAccount** runbook aan uw Azure-modules bijwerken. U kunt dit ook downloaden via de [Update Azure-modules runbook opslagplaats](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update). Zie voor meer informatie over het gebruik van de open-source-runbook, [Update Azure-Modules met open-source runbook](#open-source).

2. Klik op **Update Azure-Modules**, ter bevestiging een melding wordt weergegeven dat wordt gevraagd of u wilt doorgaan.<br><br> ![Melding van Azure-Modules bijwerken](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Klik op **Ja** en begint met het updateproces voor de module. Het updateproces duurt ongeveer 15-20 minuten om bij te werken van de volgende modules:

   * Azure
   * Azure.Storage
   * AzureRm.Automation
   * AzureRm.Compute
   * AzureRm.Profile
   * AzureRm.Resources
   * AzureRm.Sql
   * AzureRm.Storage

     Als de modules al bijgewerkt zijn, klikt u vervolgens het proces is voltooid binnen een paar seconden. Wanneer de update is voltooid, krijgt u een melding.<br><br> ![Updatestatus van de Azure-Modules bijwerken](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

     De .NET core AzureRm-modules (AzureRm.*. Core) worden niet ondersteund in Azure Automation en kan niet worden geïmporteerd.

> [!NOTE]
> Azure Automation maakt gebruik van de meest recente modules in uw Automation-account wanneer een nieuwe geplande taak wordt uitgevoerd.  

Als u van deze modules Azure PowerShell-cmdlets in uw runbooks gebruiken, wilt u dit updateproces iedere maand uitvoeren of dus om ervoor te zorgen dat u de meest recente modules hebt. Azure Automation maakt gebruik van de `AzureRunAsConnection` verbinding te verifiëren bij het bijwerken van de modules. Als de service-principal is verlopen of niet meer op het abonnementsniveau bestaat, mislukt de module-update.

## <a name="known-issues"></a>Bekende problemen

Er is een bekend probleem met het bijwerken van de AzureRM-modules in een Automation-Account dat in een resourcegroep gemaakt met een numerieke naam die met 0 begint. Voor het bijwerken van uw Azure-modules in uw Automation-Account, moet deze zich in een resourcegroep met een alfanumerieke naam. Resourcegroepen met numerieke namen die beginnen met 0 zijn kan niet worden bijgewerkt AzureRM-modules op dit moment.

## <a name="next-steps"></a>Volgende stappen

Ga naar de open-source [Update Azure-modules runbook](https://github.com/Microsoft/AzureAutomation-Account-Modules-Update) voor meer informatie over deze.
