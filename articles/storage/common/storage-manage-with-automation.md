---
title: Azure Storage met Azure Automation beheren
description: Meer informatie over hoe Azure Automation-service kan worden gebruikt voor het beheren van Azure Storage op grote schaal.
services: storage, automation
documentationcenter: 
author: jodoglevy
manager: eamono
editor: 
ms.assetid: bac41c39-1d95-46aa-a481-ec17bbb21b40
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: eamono
ms.openlocfilehash: 4649e42a628307e15f8b067503e4e8e13f16f1af
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="managing-azure-storage-using-azure-automation"></a>Azure Storage met Azure Automation beheren
Deze handleiding vindt u de Azure Automation-service en hoe deze kan worden gebruikt voor het vereenvoudigen van beheer van uw Azure Storage-blobs, tabellen en wachtrijen.

## <a name="what-is-azure-automation"></a>Wat is Azure Automation?
[Azure Automation](https://azure.microsoft.com/services/automation/) is een Azure-service voor het cloudbeheer via procesautomatisering vereenvoudigen. Met Azure Automation, langlopende, handmatige, foutgevoelige en regelmatig herhaalde taken kunnen worden geautomatiseerd om betrouwbaarheid en efficiëntie te verhogen en problemen sneller op de waarde voor uw organisatie.

Azure Automation biedt een engine voor het uitvoeren van maximaal betrouwbaar en maximaal beschikbare werkstroom die schaalbaar is om te voldoen aan de behoeften van uw wanneer uw organisatie groeit. In Azure Automation kunnen processen worden gestarte handmatig, door systemen van derden 3rd of met regelmatige tussenpozen zodat taken gebeuren precies wanneer deze nodig is.

Lagere operationele overhead en vrijmaken IT / DevOps-personeel te concentreren op het werk dat zakelijke voegt waarde door uw cloud-beheertaken automatisch wordt uitgevoerd door Azure Automation te verplaatsen.

## <a name="how-can-azure-automation-help-manage-azure-storage"></a>Hoe kan Azure Automation helpen Azure Storage beheren
Azure-opslag in Azure Automation kan worden beheerd met behulp van de PowerShell-cmdlets die beschikbaar zijn in [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure Automation heeft deze PowerShell-cmdlets Storage beschikbaar gebruiksklaar, zodat u alle van de blob, table en queue beheertaken in de service kunt uitvoeren. U kunt ook deze cmdlets in Azure Automation met de cmdlets voor andere Azure-services, complexe om taken te automatiseren via Azure-services en 3e systemen van derden koppelen.

De [galerie van Azure Automation-runbook](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) bevat een verscheidenheid aan product team en community runbooks om te beginnen met het automatiseren van beheer van Azure Storage, andere Azure-services en 3e systemen van derden. Galerie-runbooks zijn onder andere:

* [Azure Storage-Blobs die bepaalde dagen oud met behulp van Automation-Service zijn verwijderen](https://gallery.technet.microsoft.com/scriptcenter/Remove-Storage-Blobs-that-aae4b761)
* [Downloaden van een Blob naar Azure Storage](https://gallery.technet.microsoft.com/scriptcenter/a-Blob-from-Azure-Storage-6bc13745)
* [Back-up van alle schijven voor een enkele virtuele machine in Azure of voor alle VM's in een Cloud-Service](https://gallery.technet.microsoft.com/scriptcenter/Backup-all-disks-for-a-ede940d5)

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van Azure Automation en hoe deze kan worden gebruikt voor het beheren van Azure Storage-blobs, tabellen en wachtrijen hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Automation.

Zie de zelfstudie Azure Automation [maken of importeren van een runbook in Azure Automation](../../automation/automation-creating-importing-runbook.md).

