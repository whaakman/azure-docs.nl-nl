---
title: Naslaginformatie over PowerShell-cmdlets - Azure Scheduler
description: Meer informatie over PowerShell-cmdlets voor Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 9a26c457-d7a1-4e4a-bc79-f26592155218
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: a06439150ac255e7b436082ecc88702bf0c1dec1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991093"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Naslaginformatie over PowerShell-cmdlets voor Azure Scheduler

> [!IMPORTANT]
> [Met Azure Logic Apps](../logic-apps/logic-apps-overview.md) vervangt Azure Scheduler, is buiten gebruik gesteld. Voor het plannen van taken, [Azure Logic Apps in plaats daarvan probeert](../scheduler/migrate-from-scheduler-to-logic-apps.md). 

Als u wilt scripts voor het maken en beheren van Scheduler-taken en -taakcollecties ontwerpt, kunt u PowerShell-cmdlets. In dit artikel geeft een lijst van de primaire [PowerShell-cmdlets voor Azure Scheduler](/powershell/module/azurerm.scheduler) met koppelingen naar hun naslaginformatie. Zie voor informatie over het installeren van Azure PowerShell voor uw Azure-abonnement [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview). Voor meer informatie over [Azure Resource Manager-cmdlets](/powershell/azure/overview), Zie [met behulp van Azure PowerShell met Azure Resource Manager](../powershell-azure-resource-manager.md).

| Cmdlet | Beschrijving |
|--------|-------------|
| [Disable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |Hiermee schakelt u een taakverzameling. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |Hiermee kunt een taakverzameling. |
| [Get-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Hiermee haalt u Scheduler-taken. |
| [Get-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |Hiermee haalt u taakverzamelingen. |
| [Get-AzureRmSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |Haalt de taakgeschiedenis op. |
| [New-AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Hiermee maakt u een HTTP-taak. |
| [New-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |Hiermee maakt u een taakverzameling. |
| [New-AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) | Hiermee maakt u een Service Bus-wachtrij-taak. |
| [New-AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Hiermee maakt u een Service Bus-onderwerp-taak. |
| [New-AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Maakt een taak van de queue Storage. |
| [Remove-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Hiermee verwijdert u een Scheduler-taak. |
| [Remove-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |Hiermee verwijdert u een taakverzameling. |
| [Set-AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Hiermee wijzigt u een HTTP-Scheduler-taak. |
| [Set-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |Hiermee wijzigt u een taakverzameling. |
| [Set-AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Hiermee wijzigt u een Service Bus-wachtrij-taak. |
| [Set-AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Hiermee wijzigt u de taak van een Service Bus-onderwerp. |
| [Set-AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Hiermee wijzigt u een taak van de queue Storage. |
||| 

Voor meer informatie kunt u een van deze cmdlets uitvoeren: 

```
Get-Help <cmdlet name> -Detailed
Get-Help <cmdlet name> -Examples
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Zie ook

* [Wat is Azure Scheduler?](scheduler-intro.md)
* [Concepten en terminologie entiteitenhiërarchie](scheduler-concepts-terms.md)
* [Maken en plannen van uw eerste taak - Azure portal](scheduler-get-started-portal.md)
* [Naslaginformatie over REST API van Azure Scheduler](https://msdn.microsoft.com/library/mt629143)
