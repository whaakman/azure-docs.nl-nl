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
ms.openlocfilehash: 5c80e86699d671994a0989b99c0f97ebe2680592
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2019
ms.locfileid: "59044994"
---
# <a name="powershell-cmdlets-reference-for-azure-scheduler"></a>Naslaginformatie over PowerShell-cmdlets voor Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) vervangt Azure Scheduler, dat buiten gebruik wordt gesteld. [Probeer in plaats daarvan Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) als u taken wilt plannen. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u wilt scripts voor het maken en beheren van Scheduler-taken en -taakcollecties ontwerpt, kunt u PowerShell-cmdlets. In dit artikel geeft een lijst van de primaire [PowerShell-cmdlets voor Azure Scheduler](/powershell/module/azurerm.scheduler) met koppelingen naar hun naslaginformatie. Zie voor informatie over het installeren van Azure PowerShell voor uw Azure-abonnement [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview). Voor meer informatie over [Azure Resource Manager-cmdlets](/powershell/azure/overview), Zie [met behulp van Azure PowerShell met Azure Resource Manager](../powershell-azure-resource-manager.md).

| Cmdlet | Description |
|--------|-------------|
| [Disable-AzSchedulerJobCollection](/powershell/module/az.scheduler/disable-azschedulerjobcollection) |Hiermee schakelt u een taakverzameling. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/az.scheduler/enable-azschedulerjobcollection) |Hiermee kunt een taakverzameling. |
| [Get-AzSchedulerJob](/powershell/module/az.scheduler/get-azschedulerjob) |Hiermee haalt u Scheduler-taken. |
| [Get-AzSchedulerJobCollection](/powershell/module/az.scheduler/get-azschedulerjobcollection) |Hiermee haalt u taakverzamelingen op. |
| [Get-AzSchedulerJobHistory](/powershell/module/az.scheduler/get-azschedulerjobhistory) |Hiermee haalt u de taakgeschiedenis op. |
| [New-AzSchedulerHttpJob](/powershell/module/az.scheduler/new-azschedulerhttpjob) |Hiermee maakt u een HTTP-taak. |
| [New-AzSchedulerJobCollection](/powershell/module/az.scheduler/new-azschedulerjobcollection) |Hiermee maakt u een taakverzameling. |
| [New-AzSchedulerServiceBusQueueJob](/powershell/module/az.scheduler/new-azschedulerservicebusqueuejob) | Hiermee maakt u een Service Bus-wachtrij-taak. |
| [New-AzSchedulerServiceBusTopicJob](/powershell/module/az.scheduler/new-azschedulerservicebustopicjob) |Hiermee maakt u een Service Bus-onderwerp-taak. |
| [New-AzSchedulerStorageQueueJob](/powershell/module/az.scheduler/new-azschedulerstoragequeuejob) |Maakt een taak van de queue Storage. |
| [Remove-AzSchedulerJob](/powershell/module/az.scheduler/remove-azschedulerjob) |Hiermee verwijdert u een Scheduler-taak. |
| [Remove-AzSchedulerJobCollection](/powershell/module/az.scheduler/remove-azschedulerjobcollection) |Hiermee verwijdert u een taakverzameling. |
| [Set-AzSchedulerHttpJob](/powershell/module/az.scheduler/set-azschedulerhttpjob) |Hiermee wijzigt u een HTTP-Scheduler-taak. |
| [Set-AzSchedulerJobCollection](/powershell/module/az.scheduler/set-azschedulerjobcollection) |Hiermee wijzigt u een taakverzameling. |
| [Set-AzSchedulerServiceBusQueueJob](/powershell/module/az.scheduler/set-azschedulerservicebusqueuejob) |Hiermee wijzigt u een Service Bus-wachtrij-taak. |
| [Set-AzSchedulerServiceBusTopicJob](/powershell/module/az.scheduler/set-azschedulerservicebustopicjob) |Hiermee wijzigt u de taak van een Service Bus-onderwerp. |
| [Set-AzSchedulerStorageQueueJob](/powershell/module/az.scheduler/set-azschedulerstoragequeuejob) |Hiermee wijzigt u een taak van de queue Storage. |
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
