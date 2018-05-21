---
title: Scale-out werkrollen in App Services - Stack in Azure | Microsoft Docs
description: Gedetailleerde richtlijnen voor het schalen van Azure Stack App Services
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: anwestg
ms.reviewer: brenduns
ms.openlocfilehash: 04a93bc841d553296dca7635151c14892970121c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
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
   > Deze stap duurt enkele uren in beslag, afhankelijk van het type van de rol en het aantal exemplaren.
   >
   >

3. Bewaakt de status van de nieuwe rolexemplaren van de in het App Service-beheer, om te controleren op de status van een afzonderlijke instantie van het type van de rol in de lijst.

## <a name="add-additional-workers-directly-within-the-app-service-resource-provider-admin"></a>Toevoegen van extra werknemers rechtstreeks vanuit de App Service-beheerder met Resource-Provider.

1. Meld u aan bij de Azure-Stack-beheerportal als de servicebeheerder.

2. Blader naar **App Services**.

    ![](media/azure-stack-app-service-add-worker-roles/image01.png)

3. Klik op **rollen**. Hier ziet u de verdeling van alle rollen in App Service geïmplementeerd.

4. Klik met de rechtermuisknop op de rij van het type dat u wilt schalen en klik vervolgens op **ScaleSet**.

    ![](media/azure-stack-app-service-add-worker-roles/image02.png)

5. Klik op **schaal**, selecteer het aantal exemplaren dat u wilt aanpassen aan en klik vervolgens op **opslaan**.

    ![](media/azure-stack-app-service-add-worker-roles/image03.png)

6. App-Service op Azure-Stack wordt nu de extra virtuele machines toevoegen, om ze te configureren, de vereiste software installeren en gemarkeerd als gereed wanneer dit proces voltooid is. Dit kan ongeveer 80 minuten duren.

7. U kunt de voortgang van de gereedheid van de nieuwe rollen aan de hand van de werknemers in de **rollen** blade.

## <a name="result"></a>Resultaat

Nadat ze volledig geïmplementeerde en klaar bent, worden de werknemers beschikbaar voor gebruikers van hun werkbelasting naar ze te implementeren. Hieronder ziet u een voorbeeld van de beschikbare meerdere Prijscategorieën standaard. Als er geen beschikbare werknemers voor een bepaalde worker-laag, is de optie voor het kiezen van de bijbehorende prijscategorie is niet beschikbaar.

![](media/azure-stack-app-service-add-worker-roles/image04.png)

>[!NOTE]
> Als u wilt uitbreiden Management, toevoegen front-end- of Publisher rollen dat moet u het bijbehorende Roltype uitbreiden. 
>
>

Als u wilt uitbreiden Management, Front-End of Publisher rollen, volgt u dezelfde stappen selecteren van het type van de juiste rol. Domeincontrollers worden niet geïmplementeerd als Schaalsets en dus twee moeten worden geïmplementeerd tijdens de installatie voor alle productie-implementaties.

### <a name="next-steps"></a>Volgende stappen

[Implementatiebronnen configureren](azure-stack-app-service-configure-deployment-sources.md)
