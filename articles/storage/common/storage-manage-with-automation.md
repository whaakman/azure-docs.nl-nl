---
title: Azure Storage met behulp van Azure Automation beheren
description: Meer informatie over hoe Azure Automation-service kan worden gebruikt voor het beheren van Azure Storage op schaal.
services: storage, automation
author: jodoglevy
ms.service: storage
ms.topic: article
ms.date: 05/23/2016
ms.author: eamono
ms.component: common
ms.openlocfilehash: 82ec929c8d3055187a83179432fc601baa191cc4
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526365"
---
# <a name="managing-azure-storage-using-azure-automation"></a>Azure Storage met behulp van Azure Automation beheren
Deze handleiding vindt u de Azure Automation-service en hoe deze kan worden gebruikt voor een vereenvoudigd beheer van uw Azure Storage-blobs, tabellen en wachtrijen.

## <a name="what-is-azure-automation"></a>Wat is Azure Automation?
[Azure Automation](https://azure.microsoft.com/services/automation/) is een Azure-service voor cloudbeheer via procesautomatisering te vereenvoudigen. Met Azure Automation, langlopende, handmatige, foutgevoelige en regelmatig herhaalde taken kunnen worden geautomatiseerd om betrouwbaarheid en efficiëntie te verhogen en beperk de waarde voor uw organisatie.

Azure Automation biedt een uiterst betrouwbare en maximaal beschikbare engine voor werkstroomuitvoering die kan worden geschaald om te voldoen aan uw behoeften wanneer uw organisatie groeit. In Azure Automation, kunnen processen worden gestart handmatig, door systemen 3rd derden of met regelmatige tussenpozen zodat taken gebeuren precies wanneer dat nodig is.

Lagere operationele overhead en vrij IT / DevOps-personeel te concentreren op werk dat wordt toegevoegd business value door te verplaatsen van uw cloud-beheertaken automatisch wordt uitgevoerd door Azure Automation.

## <a name="how-can-azure-automation-help-manage-azure-storage"></a>Hoe u kunt Azure Automation beheren van Azure Storage?
Azure-opslag kan worden beheerd in Azure Automation met behulp van de PowerShell-cmdlets die beschikbaar zijn in [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure Automation heeft deze opslag-PowerShell-cmdlets beschikbaar gebruiksklaar, zodat u kunt alle van de blob-, tabel en wachtrij beheertaken in de service uitvoeren. U kunt ook deze cmdlets in Azure Automation met de cmdlets voor andere Azure-services voor het automatiseren van complexe taken in Azure-services en 3e systemen van derden worden gekoppeld.

De [Azure Automation runbook gallery](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) bevat tal van product-team en community-runbooks om te beginnen met het automatiseren van beheer van Azure Storage, andere Azure-services en 3e systemen van derden. Runbooks uit de galerie zijn onder andere:

* [Azure Storage-Blobs die bepaalde dagen oud met behulp van Automation-Service zijn verwijderen](https://gallery.technet.microsoft.com/scriptcenter/Remove-Storage-Blobs-that-aae4b761)
* [Een Blob downloaden uit Azure Storage](https://gallery.technet.microsoft.com/scriptcenter/a-Blob-from-Azure-Storage-6bc13745)
* [Back-up van alle schijven voor een enkele virtuele machine van Azure of voor alle virtuele machines in een Cloud Service](https://gallery.technet.microsoft.com/scriptcenter/Backup-all-disks-for-a-ede940d5)

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van Azure Automation en hoe deze kan worden gebruikt voor het beheren van Azure Storage-blobs, tabellen en wachtrijen hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Automation.

Zie de zelfstudie voor Azure Automation [maken of importeren van een runbook in Azure Automation](../../automation/automation-creating-importing-runbook.md).

