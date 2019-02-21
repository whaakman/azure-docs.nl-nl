---
title: 'App Service op Azure Stack: Domein bijwerken fault | Microsoft Docs'
description: Over het distribueren van Azure App Service in Azure Stack in domeinen met fouten
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: jeffgilb
ms.reviewer: anwestg
ms.lastreviewed: 09/05/2018
ms.openlocfilehash: c8c81d95a1179efff3bfdab49c2bc63cb074f73b
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447885"
---
# <a name="how-to-redistribute-azure-app-service-on-azure-stack-across-fault-domains"></a>Over het distribueren van Azure App Service in Azure Stack in domeinen met fouten

*Van toepassing op: Azure Stack-geïntegreerde systemen*

Met de update 1802 ondersteunt Azure Stack nu de distributie van workloads in domeinen met fouten, een functie die essentieel zijn voor hoge beschikbaarheid.

>[!IMPORTANT]
>Als u wilt profiteren van ondersteuning voor fout met betrekking tot domeinen, moet u uw geïntegreerde Azure Stack-systeem bijwerken naar 1802. Dit document is alleen van toepassing op App Service resource provider implementaties die zijn voltooid voordat de update 1802. Als u App Service in Azure Stack geïmplementeerd nadat de update 1802 is toegepast op Azure Stack, wordt de resourceprovider al verdeeld over foutdomeinen.

## <a name="rebalance-an-app-service-resource-provider-across-fault-domains"></a>Een App Service-resourceprovider herverdelen in domeinen met fouten

Als u wilt distribueren de schaalsets die is geïmplementeerd voor de resourceprovider App Service, moet u de stappen in dit artikel voor elke schaalset uitvoeren. Standaard zijn de namen van schaal:

* ManagementServersScaleSet
* FrontEndsScaleSet
* PublishersScaleSet
* SharedWorkerTierScaleSet
* SmallWorkerTierScaleSet
* MediumWorkerTierScaleSet
* LargeWorkerTierScaleSet

>[!NOTE]
> Als u geen instanties zijn geïmplementeerd in enkele van de schaalsets van de werknemer-laag, moet u niet opnieuw verdelen van de schaalsets. De schaalsets wordt correct worden verdeeld wanneer u deze in de toekomst.

Als u wilt schalen de schaalsets, de volgende stappen uit:

1. Aanmelden bij de Azure Stack-Beheerdersportal.
1. Selecteer **Alle services**.
2. In de **COMPUTE** categorie, selecteer **virtuele-machineschaalsets**. Wordt het bestaande schaalsets worden geïmplementeerd als onderdeel van de App Service-implementatie met instance count wordt informatie weergegeven. De volgende Schermafbeelding toont een voorbeeld van schaalsets.

      ![Azure App Service Scale Sets die worden vermeld in Virtual Machine Scale Sets UX][1]

1. Voor elke set schalen. Bijvoorbeeld, hebt u drie bestaande exemplaren in de schaalset u moet de schaal vergroten tot 6, zodat de drie nieuwe instanties worden geïmplementeerd op verschillende foutdomeinen. De volgende PowerShell-voorbeeld laat zien van uit de schaalset te schalen.

   ```powershell
   Add-AzureRmAccount -EnvironmentName AzureStackAdmin 

   # Get current scale set
   $vmss = Get-AzureRmVmss -ResourceGroupName "AppService.local" -VMScaleSetName "SmallWorkerTierScaleSet"

   # Set and update the capacity of your scale set
   $vmss.sku.capacity = 6
   Update-AzureRmVmss -ResourceGroupName AppService.local" -Name "SmallWorkerTierScaleSet" -VirtualMachineScaleSet $vmss
   ```

   >[!NOTE]
   >Deze stap kan duren voordat een aantal uren in beslag, afhankelijk van het type van de rol en het aantal exemplaren.

1. In **App Service-beheerdersrollen**, bewaakt de status van de nieuwe rolinstanties. Om te controleren of de status van een rolinstantie, selecteert u het Roltype in de lijst

    ![Azure App Service onder Azure Stack-rollen][2]

1. Wanneer de status van de nieuwe rolinstanties is **gereed**, gaat u terug naar **virtuele-Machineschaalset** en **verwijderen** de oude rolinstanties.

1. Herhaal deze stappen voor **elke** virtuele-machineschaalset.

## <a name="next-steps"></a>Volgende stappen

U kunt ook proberen om andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md).

* [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md)
* [MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-fault-domain-update/app-service-scale-sets.png
[2]: ./media/azure-stack-app-service-fault-domain-update/app-service-roles.png
