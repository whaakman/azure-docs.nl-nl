---
title: Azure Cloud Services beheren met Azure Automation | Microsoft Docs
description: Meer informatie over hoe Azure Automation-service kan worden gebruikt voor het beheren van Azure-cloud-services op grote schaal.
services: cloud-services, automation
documentationcenter: ''
author: jodoglevy
manager: timlt
editor: ''
ms.assetid: 3789810a-2892-4eef-bf29-c781c1b5af48
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2016
ms.author: timlt
ms.openlocfilehash: 40e91b4ba7ee62976b49975769bd1d1e656525f2
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54328865"
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a>Azure Cloud Services met Azure Automation beheren
Deze handleiding vindt u de Azure Automation-service en hoe deze kan worden gebruikt voor het vereenvoudigen van beheer van uw Azure-cloud-services.

## <a name="what-is-azure-automation"></a>Wat is Azure Automation?
[Azure Automation](https://azure.microsoft.com/services/automation/) is een Azure-service voor cloudbeheer via procesautomatisering te vereenvoudigen. Met Azure Automation, worden langlopende, handmatige, foutgevoelige en regelmatig herhaalde taken geautomatiseerd om de betrouwbaarheid, efficiëntie en tijd tot waarde voor uw organisatie te verhogen.

Azure Automation biedt een uiterst betrouwbare en maximaal beschikbare engine voor werkstroomuitvoering die kan worden geschaald om te voldoen aan uw behoeften wanneer uw organisatie groeit. In Azure Automation, kunnen processen worden gestart handmatig, door systemen 3rd derden of met regelmatige tussenpozen zodat taken gebeuren precies wanneer dat nodig is.

Lagere operationele overhead en vrij IT / DevOps-personeel te concentreren op werk dat wordt toegevoegd business value door te verplaatsen van uw cloud-beheertaken automatisch wordt uitgevoerd door Azure Automation.

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a>Hoe u kunt Azure Automation beheren Azure-cloudservices?
Azure cloudservices kunnen worden beheerd in Azure Automation met behulp van de PowerShell-cmdlets die beschikbaar zijn in de [Azure PowerShell-hulpprogramma's](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure Automation heeft deze cloud service PowerShell-cmdlets beschikbaar gebruiksklaar, zodat u al uw cloud service-beheertaken in de service kunt uitvoeren. U kunt ook deze cmdlets in Azure Automation met de cmdlets voor andere Azure-services voor het automatiseren van complexe taken in Azure-services en 3e systemen van derden worden gekoppeld.

Enkele gebruikstoepassingen voorbeeld van Azure Automation voor het beheren van Azure Cloud Services zijn onder andere:

* [Continue implementatie van een Service in de Cloud wanneer cscfg of cspkg worden bijgewerkt in Azure Blob-opslag](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [Cloud Service-exemplaren in parallelle, één upgradedomein tegelijk opnieuw opstarten](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisbeginselen van Azure Automation en hoe deze kan worden gebruikt voor het beheren van Azure-cloudservices hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Automation.

* [Overzicht van Azure Automation](../automation/automation-intro.md)
* [Mijn eerste runbook](../automation/automation-first-runbook-graphical.md)
* [Leertraject voor Azure Automation](https://azure.microsoft.com/documentation/learning-paths/automation/)
