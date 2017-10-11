---
title: Azure-sjabloon met SAS-token en Azure CLI implementeren | Microsoft Docs
description: Azure Resource Manager en Azure CLI gebruiken voor het implementeren van resources in Azure uit een sjabloon die wordt beveiligd met SAS-token.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 22387aadd8f53a65efb76a29a9403c46a2c25954
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-cli"></a>Met SAS-token en Azure CLI persoonlijke Resource Manager-sjabloon implementeren

Wanneer de sjabloon bevindt zich in een opslagaccount, kunt u het beperken van toegang aan de sjabloon en een shared access signature (SAS)-token opgeven tijdens de implementatie. In dit onderwerp wordt uitgelegd hoe u Azure PowerShell gebruiken met Resource Manager-sjablonen te geven van een SAS-token tijdens de implementatie. 

## <a name="add-private-template-to-storage-account"></a>Persoonlijke sjabloon toevoegen aan de storage-account

U kunt uw sjablonen toevoegen aan een opslagaccount en een koppeling naar deze tijdens de implementatie met een SAS-token.

> [!IMPORTANT]
> De blob met de sjabloon is door de onderstaande stappen te volgen, toegankelijk voor de eigenaar van het account. De blob is echter toegankelijk voor iedereen met de URI die bij het maken van een SAS-token voor de blob. Als een andere gebruiker de URI onderschept, kan die gebruiker toegang tot de sjabloon. Met behulp van een SAS-token is een prima manier voor het beperken van toegang tot uw sjablonen moet, maar u geen gevoelige gegevens, zoals wachtwoorden rechtstreeks in de sjabloon.
> 
> 

Het volgende voorbeeld stelt u een persoonlijke opslag account-container en een sjabloon geüpload:
   
```azurecli
az group create --name "ManageGroup" --location "South Central US"
az storage account create \
    --resource-group ManageGroup \
    --location "South Central US" \
    --sku Standard_LRS \
    --kind Storage \
    --name {your-unique-name}
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
az storage container create \
    --name templates \
    --public-access Off \
    --connection-string $connection
az storage blob upload \
    --container-name templates \
    --file vmlinux.json \
    --name vmlinux.json \
    --connection-string $connection
```

### <a name="provide-sas-token-during-deployment"></a>SAS-token opgeven tijdens de implementatie
Een SAS-token genereren en deze opnemen in de URI voor de sjabloon voor het implementeren van een persoonlijke sjabloon in een opslagaccount. Stel het verlooptijdstip voldoende tijd laten om de implementatie te vervolledigen.
   
```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
token=$(az storage blob generate-sas \
    --container-name templates \
    --name vmlinux.json \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name vmlinux.json \
    --output tsv \
    --connection-string $connection)
az group deployment create --resource-group ExampleGroup --template-uri $url?$token
```

Zie voor een voorbeeld van het gebruik van een SAS-token met gekoppelde sjablonen [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md).

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het implementeren van sjablonen [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](resource-group-template-deploy-cli.md).
* Zie voor een compleet codevoorbeeld-script waarmee een sjabloon wordt geïmplementeerd, [sjabloonscript Resource Manager implementeren](resource-manager-samples-cli-deploy.md)
* Om parameters te definiëren in de sjabloon, Zie [sjablonen](resource-group-authoring-templates.md#parameters).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).
