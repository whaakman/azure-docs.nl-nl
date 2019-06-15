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
ms.openlocfilehash: 53b68a5dc72277c9fd44b36d346e5b5c91b53a93
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60344373"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Naslaginformatie over PowerShell-cmdlets voor Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) vervangt Azure Scheduler, dat buiten gebruik wordt gesteld. [Probeer in plaats daarvan Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) als u taken wilt plannen. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u wilt scripts voor het maken en beheren van Scheduler-taken en -taakcollecties ontwerpt, kunt u PowerShell-cmdlets. Dit artikel worden de belangrijkste PowerShell-cmdlets voor Azure Scheduler met koppelingen naar hun naslaginformatie. Zie voor informatie over het installeren van Azure PowerShell voor uw Azure-abonnement [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview). Voor meer informatie over [Azure Resource Manager-cmdlets](/powershell/azure/overview), Zie [met behulp van Azure PowerShell met Azure Resource Manager](../powershell-azure-resource-manager.md).

| Cmdlet | Description |
|--------|-------------|
| [Disable-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |Hiermee schakelt u een taakverzameling. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |Hiermee kunt een taakverzameling. |
| [Get-AzSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Hiermee haalt u Scheduler-taken. |
| [Get-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |Hiermee haalt u taakverzamelingen. |
| [Get-AzSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |Haalt de taakgeschiedenis op. |
| [New-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Hiermee maakt u een HTTP-taak. |
| [New-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |Hiermee maakt u een taakverzameling. |
| [New-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) | Hiermee maakt u een Service Bus-wachtrij-taak. |
| [New-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Hiermee maakt u een Service Bus-onderwerp-taak. |
| [New-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Maakt een taak van de queue Storage. |
| [Remove-AzSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Hiermee verwijdert u een Scheduler-taak. |
| [Remove-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |Hiermee verwijdert u een taakverzameling. |
| [Set-AzSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Hiermee wijzigt u een HTTP-Scheduler-taak. |
| [Set-AzSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |Hiermee wijzigt u een taakverzameling. |
| [Set-AzSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Hiermee wijzigt u een Service Bus-wachtrij-taak. |
| [Set-AzSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Hiermee wijzigt u de taak van een Service Bus-onderwerp. |
| [Set-AzSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Hiermee wijzigt u een taak van de queue Storage. |
||| 

Voor meer informatie kunt u een van deze cmdlets uitvoeren: 

```
Get-Help <cmdlet name> -Detailed
Get-Help <cmdlet name> -Examples
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Zie ook

* [Wat is Azure Scheduler?](scheduler-intro.md)
* [Concepten, terminologie en entiteitenhiërarchie](scheduler-concepts-terms.md)
* [Maken en plannen van uw eerste taak - Azure portal](scheduler-get-started-portal.md)
* [Naslaginformatie over REST API van Azure Scheduler](https://msdn.microsoft.com/library/mt629143)
