---
title: Beheren van Azure Sleutelkluis met behulp van Azure Automation | Microsoft Docs
description: Meer informatie over hoe Azure Automation-service kan worden gebruikt voor het beheren van Azure Sleutelkluis.
services: Key-Vault, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 4e780762-19b6-4ca6-b894-ebb44c538f35
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte;csand
ms.openlocfilehash: dee39662472fe54776b591977f2b1ecb39d15b00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-key-vault-using-azure-automation"></a>Het beheren van Azure Sleutelkluis met behulp van Azure Automation
Deze handleiding vindt u de Azure Automation-service en hoe deze kan worden gebruikt voor het vereenvoudigen van beheer van uw sleutels en geheimen in Azure Sleutelkluis.

## <a name="what-is-azure-automation"></a>Wat is Azure Automation?
[Azure Automation](../automation/automation-intro.md) is een Azure-service voor het vereenvoudigen van beheer door automatisering van bedrijfsprocessen en configuratie van de gewenste status. Met behulp van Azure Automation, handmatige, kunnen herhaald, langlopende, foutgevoelige taken worden geautomatiseerd om betrouwbaarheid, efficiëntie en tijd voor de waarde voor uw organisatie te verhogen.

Azure Automation biedt een engine voor het uitvoeren van uiterst betrouwbare, maximaal beschikbare workflow die schaalbaar is om te voldoen aan uw behoeften. In Azure Automation kunnen processen worden gestarte handmatig, door systemen van derden 3rd of met regelmatige tussenpozen zodat taken gebeuren precies wanneer deze nodig is.

Operationele overhead verminderen en vrijmaken IT en DevOps-personeel te concentreren op het werk dat zakelijke meerwaarde door uw cloud-beheertaken automatisch wordt uitgevoerd door Azure Automation te verplaatsen.

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Hoe kan Azure Automation helpen beheren van Azure Sleutelkluis?
Sleutelkluis in Azure Automation kunnen worden beheerd met behulp van de [AzureRM Sleutelkluis-cmdlets](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) en [Azure Classic Sleutelkluis-cmdlets](https://msdn.microsoft.com/library/azure/dn868052.aspx). De Azure-module voor het beheren van klassieke Sleutelkluis is beschikbaar automatisch in Azure Automation en u kunt importeren de [AzureRM KeyVault-module](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) in Azure Automation, zodat u veel van de Sleutelkluis-beheertaken kunt uitvoeren in de service. U kunt ook deze cmdlets in Azure Automation met de cmdlets voor andere Azure-services, complexe om taken te automatiseren via Azure-services en 3e systemen van derden koppelen.

Met de Azure Sleutelkluis-cmdlets kunt u deze taken onder andere uitvoeren: 

* Maken en configureren van een sleutelkluis
* Maken of importeren van een sleutel
* Maken of bijwerken van een geheim
* Kenmerken van een sleutel bijwerken
* Een sleutel of geheim ophalen
* Een sleutel of geheim verwijderen

Hier volgen enkele voorbeelden van Sleutelkluis beheren met behulp van PowerShell:  

* [Azure Sleutelkluis - stap voor stap](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Installeren en configureren van een Azure Sleutelkluis](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Azure Automation en hoe deze kan worden gebruikt voor het beheren van Azure Sleutelkluis hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Automation.

* Zie de Azure Automation [zelfstudie aan de slag](../automation/automation-first-runbook-graphical.md).
* Zie de [Azure Key Vault PowerShell-scripts](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).

