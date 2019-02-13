---
title: Implementeer Azure-sjabloon met SAS-token en PowerShell | Microsoft Docs
description: Azure Resource Manager en Azure PowerShell gebruiken om resources te implementeren naar Azure met een sjabloon die is beveiligd met SAS-token.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 837effaa2b699d51a420609415396a11a0d9892c
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56113206"
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-powershell"></a>Persoonlijke Resource Manager-sjablonen met SAS-token en Azure PowerShell implementeren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Wanneer de sjabloon bevindt zich in een storage-account, kunt u beperken van toegang aan de sjabloon en de token van een shared access signature (SAS) opgeven tijdens de implementatie. In dit onderwerp wordt uitgelegd hoe u Azure PowerShell gebruiken met Resource Manager-sjablonen voor het opgeven van een SAS-token tijdens de implementatie. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="add-private-template-to-storage-account"></a>Persoonlijke sjabloon met storage-account toevoegen

U kunt uw sjablonen toevoegen aan een storage-account en een koppeling naar deze tijdens de implementatie met een SAS-token.

> [!IMPORTANT]
> De blob met de sjabloon is door de onderstaande stappen te volgen, toegankelijk voor de eigenaar van het account. De blob is echter toegankelijk voor iedereen met deze URI bij het maken van een SAS-token voor de blob. Als een andere gebruiker de URI onderschept, kan die gebruiker toegang tot de sjabloon. Met behulp van een SAS-token is een goede manier van het beperken van toegang tot uw sjablonen, maar u moet geen gevoelige gegevens zoals wachtwoorden rechtstreeks in de sjabloon.
> 
> 

Het volgende voorbeeld stelt u een persoonlijke account van de opslagcontainer en uploadt een sjabloon:
   
```powershell
# create a storage account for templates
New-AzResourceGroup -Name ManageGroup -Location "South Central US"
New-AzStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name} -Type Standard_LRS -Location "West US"
Set-AzCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# create a container and upload template
New-AzStorageContainer -Name templates -Permission Off
Set-AzStorageBlobContent -Container templates -File c:\MyTemplates\storage.json
```

## <a name="provide-sas-token-during-deployment"></a>SAS-token opgeven tijdens de implementatie
Een SAS-token genereren voor het implementeren van een persoonlijke sjablonen in een opslagaccount, en deze opnemen in de URI voor de sjabloon. Stel het verlooptijdstip om toe te staan voldoende tijd voor het voltooien van de implementatie.
   
```powershell
Set-AzCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# get the URI with the SAS token
$templateuri = New-AzStorageBlobSASToken -Container templates -Blob storage.json -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri $templateuri
```

Zie voor een voorbeeld van het gebruik van een SAS-token met gekoppelde sjablonen [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).


## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het implementeren van sjablonen, [resources implementeren met Resource Manager-sjablonen en Azure PowerShell](resource-group-template-deploy.md).
* Zie voor een volledig voorbeeld van een script waarmee een sjabloon wordt geïmplementeerd, [sjabloonscript voor implementatie van Resource Manager-](resource-manager-samples-powershell-deploy.md)
* Zie voor het definiëren van parameters in sjabloon, [-sjablonen maken](resource-group-authoring-templates.md#parameters).
