---
title: Azure API Management met behulp van Azure Automation beheren
description: Meer informatie over hoe Azure Automation-service kan worden gebruikt voor het beheren van Azure API Management.
services: api-management, automation
documentationcenter: ''
author: vladvino
manager: eamono
editor: ''
ms.assetid: 2e53c9af-f738-47f8-b1b6-593050a7c51b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: 10b483c70f7b5a3d767815306d8a690b1b9a5faf
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30161843"
---
# <a name="managing-azure-api-management-using-azure-automation"></a>Azure API Management met behulp van Azure Automation beheren
Deze handleiding vindt u de Azure Automation-service en hoe deze kan worden gebruikt voor het vereenvoudigen van beheer van Azure API Management.

## <a name="what-is-azure-automation"></a>Wat is Azure Automation?
[Azure Automation](https://azure.microsoft.com/services/automation/) is een Azure-service voor het cloudbeheer via procesautomatisering vereenvoudigen. Met behulp van Azure Automation, handmatige, kunnen herhaald, langlopende, foutgevoelige taken worden geautomatiseerd om betrouwbaarheid, efficiëntie en tijd voor de waarde voor uw organisatie te verhogen.

Azure Automation biedt een engine voor het uitvoeren van uiterst betrouwbare, maximaal beschikbare workflow die schaalbaar is om te voldoen aan uw behoeften. In Azure Automation kunnen processen worden gestarte handmatig, door systemen van derden 3rd of met regelmatige tussenpozen zodat taken gebeuren precies wanneer deze nodig is.

Operationele overhead verminderen en vrijmaken IT en DevOps-personeel te concentreren op het werk dat zakelijke meerwaarde door uw cloud-beheertaken automatisch wordt uitgevoerd door Azure Automation te verplaatsen.

## <a name="how-can-azure-automation-help-manage-azure-api-management"></a>Hoe kan Azure Automation helpen beheren van Azure API Management?
API Management in Azure Automation kunnen worden beheerd met behulp van de [Windows PowerShell-cmdlets voor Azure API Management-API](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/?view=azurermps-5.5.0#api_management/). In Azure Automation, kunt u PowerShell workflow-scripts om uit te voeren veel van uw API Management-taken met de cmdlets schrijven. U kunt ook deze cmdlets in Azure Automation met de cmdlets voor andere Azure-services, complexe om taken te automatiseren via Azure-services en 3e systemen van derden koppelen.

Hier volgen enkele voorbeelden van het gebruik van API Management met Automation:

* [Azure API Management-PowerShell gebruiken voor back-up en herstel](https://blogs.msdn.microsoft.com/katriend/2015/10/02/azure-api-management-using-powershell-for-backup-and-restore/)

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Azure Automation en hoe deze kan worden gebruikt voor het beheren van Azure API Management hebt geleerd, volgt u deze koppelingen voor meer informatie.

* Zie de Azure Automation [zelfstudie aan de slag](../automation/automation-first-runbook-graphical.md).

