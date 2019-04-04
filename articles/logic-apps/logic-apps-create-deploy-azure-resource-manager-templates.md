---
title: Logische apps met Azure Resource Manager-sjablonen - Azure Logic Apps implementeren
description: Logische apps implementeren met behulp van Azure Resource Manager-sjablonen
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.date: 10/15/2017
ms.openlocfilehash: 7543859a916de97d471db2894887e640db51dfc2
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893415"
---
# <a name="deploy-logic-apps-with-azure-resource-manager-templates"></a>Logische apps met Azure Resource Manager-sjablonen implementeren

Nadat u een Azure Resource Manager-sjabloon voor het implementeren van uw logische app maakt, kunt u de sjabloon op de volgende manieren implementeren:

* [Azure Portal](#portal)
* [Azure PowerShell](#powershell)
* [Azure-CLI](#cli)
* [Azure Resource Manager REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)
* [Azure DevOps-Azure-pijplijnen](#azure-pipelines)

<a name="portal"></a>

## <a name="deploy-through-azure-portal"></a>Implementeren via Azure portal

Als u wilt implementeren automatisch een sjabloon voor logische app in Azure, kunt u de volgende **implementeren in Azure** knop, waarmee u zich aanmeldt bij Azure portal en wordt u gevraagd voor informatie over uw logische app. U Breng eventueel benodigde wijzigingen aan de sjabloon voor logische app of de parameters.

[![Deploy naar Azure](./media/logic-apps-create-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

Bijvoorbeeld: u wordt gevraagd om deze gegevens nadat u zich aanmeldt bij Azure portal:

* De naam van de Azure-abonnement
* Resourcegroep die u wilt gebruiken
* Logic app-locatie
* De naam voor uw logische app
* Een test-URI
* Instemming met de opgegeven voorwaarden en bepalingen

Zie voor meer informatie, [resources implementeren met Azure Resource Manager-sjablonen en Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md).

## <a name="authorize-oauth-connections"></a>OAuth-verbindingen toestaan

Na de implementatie werkt de logische app end-to-end met geldige parameters op. U moet echter nog steeds OAuth-verbindingen voor het genereren van een geldige toegangstoken autoriseren. Voor geautomatiseerde implementaties kunt u een script dat u met elke OAuth-verbinding, zoals dit instemt [voorbeeldscript in het project GitHub LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth). Ook kunt u OAuth-verbindingen via de Azure-portal of in Visual Studio autoriseren door het openen van uw logische app in de ontwerper van logische Apps.

<a name="powershell"></a>

## <a name="deploy-with-azure-powershell"></a>Implementeren met Azure PowerShell

Om te implementeren op een specifieke *Azure-resourcegroep*, gebruikt u deze opdracht:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName <Azure-resource-group-name> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json 
```

Als u wilt implementeren op een specifieke Azure-abonnement, gebruikt u deze opdracht:

```powershell
New-AzDeployment -Location <location> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json 
```

* [Resources implementeren met Resource Manager-sjablonen en Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)
* [`New-AzResourceGroupDeployment`](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)
* [`New-AzDeployment`](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azdeployment)

<a name="cli"></a>

## <a name="deploy-with-azure-cli"></a>Implementeren met Azure CLI

Om te implementeren op een specifieke *Azure-resourcegroep*, gebruikt u deze opdracht:

```azurecli
az group deployment create -g <Azure-resource-group-name> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Als u wilt implementeren op een specifieke Azure-abonnement, gebruikt u deze opdracht:

```azurecli
az deployment create --location <location> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Zie de volgende onderwerpen voor meer informatie: 

* [Resources implementeren met Resource Manager-sjablonen en Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) 
* [`az group deployment create`](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)
* [`az deployment create`](https://docs.microsoft.com/cli/azure/deployment?view=azure-cli-latest#az-deployment-create)

<a name="azure-pipelines"></a>

## <a name="deploy-with-azure-devops"></a>Implementeren met Azure DevOps

Sjablonen voor logische Apps implementeren en beheren van omgevingen, teams vaak gebruik een hulpprogramma zoals [Azure pijplijnen](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines) in [Azure DevOps](https://docs.microsoft.com/azure/devops/user-guide/what-is-azure-devops-services). Azure pijplijnen biedt een [Azure Resource Group Deployment taak](https://github.com/Microsoft/azure-pipelines-tasks/tree/master/Tasks/AzureResourceGroupDeploymentV2) dat u kunt aan een build toevoegen of release-pijplijn.
Voor autorisatie om te implementeren en de release-pijplijn genereren, moet u ook een Azure Active Directory (AD) [service-principal](../active-directory/develop/app-objects-and-service-principals.md). Meer informatie over [met behulp van service-principals met Azure-pijplijnen](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure). 

Hier volgen algemene hoofdstappen voor het gebruik van Azure-pijplijnen:

1. Maak een lege pijplijn in Azure-pijplijnen.

1. Kies de resources die u nodig hebt voor de pijplijn, zoals uw sjabloon voor logische app en de sjabloon parameters bestanden, die u handmatig of als onderdeel van het bouwproces genereren.

1. Voor de agent-taak, zoeken en toevoegen de **Azure Resource Group Deployment** taak.

   !['Azure Resource Group Deployment'-taak toevoegen](./media/logic-apps-create-deploy-template/add-azure-resource-group-deployment-task.png)

1. Configureren met een [service-principal](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure). 

1. Verwijzingen naar de sjabloon voor logische app en de parameters-sjabloonbestanden toevoegen.

1. Doorgaan met het bouwen van de stappen in het uitgifteproces voor omgeving, geautomatiseerde test of goedkeurders indien nodig.

## <a name="get-support"></a>Ondersteuning krijgen

Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Logische apps bewaken](../logic-apps/logic-apps-monitor-your-logic-apps.md)
