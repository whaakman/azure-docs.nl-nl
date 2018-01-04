---
title: Resources met PowerShell en de sjabloon implementeren | Microsoft Docs
description: Azure Resource Manager en Azure PowerShell gebruiken voor het implementeren van een bronnen in Azure. De resources zijn gedefinieerd in een Resource Manager-sjabloon.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2017
ms.author: tomfitz
ms.openlocfilehash: 1210b2da9126c24b59e8ef59b50742a17b2e740d
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Resources implementeren met Resource Manager-sjablonen en Azure PowerShell

In dit artikel wordt uitgelegd hoe u Azure PowerShell gebruiken met Resource Manager-sjablonen voor het implementeren van uw resources in Azure. Als u niet bekend met concepten voor het implementeren bent en beheren van uw Azure-oplossingen, Zie [overzicht van Azure Resource Manager](resource-group-overview.md).

De Resource Manager-sjabloon die u implementeert, kan een lokaal bestand op uw computer of een extern bestand dat zich bevindt in een zoals GitHub-opslagplaats. De sjabloon die u in dit artikel implementeert vindt u in de [voorbeeldsjabloon](#sample-template) sectie, of als [storage accountsjabloon in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

Installeer zo nodig de Azure PowerShell-module met behulp van de instructies in de [Azure PowerShell handleiding](/powershell/azure/overview), en voer vervolgens `Login-AzureRmAccount` geen verbinding maken met Azure.

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a>Een sjabloon implementeren vanuit uw lokale computer

Bij het implementeren van resources in Azure, u:

1. Aanmelden bij uw Azure-account
2. Maak een resourcegroep die als de container voor de geïmplementeerde resources fungeert. De naam van de resourcegroep kan alleen alfanumerieke tekens, punten, onderstrepingstekens, afbreekstreepjes en haakjes bevatten. Het kan maximaal 90 tekens zijn. Het mag niet eindigen op een punt.
3. De sjabloon waarin de bronnen te maken aan de resourcegroep implementeren

Een sjabloon kunt opnemen parameters waarmee u de implementatie aanpassen. U kunt bijvoorbeeld waarden die zijn toegesneden opgeven voor een bepaalde omgeving (zoals ontwikkelen, testen en productie). De voorbeeldsjabloon definieert een parameter voor de SKU van het opslagaccount.

Het volgende voorbeeld maakt u een resourcegroep en implementeert een sjabloon op basis van uw lokale computer:

```powershell
Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
 
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

De implementatie kan enkele minuten duren. Wanneer deze is voltooid, ziet u een bericht die het resultaat bevat:

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a>Een sjabloon implementeren vanuit een externe bron

In plaats van Resource Manager-sjablonen worden opgeslagen op uw lokale computer, kunt u ook deze opslaan op een externe locatie. U kunt sjablonen opslaan in een resourcebeheerbibliotheek (zoals GitHub). Of u kunt ze opslaan in Azure storage-account voor gedeelde toegang in uw organisatie.

Een externe als sjabloon wilt implementeren, gebruiken de **TemplateUri** parameter. Gebruik de URI in het voorbeeld voor het voorbeeldsjabloon implementeren vanuit GitHub.

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

Het vorige voorbeeld vereist een openbaar toegankelijke URI voor de sjabloon die voor de meeste scenario werkt, omdat de sjabloon mag geen gevoelige gegevens bevatten. Als u nodig hebt om op te geven gevoelige gegevens (zoals een beheerderswachtwoord), wordt die waarde als beveiligde parameter doorgeven. Echter, als u niet wilt dat uw sjabloon openbaar toegankelijk is, kunt u deze beschermen door op te slaan deze in een persoonlijke opslagcontainer. Zie voor meer informatie over het implementeren van een sjabloon waarvoor een shared access signature (SAS)-token [persoonlijke sjabloon implementeren met SAS-token](resource-manager-powershell-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

Gebruik de volgende opdrachten in de Cloud-Shell:

```powershell
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile "C:\users\ContainerAdministrator\CloudDrive\templates\azuredeploy.json" -storageAccountType Standard_GRS
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

Als u wilt doorgeven in een lokale parameterbestand, gebruiken de **TemplateParameterFile** parameter:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

Voor het doorgeven van een externe parameterbestand, gebruiken de **TemplateParameterUri** parameter:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

U kunt de parameters inline en een parameter local-bestand in dezelfde implementatiebewerking gebruiken. U kunt bijvoorbeeld waarden opgeven in het lokale parameterbestand en toevoegen van andere waarden inline tijdens de implementatie. Als u waarden voor een parameter in de lokale parameterbestand- en inline opgeeft, wordt de waarde inline voorrang.

Echter, wanneer u een parameterbestand externe gebruikt, u kunt niet doorgeven andere waarden beide inline of vanuit een lokaal bestand. Wanneer u een parameterbestand in de **TemplateParameterUri** parameter, alle inline parameters worden genegeerd. Alle parameterwaarden in het externe bestand opgeven. Als uw sjabloon een gevoelige waarde waarmee u niet opnemen in het parameterbestand bevat, die waarde toevoegen aan een sleutelkluis of dynamisch bieden alle parameter waarden inline.

Als de sjabloon een parameter met dezelfde naam als een van de parameters in de PowerShell-opdracht is bevat PowerShell biedt de parameter van de sjabloon voor de postfix **FromTemplate**. Bijvoorbeeld, een parameter genaamd **ResourceGroupName** in uw sjabloon veroorzaakt een conflict met de **ResourceGroupName** parameter in de [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet. U wordt gevraagd om een waarde voor **ResourceGroupNameFromTemplate**. In het algemeen moet u deze verwarring niet door de naam geen parameters met dezelfde naam als parameters die worden gebruikt voor implementatiebewerkingen.

## <a name="test-a-template-deployment"></a>Een sjabloonimplementatie testen

U kunt uw sjabloon en de parameterbestanden waarden testen zonder het distribueren van alle resources met [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment). 

```powershell
Test-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

Als er geen fouten worden aangetroffen, wordt de opdracht is voltooid zonder reactie. Als er een fout wordt gedetecteerd, wordt een foutbericht geretourneerd door de opdracht. Poging om door te geven van een onjuiste waarde voor het opslagaccount SKU, resulteert bijvoorbeeld de volgende fout:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'The provided value 'badSku' for the template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. The parameter value is not part of the allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

Als uw sjabloon een syntaxisfout heeft, foutmelding de opdracht een die wijzen op dat de sjabloon kan niet worden geparseerd. Het bericht geeft de regelnummer en positie van de fout bij het parseren.

```powershell
Test-AzureRmResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

Volledige modus, gebruikt de `Mode` parameter:

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
```

## <a name="sample-template"></a>Voorbeeldsjabloon

De volgende sjabloon wordt gebruikt voor de voorbeelden in dit artikel. Kopiëren en opslaan als een bestand met de naam storage.json. Om te begrijpen hoe u deze sjabloon maakt, Zie [maken van uw eerste Azure Resource Manager-sjabloon](resource-manager-create-first-template.md).  

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
* De voorbeelden in dit artikel implementeren resources naar een resourcegroep in uw standaardabonnement. Zie voor het gebruik van een ander abonnement [meerdere Azure-abonnementen beheren](/powershell/azure/manage-subscriptions-azureps).
* Zie voor een compleet codevoorbeeld-script waarmee een sjabloon wordt geïmplementeerd, [script voor implementatie van Resource Manager-sjabloon](resource-manager-samples-powershell-deploy.md).
* Om te begrijpen hoe parameters in de sjabloon definieert, Zie [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* Zie voor tips over het oplossen van algemene implementatiefouten [oplossen van veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Zie voor meer informatie over het implementeren van een sjabloon waarvoor een SAS-token [persoonlijke sjabloon implementeren met SAS-token](resource-manager-powershell-sas-token.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).

