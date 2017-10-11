---
title: Azure Cloud Services beheren met Azure Automation | Microsoft Docs
description: Meer informatie over hoe Azure Automation-service kan worden gebruikt voor het beheren van Azure-cloud-services op grote schaal.
services: cloud-services, automation
documentationcenter: 
author: jodoglevy
manager: timlt
editor: 
ms.assetid: 3789810a-2892-4eef-bf29-c781c1b5af48
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2016
ms.author: timlt
ms.openlocfilehash: 6b5acac1b8647c324988c316cd5602b3dba98a1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a>Azure Cloud Services met behulp van Azure Automation beheren
Deze handleiding vindt u de Azure Automation-service en hoe deze kan worden gebruikt voor het vereenvoudigen van beheer van uw Azure-cloud-services.

## <a name="what-is-azure-automation"></a>Wat is Azure Automation?
[Azure Automation](https://azure.microsoft.com/services/automation/) is een Azure-service voor het cloudbeheer via procesautomatisering vereenvoudigen. Met behulp van Azure Automation, worden langlopende, handmatige, foutgevoelige en regelmatig herhaalde taken geautomatiseerd om betrouwbaarheid, efficiëntie en tijd voor de waarde voor uw organisatie te verhogen.

Azure Automation biedt een engine voor het uitvoeren van maximaal betrouwbaar en maximaal beschikbare werkstroom die schaalbaar is om te voldoen aan de behoeften van uw wanneer uw organisatie groeit. In Azure Automation kunnen processen worden gestarte handmatig, door systemen van derden 3rd of met regelmatige tussenpozen zodat taken gebeuren precies wanneer deze nodig is.

Lagere operationele overhead en vrijmaken IT / DevOps-personeel te concentreren op het werk dat zakelijke voegt waarde door uw cloud-beheertaken automatisch wordt uitgevoerd door Azure Automation te verplaatsen.

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a>Hoe kan Azure Automation helpen Azure-cloudservices beheren
Azure-cloud-services in Azure Automation kunnen worden beheerd met behulp van de PowerShell-cmdlets die beschikbaar zijn in de [hulpprogramma's van Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure Automation heeft deze cloud service PowerShell-cmdlets beschikbaar gebruiksklaar, zodat u al uw cloud service management-taken in de service kunt uitvoeren. U kunt ook deze cmdlets in Azure Automation met de cmdlets voor andere Azure-services, complexe om taken te automatiseren via Azure-services en 3e systemen van derden koppelen.

Sommige voorbeeld maakt gebruik van Azure Automation voor het beheren van Azure Cloud Services omvatten:

* [Continue implementatie van een Cloudservice wanneer cscfg of cspkg wordt bijgewerkt in Azure Blob-opslag](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [Cloud Service-exemplaren parallel één upgradedomein tegelijk opnieuw op te starten](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Azure Automation en hoe deze kan worden gebruikt voor het beheren van Azure-cloudservices hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure Automation.

* [Overzicht van Azure Automation](../automation/automation-intro.md)
* [Mijn eerste runbook](../automation/automation-first-runbook-graphical.md)
* [Azure Automation-leeroverzicht](https://azure.microsoft.com/documentation/learning-paths/automation/)
