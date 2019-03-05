---
title: Azure-resources verplaatsen naar een andere resourcegroep | Microsoft Docs
description: Informatie over het verplaatsen van Azure-resources van de ene naar de andere resourcegroep of van het ene naar het andere abonnement.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 12/19/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 5bc8a408ce247ad2980983c3eb32807074af599f
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56820821"
---
# <a name="tutorial-move-azure-resources-to-another-resource-group"></a>Zelfstudie: Azure-resources verplaatsen naar een andere resourcegroep

Informatie over het verplaatsen van Azure-resources van de ene naar de andere resourcegroep. U kunt Azure-resources ook verplaatsen van het ene naar het andere Azure-abonnement. In deze zelfstudie gebruikt u een resourcemanagerssjabloon om twee resourcegroepen en één opslagaccount te implementeren. Vervolgens verplaatst u het opslagaccount van de ene naar de andere resourcegroep.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * De resources voorbereiden.
> * Controleren of de resource kan worden verplaatst.
> * Controlelijst voordat u de resource verplaatst.
> * De verplaatsingsbewerking valideren.
> * De resource verplaatsen.
> * Resources opschonen.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prepare-the-resources"></a>De resources voorbereiden

Er is een sjabloon gemaakt en in een [gedeeld opslagaccount](https://armtutorials.blob.core.windows.net/moveresources/azuredeploy.json) geplaatst. De sjabloon definieert twee resourcegroepen en één opslagaccount. Bij het implementeren van de sjabloon, dient u een projectnaam op te geven. De projectnaam wordt gebruikt om unieke resourcenamen te genereren.  De volgende JSON wordt uit de sjabloon geëxtraheerd:

```json
"variables": {
  "resourceGroupSource": {
    "name": "[concat(parameters('projectName'), 'rg1')]",
    "location": "eastus"
  },
  "resourceGroupDestination": {
    "name": "[concat(parameters('projectName'), 'rg2')]",
    "location": "westus"      
  },
  "storageAccount": {
    "name": "[concat(parameters('projectName'), 'store')]",
    "location": "eastus"
  }
},
```

Let op de locaties die in de JSON zijn gedefinieerd; de twee resourcegroepen bevinden zich in US - oost en US - west. Het opslagaccount bevindt zich in US - oost. Als u een resource naar een andere resourcegroep met een andere locatie verplaatst, wordt de locatie van de resourcegroep niet door deze verplaatsingsbewerking gewijzigd.

Selecteer **Probeer het nu** om de Cloud Shell te openen en voer vervolgens het PowerShell-script in de Cloud Shell uit:

```azurepowershell-interactive
$projectName = Read-Host -prompt "Enter a project name"
New-AzDeployment `
    -Name $projectname `
    -Location "centralus" `
    -TemplateUri "https://armtutorials.blob.core.windows.net/moveresources/azuredeploy.json" `
    -projectName $projectName
```

Wacht tot het script is uitgevoerd, open de [Azure-portal](https://portal.azure.com) en controleer of de resourcegroepen en het opslagaccount zijn geïmplementeerd zoals verwacht.

> [!NOTE]
> Omdat er twee resourcegroepen door de sjabloon worden gedefinieerd, wordt deze implementatie als een implementatie op abonnementsniveau beschouwd. De implementatie van de portalsjabloon ondersteunt geen implementaties op abonnementsniveau. Vandaar dat in deze zelfstudie Azure PowerShell wordt gebruikt. Azure CLI ondersteunt ook implementaties op abonnementsniveau. Zie [Resourcegroepen en resources voor een Azure-abonnement maken](./deploy-to-subscription.md).

## <a name="verify-the-resource-can-be-moved"></a>Controleren of de resource kan worden verplaatst

Niet alle resources kunnen worden verplaatst. De in deze zelfstudie gebruikte resource is een opslagaccount. Dit kan worden verplaatst. Zie [Services die kunnen worden verplaatst](./resource-group-move-resources.md#services-that-can-be-moved) om te controleren of een resource kan worden verplaatst.

## <a name="checklist-before-moving-resources"></a>Controlelijst voordat u de resource verplaatst

Deze stap is optioneel voor deze zelfstudie omdat deze al is uitgevoerd.

Voordat u een resource verplaatst, moeten er enkele belangrijke stappen worden uitgevoerd. Zie [Controlelijst voordat u de resource verplaatst](./resource-group-move-resources.md#checklist-before-moving-resources).

## <a name="validate-the-move"></a>De verplaatsing valideren

Het valideren van de verplaatsing is optioneel voor deze zelfstudie omdat deze al is uitgevoerd.

Met de bewerking voor het valideren van de verplaatsing kunt u het verplaatsingsscenario testen zonder dat u werkelijk de resources verplaatst. Gebruik deze bewerking om te bepalen of de verplaatsing zal lukken. Zie [De verplaatsing valideren](./resource-group-move-resources.md#validate-move) voor meer informatie.

## <a name="move-the-resource"></a>De resource verplaatsen

Het opslagaccount bevindt zich binnen in de bronresourcegroep (rg1). Voer het volgende PowerShell-script uit om de resource naar de doelresourcegroep (rg2) te verplaatsen. Gebruik dezelfde projectnaam die u hebt gebruikt bij het implementeren van de resources.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

```azurepowershell-interactive
$projectName = Read-Host -prompt "Enter a project name"
$resourceGroupSource = $projectName + "rg1"
$resourceGroupDestination = $projectName + "rg2"
$storageAccountName = $projectName + "store"

$storageAccount = Get-AzResource -ResourceGroupName $resourceGroupSource -ResourceName $storageAccountName
Move-AzResource -DestinationResourceGroupName $resourceGroupDestination -ResourceId $storageAccount.ResourceId
```

Open de [Azure-portal](https://portal.azure.com), controleer of het opslagaccount naar de andere resourcegroep is verplaatst en controleer ook of de locatie van het opslagaccount nog steeds US - oost is.

Als u resources verplaatst, wordt zowel de brongroep als de doelgroep tijdens de verplaatsingsbewerking vergrendeld. Schrijf- en verwijderingsbewerkingen voor de resourcegroepen worden vergrendeld tot de bewerking is voltooid. Deze vergrendeling betekent dat u resources in de resourcegroepen niet kunt toevoegen, bijwerken of verwijderen. Dit betekent niet dat de resources bevroren zijn. Als u bijvoorbeeld een SQL-server en de bijbehorende database naar een nieuwe resourcegroep verplaatst, heeft de toepassing die gebruikmaakt van de database geen last van downtime. De server kan nog steeds naar de database schrijven en deze lezen.

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de bronresourcegroep.  
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.
5. Selecteer de naam van de doelresourcegroep.  
6. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een opslagaccount van de ene naar de andere resourcegroep verplaatst. Tot nu toe hebt u te maken gehad met één opslagaccount of meerdere instanties van een opslagaccount. In de volgende zelfstudie ontwikkelt u een sjabloon met meerdere resources en meerdere resourcetypen. Sommige resources hebben afhankelijke resources.

> [!div class="nextstepaction"]
> [Afhankelijke resources maken](./resource-manager-tutorial-create-templates-with-dependent-resources.md)
