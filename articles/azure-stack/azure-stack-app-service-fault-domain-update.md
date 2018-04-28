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
ms.date: 03/09/2018
ms.author: anwestg
ms.openlocfilehash: 42adef66fb1b1141ab44aab3a1ccdaae022202b5
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="how-to-redistribute-azure-app-service-on-azure-stack-across-fault-domains"></a>Het distribueren van Azure App Service op Azure-Stack in domeinen met fouten

*Van toepassing op: Azure Stack geïntegreerd systemen*

Met de update 1802 ondersteunt Azure Stack nu de distributie van werkbelastingen tussen domeinen met fouten, een functie, wat van essentieel belang voor hoge beschikbaarheid.

> [!IMPORTANT]
> Moet dat u uw Azure-Stack geïntegreerd systeem hebt bijgewerkt naar 1802 te kunnen profiteren van ondersteuning voor fouttolerantie domeinen.  Dit document is alleen van toepassing op App Service resource provider-implementaties die vóór de update 1802 zijn voltooid.  Als u hebt de App-Service op Azure-Stack geïmplementeerd nadat de update 1802 is toegepast op Azure-Stack, wordt de resourceprovider al verdeeld over de domeinen met fouten.
>
>

## <a name="rebalance-an-app-service-resource-provider-across-fault-domains"></a>Opnieuw verdelen van een App Service-resourceprovider tussen domeinen met fouten

Om de schaalsets geïmplementeerd voor de App Service-resourceprovider distribueren, moet u de volgende stappen uitvoeren voor elke schaalset.  Standaard zijn de namen scaleset:

* ManagementServersScaleSet
* FrontEndsScaleSet
* PublishersScaleSet
* SharedWorkerTierScaleSet
* SmallWorkerTierScaleSet
* MediumWorkerTierScaleSet
* LargeWorkerTierScaleSet

> [!NOTE]
> Als er geen instanties zijn geïmplementeerd in sommige van de werknemer laag-schaalsets, hoeft u niet opnieuw verdelen die schaalsets.  De schaalsets wordt correct worden verdeeld wanneer u deze in de toekomst schalen.
>
>

1. Blader naar de virtuele-Machineschaalsets die u in de Azure Portal in de Stack-beheerder.  Bestaande schaalsets geïmplementeerd als onderdeel van de App Service-implementatie wordt vermeld met exemplaar count-informatie.

    ![Azure App Service Scale Sets die worden vermeld in de virtuele Machine Scale Sets UX][1]

2. Volgende scale-out elke set.  Bijvoorbeeld, hebt u drie bestaande exemplaren in de schaalset u moet worden uitgebreid tot en met 6 zodat de drie nieuwe exemplaren worden ingericht in domeinen met fouten.
    a. [Instellen van de beheerder van Azure-Stack-omgeving in PowerShell](azure-stack-powershell-configure-admin.md) b. In dit voorbeeld moet worden uitgebreid in de schaalset gebruiken:
        ```powershell
                Add-AzureRmAccount -EnvironmentName AzureStackAdmin 

                # Get current scale set
                $vmss = Get-AzureRmVmss -ResourceGroupName "AppService.local" -VMScaleSetName "SmallWorkerTierScaleSet"

                # Set and update the capacity of your scale set
                $vmss.sku.capacity = 6
                Update-AzureRmVmss -ResourceGroupName "AppService.local" -Name "SmallWorkerTierScaleSet" -VirtualMachineScaleSet $vmss
        '''
> [!NOTE]
> Deze stap duurt enkele uren in beslag, afhankelijk van het type van de rol en het aantal exemplaren.
>
>

3. De status van de nieuwe rolinstanties in de blade App Service-beheer rollen bewaken.  Controleer de status van een afzonderlijke rolinstantie door te klikken op het type van de rol in de lijst

    ![Op rollen Azure Stack-Azure App Service][2]

4. Een nieuwe exemplaren zijn in een **gereed** staat, Ga terug naar de blade virtuele-Machineschaalset en **verwijderen** de oude exemplaren.

5. Herhaal deze stappen voor **elke** virtuele-machineschaalset.

## <a name="next-steps"></a>Volgende stappen

U kunt ook proberen om andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md).

* [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md)
* [MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md)

<!--Image references-->
[1]: ./media/azure-stack-app-service-fault-domain-update/app-service-scale-sets.png
[2]: ./media/azure-stack-app-service-fault-domain-update/app-service-roles.png
