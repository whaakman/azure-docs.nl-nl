---
title: Een aangepaste installatiekopie van Azure DevTest Labs maken van een VHD-bestand met behulp van PowerShell | Microsoft Docs
description: Automatiseer het maken van een aangepaste installatiekopie in Azure DevTest Labs van een VHD-bestand met behulp van PowerShell
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 7c18ac13b9663ad541ae206347a8df17ff06297c
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737379"
---
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a>Een aangepaste installatiekopie maken van een VHD-bestand met behulp van PowerShell

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

De volgende stappen helpen u bij het maken van een aangepaste installatiekopie van een VHD-bestand met behulp van PowerShell:

1. Bij een PowerShell-prompt, meld u aan bij uw Azure-account met de volgende aanroep naar de **Connect AzAccount** cmdlet.  
    
    ```PowerShell
    Connect-AzAccount
    ```

1.  Selecteer de gewenste Azure-abonnement door het aanroepen van de **Selecteer AzSubscription** cmdlet. Vervang de volgende tijdelijke aanduiding voor de **$subscriptionId** variabele met een geldig Azure-abonnement-ID. 

    ```PowerShell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzSubscription -SubscriptionId $subscriptionId
    ```

1.  Het lab-object ophalen door het aanroepen van de **Get-AzResource** cmdlet. Vervang de volgende tijdelijke aanduidingen voor de **$labRg** en **$labName** variabelen met de juiste waarden voor uw omgeving. 

    ```PowerShell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```
 
1.  In het lab storage-account en lab opslag accountsleutel in het lab-object ophalen. 

    ```PowerShell
    $labStorageAccount = Get-AzResource -ResourceId $lab.Properties.defaultStorageAccount 
    $labStorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value
    ```

1.  Vervang de volgende tijdelijke aanduiding voor de **$vhdUri** variabele met de URI moet het geüploade VHD-bestand. U kunt het VHD-bestand-URI ophalen uit blade met blob storage-account in Azure portal.

    ```PowerShell
    $vhdUri = '<Specify the VHD URI here>'
    ```

1.  Maak de aangepaste installatiekopie met de **New-AzResourceGroupDeployment** cmdlet. Vervang de volgende tijdelijke aanduidingen voor de **$customImageName** en **$customImageDescription** variabelen betekenisvolle namen voor uw omgeving.

    ```PowerShell
    $customImageName = '<Specify the custom image name>'
    $customImageDescription = '<Specify the custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-to-create-a-custom-image-from-a-vhd-file"></a>PowerShell-script voor het maken van een aangepaste installatiekopie van een VHD-bestand

De volgende PowerShell-script kan worden gebruikt om een aangepaste installatiekopie maken van een VHD-bestand. Vervang de tijdelijke aanduidingen (beginnen en eindigend met punthaken) door de juiste waarden voor uw behoeften. 

```PowerShell
# Log in to your Azure account.  
Connect-AzAccount

# Select the desired Azure subscription. 
$subscriptionId = '<Specify your subscription ID here>'
Select-AzSubscription -SubscriptionId $subscriptionId

# Get the lab object.
$labRg = '<Specify your lab resource group name here>'
$labName = '<Specify your lab name here>'
$lab = Get-AzResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)

# Get the lab storage account and lab storage account key values.
$labStorageAccount = Get-AzResource -ResourceId $lab.Properties.defaultStorageAccount 
$labStorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value

# Set the URI of the VHD file.  
$vhdUri = '<Specify the VHD URI here>'

# Set the custom image name and description values.
$customImageName = '<Specify the custom image name>'
$customImageDescription = '<Specify the custom image description>'

# Set up the parameters object.
$parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

# Create the custom image. 
New-AzResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
```

## <a name="related-blog-posts"></a>Gerelateerde blogberichten

- [Aangepaste installatiekopieën of formules?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Kopiëren van aangepaste installatiekopieën tussen Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Volgende stappen

- [Een VM toevoegen aan uw testomgeving](devtest-lab-add-vm.md)
