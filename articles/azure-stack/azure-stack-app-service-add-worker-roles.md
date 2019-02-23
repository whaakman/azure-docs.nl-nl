---
title: 'Scale-out werkrollen in App Services: Azure Stack | Microsoft Docs'
description: Gedetailleerde richtlijnen voor het schalen van Azure Stack App Services
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2010
ms.author: jeffgilb
ms.reviewer: anwestg
ms.lastreviewed: 06/08/2018
ms.openlocfilehash: 839fa7fe8374f1f85b019178d4c3fe53f7137372
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729641"
---
# <a name="app-service-on-azure-stack-add-more-infrastructure-or-worker-roles"></a>App Service op Azure Stack: Meer functies voor infrastructuur- of werkrollen toevoegen

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*  

Dit document bevat instructies over het schalen van App Service op Azure Stack-infrastructuur- en werkrollen rollen. Het bevat stappen voor het maken van extra werkrollen ter ondersteuning van toepassingen van elke grootte.

> [!NOTE]
> Als uw Azure Stack-omgeving niet meer dan 96 GB RAM-geheugen heeft, wellicht u problemen bij het toevoegen van extra capaciteit.

App Service in Azure Stack, standaard, biedt ondersteuning voor gratis en gedeelde werkrolniveaus. Als u wilt andere werkrolniveaus toevoegen, moet u meer werkrollen toevoegen.

Als u niet zeker weet wat is geïmplementeerd met de standaard-App Service op Azure Stack-installatie, kunt u aanvullende informatie in controleren de [App Service op Azure Stack-overzicht](azure-stack-app-service-overview.md).

Azure App Service in Azure Stack implementeert alle rollen met behulp van Virtual Machine Scale Sets en daarom profiteert van de schalingsmogelijkheden van deze workload. Daarom wordt alle schalen van de werkrolniveaus uitgevoerd via de App Service-beheerder.

## <a name="add-additional-workers-with-powershell"></a>Toevoegen van extra werkrollen met PowerShell

1. [De beheerder van Azure Stack-omgeving in PowerShell instellen](azure-stack-powershell-configure-admin.md)

2. Dit voorbeeld gebruiken om uit de schaalset te schalen:
   ```powershell
   
    ##### Scale out the AppService Role instances ######
   
    # Set context to AzureStack admin.
    Login-AzureRmAccount -EnvironmentName AzureStackAdmin
                                                 
    ## Name of the Resource group where AppService is deployed.
    $AppServiceResourceGroupName = "AppService.local"

    ## Name of the ScaleSet : e.g. FrontEndsScaleSet, ManagementServersScaleSet, PublishersScaleSet , LargeWorkerTierScaleSet,      MediumWorkerTierScaleSet, SmallWorkerTierScaleSet, SharedWorkerTierScaleSet
    $ScaleSetName = "SharedWorkerTierScaleSet"

    ## TotalCapacity is sum of the instances needed at the end of operation. 
    ## e.g. if your VMSS has 1 instance(s) currently and you need 1 more the TotalCapacity should be set to 2
    $TotalCapacity = 2  

    # Get current scale set
    $vmss = Get-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -VMScaleSetName $ScaleSetName

    # Set and update the capacity
    $vmss.sku.capacity = $TotalCapacity
    Update-AzureRmVmss -ResourceGroupName $AppServiceResourceGroupName -Name $ScaleSetName -VirtualMachineScaleSet $vmss 
   ```    

   > [!NOTE]
   > Deze stap kan duren voordat een aantal uren in beslag, afhankelijk van het type van de rol en het aantal exemplaren.
   >
   >

3. Bewaakt de status van de nieuwe rolinstanties in de App Service-beheer, om te controleren of de status van een afzonderlijke rolinstantie klikt u op het Roltype in de lijst.

## <a name="add-additional-workers-using-the-administration-portal"></a>Toevoegen van extra workers met behulp van de beheerportal

1. Meld u aan de Azure Stack-beheerportal als de service-beheerder.

2. Blader naar **App Services**.

    ![](media/azure-stack-app-service-add-worker-roles/image01.png)

3. Klik op **rollen**. Hier ziet u de verdeling van alle App Service-rollen geïmplementeerd.

4. Klik met de rechtermuisknop op de rij van het type dat u wilt schalen en klik vervolgens op **ScaleSet**.

    ![](media/azure-stack-app-service-add-worker-roles/image02.png)

5. Klik op **schalen**, selecteert u het aantal exemplaren dat u wilt schalen, en klik vervolgens op **opslaan**.

    ![](media/azure-stack-app-service-add-worker-roles/image03.png)

6. App Service in Azure Stack wordt nu de aanvullende virtuele machines toevoegt, ze configureren, de vereiste software installeren en deze markeren als gereed wanneer dit proces voltooid is. Dit proces kan ongeveer 80 minuten duren.

7. U kunt de voortgang van de gereedheid van de nieuwe functies door de werknemers in weer te geven de **rollen** blade.

## <a name="result"></a>Resultaat

Nadat ze volledig geïmplementeerde en gereed is zijn, wordt de werknemers beschikbaar voor gebruikers om hun werkbelasting op te implementeren. Hieronder ziet u een voorbeeld van de verschillende beschikbare Prijscategorieën standaard. Als er geen beschikbare werkrollen voor een bepaalde werkrolniveau, is de optie voor het kiezen van de bijbehorende prijscategorie is niet beschikbaar.

![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> Als u wilt schalen Management, toevoegen Front-End of uitgever rollen dat moet u het bijbehorende Roltype uitschalen. 
>
>

Als u wilt schalen Management, Front-End of functies van de uitgever, volgt u dezelfde stappen selecteren van het type van de juiste rol. Domeincontrollers worden niet geïmplementeerd als de Schaalsets en daarom twee moeten worden geïmplementeerd tijdens de installatie voor alle productie-implementaties.

### <a name="next-steps"></a>Volgende stappen

[Implementatiebronnen configureren](azure-stack-app-service-configure-deployment-sources.md)
