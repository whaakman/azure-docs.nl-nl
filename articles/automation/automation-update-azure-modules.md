---
title: Azure-modules in Azure Automation bijwerken
description: Dit artikel wordt beschreven hoe u kunt algemene Azure PowerShell-modules standaardvoorziening in Azure Automation nu bijwerken.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 12/04/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c0636a3e1fa50f90c68393aea910f36d38d8eaf5
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54437264"
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Het bijwerken van Azure PowerShell-modules in Azure Automation

De meest voorkomende Azure PowerShell-modules worden geboden door standaard in elk Automation-account. De Azure-modules worden regelmatig bijgewerkt in de Azure-team. In uw Automation-account krijgt u een manier om bij te werken van de modules in het account als nieuwe versies beschikbaar via de portal zijn.

> [!NOTE]
> De nieuwe [Az van Azure PowerShell-module](/powershell/azure/new-azureps-module-az?view=azurermps-6.13.0) worden niet ondersteund in Azure Automation.

Omdat modules regelmatig door de productgroep bijgewerkt worden, kunnen wijzigingen optreden met de cmdlets opgenomen. Deze actie kan een negatieve invloed hebben op uw runbooks afhankelijk van het type wijziging, zoals de naam van een parameter of volledig beëindigde van een cmdlet. Om te voorkomen dat dit gevolgen heeft voor uw runbooks en de processen die ze automatiseren, testen en te valideren voordat u doorgaat. Als u een speciale Automation-account dat is bestemd voor dit doel niet hebt, houd rekening met het maken van een zodat u veel verschillende scenario's tijdens de ontwikkeling van uw runbooks testen kunt. Deze test moet iteratieve wijzigingen zoals het bijwerken van de PowerShell-modules bevatten. Als u uw scripts lokaal ontwikkelt, is het aanbevolen dat de dezelfde moduleversies lokaal die u in uw Automation-Account hebt, wanneer de dezelfde resultaten testen om ervoor te zorgen dat u ontvangt. Nadat de resultaten worden gevalideerd en u de vereiste wijzigingen hebt toegepast, kunt u de wijzigingen kunt verplaatsen naar productie.

> [!NOTE]
> Een nieuw Automation-account kan niet de meest recente modules bevatten.

## <a name="updating-azure-modules"></a>Azure-Modules bijwerken

1. Op de pagina Modules van uw Automation-account, wordt de optie **Update Azure-Modules**. Het altijd ingeschakeld.<br><br> ![De optie Azure-Modules in Modules pagina bijwerken](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

  > [!NOTE]
  > Voordat u bijwerkt van uw Azure-modules die het wordt aanbevolen dat u werkt ze bij in een test Automation-Account om ervoor te zorgen dat werken uw bestaande scripts zoals verwacht voordat het bijwerken van uw Azure-modules.
  >
  > De **Update Azure-Modules** knop is alleen beschikbaar in de openbare cloud. het is niet beschikbaar in de [soevereine regio's](https://azure.microsoft.com/global-infrastructure/). Raadpleeg [alternatieve manieren om bij te werken uw modules](#alternative-ways-to-update-your-modules) sectie voor meer informatie.

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

## <a name="alternative-ways-to-update-your-modules"></a>Alternatieve manieren om uw modules bijwerken

Zoals gezegd, de **Update Azure-Modules** knop niet beschikbaar in soevereine clouds, dit is alleen beschikbaar in de globale Azure-cloud. Dit is vanwege het feit dat de nieuwste versie van de Azure PowerShell-modules uit de galerie met PowerShell niet met het Resource Manager-resources in deze clouds die momenteel zijn geïmplementeerd werkt mogelijk.

U kunt importeren en voer de [Update AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook om te proberen bij te werken van de Azure-modules in uw Automation-Account. Het is doorgaans een goed idee om alle Azure-modules op hetzelfde moment bijwerken. Maar dit proces kan mislukken als de versies die u probeert te importeren vanuit de galerie zijn niet compatibel is met de Azure-services met de doel-Azure-omgeving die momenteel zijn geïmplementeerd. Mogelijk moet u controleren of dat de compatibele versie van de modules zijn opgegeven in de runbookparameters.

Gebruik de `AzureRmEnvironment` parameter om door te geven van de juiste omgeving aan het runbook.  Acceptabele waarden zijn **AzureCloud**, **AzureChinaCloud**, **AzureGermanCloud**, en **AzureUSGovernment**. Deze waarden kunnen worden opgehaald uit met behulp van `Get-AzureRmEnvironment | select Name`. Als u niet een waarde voor deze parameter doorgeven, het runbook de openbare cloud van Azure wordt standaard **AzureCloud**

Als u een specifieke versie van de Azure PowerShell-module gebruiken in plaats van de meest recente beschikbare op de PowerShell Gallery wilt, deze versies doorgeven aan de optionele `ModuleVersionOverrides` parameter van de **Update AzureModule** runbook. Zie voor voorbeelden van de [Update AzureModule.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AzureModule.ps1) runbook. Azure PowerShell-modules die niet worden vermeld in de `ModuleVersionOverrides` parameter worden bijgewerkt met de meest recente moduleversies op de PowerShell Gallery. Als u niets om weer te geven de `ModuleVersionOverrides` parameter, alle modules worden bijgewerkt met de meest recente moduleversies op de PowerShell Gallery. Dit gedrag is hetzelfde als de **Update Azure-Modules** knop.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over integratiemodules en over het maken van aangepaste modules voor het verder Automation integreren met andere systemen, services of oplossingen, [integratiemodules](automation-integration-modules.md).


