---
title: Resources met Azure CLI en sjabloon implementeren | Microsoft Docs
description: Azure Resource Manager en Azure CLI gebruiken voor het implementeren van een bronnen in Azure. De resources zijn gedefinieerd in een Resource Manager-sjabloon.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: tomfitz
ms.openlocfilehash: 13154e41ebd4867de9af74340a69446400814f5a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Resources implementeren met Resource Manager-sjablonen en Azure CLI

In dit onderwerp wordt uitgelegd hoe u Azure CLI 2.0 gebruiken met Resource Manager-sjablonen voor het implementeren van uw resources in Azure. Als u niet bekend met concepten voor het implementeren bent en beheren van uw Azure-oplossingen, Zie [overzicht van Azure Resource Manager](resource-group-overview.md).  

De Resource Manager-sjabloon die u implementeert, kan een lokaal bestand op uw computer of een extern bestand dat zich bevindt in een zoals GitHub-opslagplaats. De sjabloon die u in dit artikel implementeert vindt u in de [voorbeeldsjabloon](#sample-template) sectie, of als een [storage accountsjabloon in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

Als u geen Azure CLI is geïnstalleerd, kunt u de [Cloud Shell](#deploy-template-from-cloud-shell).

## <a name="deploy-local-template"></a>Lokale sjabloon implementeren

Bij het implementeren van resources in Azure, u:

1. Aanmelden bij uw Azure-account
2. Maak een resourcegroep die als de container voor de geïmplementeerde resources fungeert. De naam van de resourcegroep kan alleen alfanumerieke tekens, punten, onderstrepingstekens, afbreekstreepjes en haakjes bevatten. Het kan maximaal 90 tekens zijn. Het mag niet eindigen op een punt.
3. De sjabloon waarin de bronnen te maken aan de resourcegroep implementeren

Een sjabloon kunt opnemen parameters waarmee u de implementatie aanpassen. U kunt bijvoorbeeld waarden die zijn toegesneden opgeven voor een bepaalde omgeving (zoals ontwikkelen, testen en productie). De voorbeeldsjabloon definieert een parameter voor de SKU van het opslagaccount. 

Het volgende voorbeeld maakt u een resourcegroep en implementeert een sjabloon op basis van uw lokale computer:

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

De implementatie kan enkele minuten duren. Wanneer deze is voltooid, ziet u een bericht die het resultaat bevat:

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a>Externe sjabloon implementeren

In plaats van Resource Manager-sjablonen worden opgeslagen op uw lokale computer, kunt u ook deze opslaan op een externe locatie. U kunt sjablonen opslaan in een resourcebeheerbibliotheek (zoals GitHub). Of u kunt ze opslaan in Azure storage-account voor gedeelde toegang in uw organisatie.

Een externe als sjabloon wilt implementeren, gebruiken de **sjabloon uri** parameter. Gebruik de URI in het voorbeeld voor het voorbeeldsjabloon implementeren vanuit GitHub.
   
```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters storageAccountType=Standard_GRS
```

Het vorige voorbeeld vereist een openbaar toegankelijke URI voor de sjabloon die voor de meeste scenario werkt, omdat de sjabloon mag geen gevoelige gegevens bevatten. Als u nodig hebt om op te geven gevoelige gegevens (zoals een beheerderswachtwoord), wordt die waarde als beveiligde parameter doorgeven. Echter, als u niet wilt dat uw sjabloon openbaar toegankelijk is, kunt u deze beschermen door op te slaan deze in een persoonlijke opslagcontainer. Zie voor meer informatie over het implementeren van een sjabloon waarvoor een shared access signature (SAS)-token [persoonlijke sjabloon implementeren met SAS-token](resource-manager-cli-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

Gebruik de volgende opdrachten in de Cloud-Shell:

   ```azurecli-interactive
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageAccountType=Standard_GRS
   ```

## <a name="parameter-files"></a>De parameterbestanden

In plaats van met parameters wordt doorgegeven als inline-waarden in het script, wellicht u eenvoudiger te gebruiken van een JSON-bestand met de parameterwaarden. De parameterbestand moet in de volgende indeling:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

U ziet dat de parameters-sectie bevat een parameternaam die overeenkomt met de parameter die is gedefinieerd in de sjabloon (storageAccountType). De parameter-bestand bevat een waarde voor de parameter. Deze waarde wordt automatisch doorgegeven aan de sjabloon tijdens de implementatie. U kunt meerdere parameterbestanden voor verschillende scenario's maken en geeft u het juiste parameterbestand. 

Het vorige voorbeeld kopiëren en opslaan als een bestand met de naam `storage.parameters.json`.

Gebruik voor het doorgeven van een lokale parameterbestand, `@` om op te geven van een lokaal bestand met de naam storage.parameters.json.

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a>Een sjabloonimplementatie testen

U kunt uw sjabloon en de parameterbestanden waarden testen zonder het distribueren van alle resources met [az implementatie valideren](/cli/azure/group/deployment#validate). 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

Als er geen fouten worden aangetroffen, retourneert de opdracht informatie over de testimplementatie. In het bijzonder u ziet dat de **fout** waarde null is.

```azurecli
{
  "error": null,
  "properties": {
      ...
```

Als er een fout wordt gedetecteerd, wordt een foutbericht geretourneerd door de opdracht. Poging om door te geven van een onjuiste waarde voor het opslagaccount SKU, resulteert bijvoorbeeld de volgende fout:

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'The provided value 'badSKU' for the template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. The parameter value is not part of the allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Als uw sjabloon een syntaxisfout heeft, foutmelding de opdracht een die wijzen op dat de sjabloon kan niet worden geparseerd. Het bericht geeft de regelnummer en positie van de fout bij het parseren.

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

Volledige modus, gebruikt de `mode` parameter:

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

## <a name="sample-template"></a>Voorbeeldsjabloon

De volgende sjabloon wordt gebruikt voor de voorbeelden in dit onderwerp. Kopiëren en opslaan als een bestand met de naam storage.json. Om te begrijpen hoe u deze sjabloon maakt, Zie [maken van uw eerste Azure Resource Manager-sjabloon](resource-manager-create-first-template.md).  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

## <a name="next-steps"></a>Volgende stappen
* De voorbeelden in dit artikel implementeren resources naar een resourcegroep in uw standaardabonnement. Zie voor het gebruik van een ander abonnement [meerdere Azure-abonnementen beheren](/cli/azure/manage-azure-subscriptions-azure-cli).
* Zie voor een compleet codevoorbeeld-script waarmee een sjabloon wordt geïmplementeerd, [script voor implementatie van Resource Manager-sjabloon](resource-manager-samples-cli-deploy.md).
* Om te begrijpen hoe parameters in de sjabloon definieert, Zie [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* Zie voor tips over het oplossen van algemene implementatiefouten [oplossen van veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Zie voor meer informatie over het implementeren van een sjabloon waarvoor een SAS-token [persoonlijke sjabloon implementeren met SAS-token](resource-manager-cli-sas-token.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).
