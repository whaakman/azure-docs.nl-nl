---
title: 'App-Service op Azure Stack: Fault-domein Update | Microsoft Docs'
description: Het distribueren van Azure App Service op Azure-Stack in domeinen met fouten
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2018
ms.author: anwestg
ms.openlocfilehash: ce57e153dcab6a386150ebefe1ecb4a018514247
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130367"
---
# <a name="how-to-redistribute-azure-app-service-on-azure-stack-across-fault-domains"></a>Het distribueren van Azure App Service op Azure-Stack in domeinen met fouten

*Van toepassing op: Azure Stack geïntegreerd systemen*

Met de update 1802 ondersteunt Azure Stack nu de distributie van werkbelastingen tussen domeinen met fouten, een functie die van essentieel belang voor hoge beschikbaarheid.

>[!IMPORTANT]
>Om te profiteren van ondersteuning voor fouttolerantie domeinen, moet u uw Azure-Stack geïntegreerd systeem naar 1802 bijwerken. Dit document is alleen van toepassing op App Service resource provider-implementaties die zijn voltooid voordat u de update 1802. Als u App-Service op Azure-Stack geïmplementeerd nadat de update 1802 is toegepast op Azure-Stack, wordt de resourceprovider al verdeeld over de domeinen met fouten.

## <a name="rebalance-an-app-service-resource-provider-across-fault-domains"></a>Opnieuw verdelen van een App Service-resourceprovider tussen domeinen met fouten

Als u wilt de schaalsets geïmplementeerd voor de App Service-resourceprovider distribueren, moet u de stappen in dit artikel voor elke set scale uitvoeren. Standaard zijn de namen scaleset:

* ManagementServersScaleSet
* FrontEndsScaleSet
* PublishersScaleSet
* SharedWorkerTierScaleSet
* SmallWorkerTierScaleSet
* MediumWorkerTierScaleSet
* LargeWorkerTierScaleSet

>[!NOTE]
> Als er geen instanties zijn geïmplementeerd in sommige van de werknemer laag-schaalsets, moet u niet opnieuw verdelen die schaalsets. De schaalsets wordt correct worden verdeeld wanneer u deze in de toekomst schalen.

Als u wilt uitbreiden de schaalsets, de volgende stappen uit:

1. Meld u aan bij de Azure Portal in de Stack-beheerder.
2. Selecteer **meer services**.
3. Selecteer onder COMPUTE-, **virtuele-machineschaalsets**. Bestaande schaalsets geïmplementeerd als onderdeel van de App Service-implementatie wordt vermeld met exemplaar count-informatie. De volgende Schermafbeelding toont een voorbeeld van-schaalsets.

      ![Azure App Service Scale Sets die worden vermeld in de virtuele Machine Scale Sets UX][1]

4. Elke set uitschalen. Bijvoorbeeld, hebt u drie bestaande exemplaren in de schaalset u moet worden uitgebreid tot en met 6 zodat de drie nieuwe exemplaren zijn geïmplementeerd op domeinen met fouten. De volgende PowerShell-voorbeeld toont uit aan de schaalaanpassingsset worden uitgebreid.

   ```powershell
   Add-AzureRmAccount -EnvironmentName AzureStackAdmin 

   # Get current scale set
   $vmss = Get-AzureRmVmss -ResourceGroupName "AppService.local" -VMScaleSetName "SmallWorkerTierScaleSet"

   # Set and update the capacity of your scale set
   $vmss.sku.capacity = 6
   Update-AzureRmVmss -ResourceGroupName AppService.local" -Name "SmallWorkerTierScaleSet" -VirtualMachineScaleSet $vmss
   ```

   >[!NOTE]
   >Deze stap duurt enkele uren in beslag, afhankelijk van het type van de rol en het aantal exemplaren.

5. In **App Service-beheer rollen**, de status van de nieuwe rolexemplaren controleren. Om de status van een rolinstantie controleren, selecteert u het Roltype in de lijst

    ![Op rollen Azure Stack-Azure App Service][2]

6. Wanneer de status van de nieuwe rolexemplaren is **gereed**, gaat u terug naar **virtuele-Machineschaalset** en **verwijderen** de oude rolinstanties.

7. Herhaal deze stappen voor **elke** virtuele-machineschaalset.

## <a name="next-steps"></a>Volgende stappen

U kunt ook proberen om andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md).

* [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md)
* [MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-fault-domain-update/app-service-scale-sets.png
[2]: ./media/azure-stack-app-service-fault-domain-update/app-service-roles.png
