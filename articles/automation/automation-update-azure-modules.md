---
title: Bijwerken van de Azure-modules in Azure Automation | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt algemene Azure PowerShell-modules standaard meegeleverd in Azure Automation nu bijwerken.
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/13/2017
ms.author: magoedte
ms.openlocfilehash: f5e7c66cfd26bd6927d48ffd8bc0f82e9a3e2d13
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a>Het bijwerken van Azure PowerShell-modules in Azure Automation

De meest voorkomende Azure PowerShell-modules zijn standaard in elk Automation-account opgegeven.  Het team van Azure de Azure-modules worden regelmatig updates, zodat het Automation-account we een manier bieden om de modules in het account bijwerken wanneer er nieuwe versies beschikbaar via de portal zijn.  

Omdat modules regelmatig door de productgroep bijgewerkt worden, worden wijzigingen kunnen optreden met de cmdlets opgenomen die mogelijk negatieve invloed hebben op uw runbooks afhankelijk van het type wijziging, zoals de naam van een parameter of een cmdlet volledig bestandstypen. Om te voorkomen die invloed hebben op uw runbooks en de processen die ze automatiseren, is het raadzaam dat u testen en te voordat u doorgaat valideren.  Als u een speciale voor dit doel bestemde Automation-account niet hebt, kunt u een maken zodat u testen veel verschillende scenario's en permutaties tijdens de ontwikkeling van uw runbooks bovendien iteratieve wijzigingen zoals het bijwerken van kunt de PowerShell-modules.  Nadat de resultaten worden gevalideerd en u vereiste wijzigingen hebt toegepast, doorgaan met de coördinatie van de migratie van alle runbooks die wijziging vereist en de update uitvoeren, zoals hieronder wordt beschreven in de productieomgeving.     

## <a name="updating-azure-modules"></a>Bijwerken van de Azure-Modules

1. Op de pagina Modules van uw Automation-account, is de optie **Azure-Modules WU**. Het is altijd ingeschakeld.<br><br> ![De optie Azure Modules in Modules pagina bijwerken](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

2. Klik op **Azure-Modules WU** en krijgt u een bevestigingsbericht weergegeven waarin u wordt gevraagd of u wilt doorgaan.<br><br> ![Azure-Modules updatebericht](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Klik op **Ja** en begint het updateproces van de module. Het updateproces duurt ongeveer 15-20 minuten om bij te werken van de volgende modules:

  * Azure
  * Azure.Storage
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Als de modules al up-to-date te houden, klikt u vervolgens het proces is voltooid binnen een paar seconden. U ontvangt een melding wanneer de update is voltooid.<br><br> ![Azure-Modules updatestatus bijwerken](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> Azure Automation maakt gebruik van de meest recente modules in uw Automation-account wanneer een nieuwe geplande taak wordt uitgevoerd.    

Als u cmdlets uit deze Azure PowerShell-modules in uw runbooks gebruikt om Azure-resources te beheren, klikt u voor dit updateproces elke maand of dus voor zorgen dat u de meest recente modules hebben.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over integratiemodules en het maken van aangepaste modules voor het Automation verder te integreren met andere systemen, services of oplossingen, [integratiemodules](automation-integration-modules.md).

* Overweeg het gebruik van bron besturingselement integratie [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) of [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) centraal beheren en versies van uw Automation-runbook en configuratie portfolio beheren.  