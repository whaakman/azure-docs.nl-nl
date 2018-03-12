---
title: Scale-out werkrollen in App Services - Stack in Azure | Microsoft Docs
description: Gedetailleerde richtlijnen voor het schalen van Azure Stack App Services
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: d6471796863a80e69fdaf740b68fb27d59503453
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="app-service-on-azure-stack-add-more-infrastructure-or-worker-roles"></a>App-Service op Azure-Stack: meer infrastructuur of worker rollen toevoegen

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*  

Dit document bevat instructies over het schalen van App Service voor Azure-Stack infrastructuur- en werkrollen rollen. Deze bevat stappen voor het maken van aanvullende werkrollen ter ondersteuning van toepassingen van elke grootte.

> [!NOTE]
> Als uw Azure-Stack-omgeving niet meer dan 96 GB RAM-geheugen heeft, mogelijk hebt u problemen bij het toevoegen van extra capaciteit.

App-Service op Azure-Stack standaard ondersteunt gratis en gedeelde worker lagen. Als u wilt toevoegen andere worker-laag, moet u meer werkrollen toevoegen.

Als u niet zeker wat is geïmplementeerd met de standaard-App Service op Azure-Stack-installatie weet, kunt u meer informatie over in controleren de [App-Service op Azure-Stack overzicht](azure-stack-app-service-overview.md).

Azure App Service op Azure-Stack alle rollen met behulp van de virtuele-Machineschaalsets implementeert en als zodanig wordt gebruikgemaakt van de mogelijkheden voor vergroten/verkleinen van deze workload. Daarom wordt alle schalen van de werknemer lagen uitgevoerd via de App Service-beheerder.

> [!IMPORTANT]
> Het is momenteel niet mogelijk om te schalen van virtuele-machineschaalsets in de portal, zoals aangegeven in de Azure-Stack release-opmerkingen, Gebruik daarom het PowerShell-voorbeeld uit te schalen.
>
>

## <a name="add-additional-workers-with-powershell"></a>Toevoegen van extra werknemers met PowerShell

1. [De beheerder van Azure-Stack-omgeving in PowerShell instellen](azure-stack-powershell-configure-admin.md)
2. In dit voorbeeld moet worden uitgebreid in de schaalset gebruiken:
   ```powershell
   
    ##### Scale out the AppService Role instances ######
   
    # Set context to AzureStack admin.
    Login-AzureRMAccount -EnvironmentName AzureStackAdmin
                                                 
    ## Name of the Resource group where AppService is deployed.
    $AppServiceResourceGroupName = "AppService.local"

    ## Name of the ScaleSet : e.g. FrontEndsScaleSet, ManagementServersScaleSet, PublishersScaleSet , LargeWorkerTierScaleSet,      MediumWorkerTierScaleSet, SmallWorkerTierScaleSet, SharedWorkerTierScaleSet
    $ScaleSetName = "SharedWorkerTierScaleSet"

    ## TotalCapacity is sum of the instances needed at the end of operation. 
    ## e.g. if you VMSS has 1 instance(s) currently and you need 1 more the TotalCapacity should be set to 2
    $TotalCapacity = 2  

    # Get current scale set
    $vmss = Get-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -VMScaleSetName $ScaleSetName

    # Set and update the capacity
    $vmss.sku.capacity = $TotalCapacity
    Update-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -Name $ScaleSetName -VirtualMachineScaleSet $vmss 
  
    '''

> [!NOTE]
> This step can take a number of hours to complete depending on the type of role and the number of instances.
>
>

3. Monitor the status of the new role instances in the App Service Administration, to check the status of an individual role instance click the role type in the list.

## Add additional workers directly within the App Service Resource Provider Admin.

1. Log in to the Azure Stack administration portal as the service administrator.

2. Browse to **App Services**.

    ![](media/azure-stack-app-service-add-worker-roles/image01.png)

3. Click **Roles**. Here you see the breakdown of all App Service roles deployed.

4. Right click on the row of the type you want to scale and then click **ScaleSet**.

    ![](media/azure-stack-app-service-add-worker-roles/image02.png)

5. Click **Scaling**, select the number of instances you want to scale to, and then click **Save**.

    ![](media/azure-stack-app-service-add-worker-roles/image03.png)

6. App Service on Azure Stack will now add the additional VMs, configure them, install all the required software, and mark them as ready when this process is complete. This process can take approximately 80 minutes.

7. You can monitor the progress of the readiness of the new roles by viewing the workers in the **Roles** blade.

## Result

After they are fully deployed and ready, the workers become available for users to deploy their workload onto them. The following shows an example of the multiple pricing tiers available by default. If there are no available workers for a particular worker tier, the option to choose the corresponding pricing tier is unavailable.

![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> To scale out Management, Front End or Publisher roles add you must scale out the corresponding role type. 
>
>

To scale out Management, Front End, or Publisher roles, follow the same steps selecting the appropriate role type. Controllers are not deployed as Scale Sets and therefore two should be deployed at Installation time for all production deployments.

### Next steps

[Configure deployment sources](azure-stack-app-service-configure-deployment-sources.md)
