---
title: Azure-sjabloon met SAS-token en PowerShell implementeren | Microsoft Docs
description: Azure Resource Manager en Azure PowerShell gebruiken voor het implementeren van resources in Azure uit een sjabloon die wordt beveiligd met SAS-token.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 1e3cea027b599e2b1af1ced0fdf14e2cc8a0db82
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-powershell"></a>Met SAS-token en Azure PowerShell persoonlijke Resource Manager-sjabloon implementeren

Wanneer de sjabloon bevindt zich in een opslagaccount, kunt u het beperken van toegang aan de sjabloon en een shared access signature (SAS)-token opgeven tijdens de implementatie. In dit onderwerp wordt uitgelegd hoe u Azure PowerShell gebruiken met Resource Manager-sjablonen te geven van een SAS-token tijdens de implementatie. 

## <a name="add-private-template-to-storage-account"></a>Persoonlijke sjabloon toevoegen aan de storage-account

U kunt uw sjablonen toevoegen aan een opslagaccount en een koppeling naar deze tijdens de implementatie met een SAS-token.

> [!IMPORTANT]
> De blob met de sjabloon is door de onderstaande stappen te volgen, toegankelijk voor de eigenaar van het account. De blob is echter toegankelijk voor iedereen met de URI die bij het maken van een SAS-token voor de blob. Als een andere gebruiker de URI onderschept, kan die gebruiker toegang tot de sjabloon. Met behulp van een SAS-token is een prima manier voor het beperken van toegang tot uw sjablonen moet, maar u geen gevoelige gegevens, zoals wachtwoorden rechtstreeks in de sjabloon.
> 
> 

Het volgende voorbeeld stelt u een persoonlijke opslag account-container en een sjabloon geüpload:
   
```powershell
# create a storage account for templates
New-AzureRmResourceGroup -Name ManageGroup -Location "South Central US"
New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name} -Type Standard_LRS -Location "West US"
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# create a container and upload template
New-AzureStorageContainer -Name templates -Permission Off
Set-AzureStorageBlobContent -Container templates -File c:\MyTemplates\storage.json
```

## <a name="provide-sas-token-during-deployment"></a>SAS-token opgeven tijdens de implementatie
Een SAS-token genereren en deze opnemen in de URI voor de sjabloon voor het implementeren van een persoonlijke sjabloon in een opslagaccount. Stel het verlooptijdstip voldoende tijd laten om de implementatie te vervolledigen.
   
```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# get the URI with the SAS token
$templateuri = New-AzureStorageBlobSASToken -Container templates -Blob storage.json -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri $templateuri
```

Zie voor een voorbeeld van het gebruik van een SAS-token met gekoppelde sjablonen [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).


## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het implementeren van sjablonen [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](resource-group-template-deploy.md).
* Zie voor een compleet codevoorbeeld-script waarmee een sjabloon wordt geïmplementeerd, [sjabloonscript Resource Manager implementeren](resource-manager-samples-powershell-deploy.md)
* Om parameters te definiëren in de sjabloon, Zie [sjablonen](resource-group-authoring-templates.md#parameters).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).

