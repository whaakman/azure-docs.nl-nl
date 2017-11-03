---
title: Implementeren van een Machine Learning-werkruimte met Azure Resource Manager | Microsoft Docs
description: Het implementeren van een werkruimte voor Azure Machine Learning met Azure Resource Manager-sjabloon
services: machine-learning
documentationcenter: 
author: ahgyger
manager: haining
editor: garye
ms.assetid: 4955ac4d-ff99-4908-aa27-69b6bfcc8e85
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/15/2017
ms.author: ahgyger
ms.openlocfilehash: e3cbcb8118aa05e554b2493506280d0e24706059
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Machine Learning-werkruimte implementeren met Azure Resource Manager
## <a name="introduction"></a>Inleiding
Met een Azure Resource Manager-implementatiesjabloon u tijd bespaart doordat u een schaalbare manier om te implementeren met elkaar verbonden onderdelen met een validatie en mechanisme voor opnieuw proberen. Als u Azure Machine Learning werkruimten instelt, bijvoorbeeld, moet u eerst een Azure storage-account configureert en vervolgens implementeert uw werkruimte. Stel dit handmatig te doen voor honderden werkruimten. Een eenvoudiger alternatief is voor het gebruik van een Azure Resource Manager-sjabloon voor het implementeren van een Azure Machine Learning-werkruimte en de afhankelijkheden ervan. In dit artikel leert u dit proces stapsgewijs. Zie voor een goed overzicht van Azure Resource Manager [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Stap voor stap: een Machine Learning-werkruimte maken
We wordt een Azure-resourcegroep maken en implementeren van een nieuwe Azure-opslagaccount en een nieuwe Azure Machine Learning-werkruimte met een Resource Manager-sjabloon. Zodra de implementatie voltooid is, wordt er belangrijke informatie over de werkruimten die zijn gemaakt (de primaire sleutel, de workspaceID en de URL naar de werkruimte) afgedrukt.

### <a name="create-an-azure-resource-manager-template"></a>Een Azure Resource Manager-sjabloon maken
Een Machine Learning-werkruimte is vereist voor het opslaan van de gegevensset die is gekoppeld aan Azure storage-account.
De volgende sjabloon maakt gebruik van de naam van de resourcegroep voor het genereren van de naam van het opslagaccount en de naam van de werkruimte.  Deze gebruikt ook de opslagaccountnaam als een eigenschap tijdens het maken van de werkruimte.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Deze sjabloon opslaan als mlworkspace.json bestand onder c:\temp\.

### <a name="deploy-the-resource-group-based-on-the-template"></a>De resourcegroep, op basis van de sjabloon implementeren
* Open PowerShell
* Modules voor Azure Resource Manager en Azure Service Management installeren  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Deze stappen downloaden en installeren van de modules die nodig zijn voor de resterende stappen. Dit hoeft slechts één keer worden uitgevoerd in de omgeving waarin u de PowerShell-opdrachten worden uitgevoerd.   

* Verificatie op Azure  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
Deze stap moet worden herhaald voor elke sessie. Eenmaal is geverifieerd, moet uw abonnementsgegevens worden weergegeven.

![Azure-Account][1]

Nu we hebben toegang tot Azure, kunnen we de resourcegroep maken.

* Een resourcegroep maken

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Controleer of dat de resourcegroep correct wordt ingericht. **ProvisioningState** moet worden "is voltooid."
Naam van de resourcegroep wordt gebruikt door de sjabloon voor het genereren van de opslagaccountnaam. De opslagaccountnaam moet tussen 3 en 24 tekens lang zijn en alleen cijfers en kleine letters gebruiken.

![Resourcegroep][2]

* Met behulp van de implementatie van de groep resource, implementeert u een nieuwe Machine Learning-werkruimte.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Zodra de implementatie is voltooid, is het eenvoudig om toegang tot eigenschappen van de werkruimte die u hebt geïmplementeerd. Bijvoorbeeld, u kunt toegang tot de primaire Key Token.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Ophalen van een bestaande werkruimte op een andere manier is het gebruik van de opdracht Invoke-AzureRmResourceAction. Bijvoorbeeld, kunt u de primaire en secundaire tokens van alle werkruimten aanbieden.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Nadat u de werkruimte is ingericht, kunt u ook veel Azure Machine Learning Studio taken automatiseren de [PowerShell-Module voor Azure Machine Learning](http://aka.ms/amlps).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Azure Resource Manager-sjablonen ontwerpen](../../azure-resource-manager/resource-group-authoring-templates.md). 
* Bekijk de [Azure Quickstart-opslagplaats voor sjablonen](https://github.com/Azure/azure-quickstart-templates). 
* Bekijk deze video over [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 

<!--Image references-->
[1]: ./media/deploy-with-resource-manager-template/azuresubscription.png
[2]: ./media/deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
